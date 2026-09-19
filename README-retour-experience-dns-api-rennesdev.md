# README — Retour d’expérience DNS, API FastAPI et prévention des faux bugs CORS

## Objectif

Ce document sert de garde-fou pour éviter de perdre plusieurs heures sur un problème déjà connu : une API FastAPI fonctionnelle en local et sur le VPS, mais inaccessible publiquement à cause d’un sous-domaine `api.rennesdev.fr` qui pointe encore vers l’ancien hébergement mutualisé au lieu du VPS OVH.[file:27][file:33][file:38]

Le symptôme visible côté navigateur ressemblait à un bug CORS ou à un endpoint cassé, alors que le vrai problème était un mélange de DNS, de cache DNS et d’ancien hébergement Apache encore actif sur le sous-domaine API.[file:36][file:38][file:41]

## Architecture retenue

Les choix d’architecture sont volontaires et doivent rester clairs pour toute future maintenance :

- `rennesdev.fr` et `www.rennesdev.fr` restent hébergés chez Infomaniak pour le site vitrine.[file:38][file:41]
- `api.rennesdev.fr` pointe vers le VPS OVH `162.19.246.165`, où l’API FastAPI tourne derrière Nginx et Uvicorn sur `127.0.0.1:8001`.[file:27][file:34][file:41]
- `n8n.rennesdev.fr` pointe aussi vers ce VPS OVH pour exécuter les workflows n8n en HTTPS.[file:27][file:37][file:41]
- Le flux métier visé est : page front RennesDev → `POST /api/v1/payments/session-conseil` → Stripe Checkout → webhook FastAPI → webhook n8n `booking-confirmed` → email client et notifications.[file:27][file:37]

Cette séparation est saine : la vitrine peut rester sur un hébergement mutualisé simple, tandis que les briques applicatives techniques (`api.` et `n8n.`) vivent sur un VPS dédié, plus adapté au reverse proxy, aux webhooks et à l’automatisation.[file:33][file:34]

## Le problème rencontré

Le backend FastAPI fonctionnait correctement :

- le service `api-rennesdev.service` tournait bien ;[file:27]
- l’endpoint local `http://127.0.0.1:8001/api/v1/payments/session-conseil` répondait ;[file:27]
- la création de session Stripe Checkout fonctionnait ;[file:27][file:42]
- le middleware CORS autorisait déjà `https://rennesdev.fr` et `https://www.rennesdev.fr` dans `main.py`.[file:36]

Pourtant, depuis le site public, le navigateur affichait une erreur CORS du type :

- absence de `Access-Control-Allow-Origin` ;
- échec de préflight `OPTIONS` ;
- `Failed to load resource` sur `https://api.rennesdev.fr/api/v1/payments/session-conseil`.[file:36]

La vraie cause était la suivante : `api.rennesdev.fr` pointait encore vers l’hébergement Infomaniak `185.125.27.133`, en environnement `Apache | PHP`, au lieu de pointer vers le VPS OVH `162.19.246.165`.[file:38][file:39]

En clair, le front appelait bien `https://api.rennesdev.fr/...`, mais cette URL publique ne tombait pas sur FastAPI. Elle tombait sur un ancien serveur Apache qui renvoyait un `404 Object not found` sans les en-têtes CORS attendus.[file:38][file:41]

## Pourquoi ce bug fait perdre du temps

Ce cas est trompeur parce qu’il donne plusieurs faux indices :

- le service systemd est vert ;[file:27]
- `curl` en local sur `127.0.0.1:8001` marche ;[file:27]
- Nginx côté VPS peut être correctement configuré ;[file:33][file:34]
- le code FastAPI et CORS peuvent déjà être justes ;[file:36]
- mais le domaine public continue d’aller ailleurs à cause du DNS.[file:38][file:41]

Résultat : on pense corriger un bug applicatif, alors qu’on est en train de diagnostiquer un bug d’aiguillage réseau.

## Signaux d’alerte à connaître

Si l’un des symptômes suivants apparaît, penser **DNS avant code** :

- `curl http://127.0.0.1:8001/health` répond `{"status":"ok"}` mais `curl https://api.rennesdev.fr/health` répond du HTML ;[file:27][file:41]
- le header serveur public annonce `Apache` alors que la stack attendue est Nginx + Uvicorn ;[file:38][file:41]
- le navigateur parle de CORS alors que FastAPI contient déjà `CORSMiddleware` ;[file:36]
- `dig +short api.rennesdev.fr` ne renvoie pas la même IP que `dig +short api.rennesdev.fr @1.1.1.1` ou `@8.8.8.8` ;[file:41]
- la route `/health` ne renvoie pas le JSON attendu publiquement alors qu’elle répond bien en local.[file:27][file:34]

## Diagnostic qui a permis d’isoler le vrai problème

Les commandes suivantes ont permis de sortir du faux diagnostic CORS :

```bash
curl -i -X OPTIONS "https://api.rennesdev.fr/api/v1/payments/session-conseil" \
  -H "Origin: https://rennesdev.fr" \
  -H "Access-Control-Request-Method: POST" \
  -H "Access-Control-Request-Headers: content-type"
```

Cette requête ne montrait pas les en-têtes CORS attendus et renvoyait une réponse servie par Apache, ce qui était incompatible avec la stack FastAPI attendue.[file:36][file:38]

Ensuite, les commandes suivantes ont montré la propagation DNS incomplète :

```bash
dig +short api.rennesdev.fr
dig +short api.rennesdev.fr @1.1.1.1
dig +short api.rennesdev.fr @8.8.8.8
```

Le résolveur par défaut renvoyait encore `185.125.27.133`, tandis que Cloudflare et Google voyaient déjà `162.19.246.165`. C’était la preuve d’une propagation en cours, pas d’un bug de code.[file:41]

Enfin, cette commande a validé que le VPS répondait correctement avant la fin de propagation DNS :

```bash
curl -k --resolve api.rennesdev.fr:443:162.19.246.165 https://api.rennesdev.fr/health
```

Le retour `{"status":"ok"}` a confirmé que la conf Nginx + FastAPI sur le VPS était correcte et que le dernier frein était bien la résolution DNS publique.[file:34]

## Correction appliquée

La correction mise en place est la suivante :

| Élément | Mauvaise valeur | Bonne valeur |
|---|---|---|
| `api.rennesdev.fr` `A` | `185.125.27.133` (Infomaniak) [file:38] | `162.19.246.165` (VPS OVH) [file:41] |
| `api.rennesdev.fr` `AAAA` | ancien IPv6 mutualisé, à supprimer si non utilisé [file:38][file:34] | vide si pas d’IPv6 configurée sur le VPS [file:34] |
| `rennesdev.fr` | reste sur Infomaniak [file:38][file:41] | inchangé [file:41] |
| `www.rennesdev.fr` | reste sur Infomaniak [file:41] | inchangé [file:41] |
| `n8n.rennesdev.fr` | `162.19.246.165` [file:37][file:41] | inchangé [file:41] |

Après propagation complète, `api.rennesdev.fr` renvoie bien vers le VPS OVH et `/health` répond en JSON publiquement.[file:41][file:34]

## État final attendu

Quand tout est bon, les tests de référence doivent donner ceci :

```bash
dig +short api.rennesdev.fr
# attendu : 162.19.246.165

dig +short api.rennesdev.fr @1.1.1.1
# attendu : 162.19.246.165

dig +short api.rennesdev.fr @8.8.8.8
# attendu : 162.19.246.165

curl -k https://api.rennesdev.fr/health
# attendu : {"status":"ok"}
```

Et côté navigateur :

- le bouton de réservation appelle bien `POST https://api.rennesdev.fr/api/v1/payments/session-conseil` ;[file:27][file:31]
- l’API renvoie un JSON avec `checkout_url` ;[file:27]
- l’utilisateur est redirigé vers Stripe Checkout ;[file:42]
- après paiement, le webhook Stripe appelle FastAPI, puis FastAPI appelle le webhook n8n `booking-confirmed`.[file:27][file:37]

## Règle de prévention pour les prochaines mises en production

Avant de chercher un bug CORS, Stripe, Nginx ou FastAPI, toujours faire cette checklist dans cet ordre :

1. **Tester l’app en local VPS**
   ```bash
   curl http://127.0.0.1:8001/health
   ```
2. **Tester le vhost local via l’IP forcée**
   ```bash
   curl -k --resolve api.rennesdev.fr:443:162.19.246.165 https://api.rennesdev.fr/health
   ```
3. **Comparer la résolution DNS réelle**
   ```bash
   dig +short api.rennesdev.fr
   dig +short api.rennesdev.fr @1.1.1.1
   dig +short api.rennesdev.fr @8.8.8.8
   dig +short AAAA api.rennesdev.fr
   ```
4. **Tester la réponse publique**
   ```bash
   curl -k https://api.rennesdev.fr/health
   ```
5. **Seulement après ça**, analyser le code, CORS, Stripe ou le front.

Cette séquence évite de perdre du temps sur le mauvais étage du problème.

## Décision d’architecture à conserver

La décision à garder pour la suite est simple :

- le site vitrine reste chez Infomaniak ;[file:38][file:41]
- l’API FastAPI et n8n restent sur le VPS OVH ;[file:27][file:37][file:41]
- chaque sous-domaine technique (`api.`, `n8n.`, plus tard éventuellement `webhook.` ou `app.`) doit être validé indépendamment en DNS avant toute mise en prod front.[file:33][file:34]

Ce découpage est plus propre, plus maintenable et plus sûr qu’un mélange flou entre hébergement mutualisé et endpoints applicatifs.[file:33][file:34]

## Phrase mémo à garder

Si `127.0.0.1` marche mais que le domaine public renvoie Apache, **ce n’est probablement pas un bug FastAPI**. C’est d’abord un problème de DNS ou de propagation DNS.
