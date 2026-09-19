<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# README V3 — Session Conseil Automatisation / Atelier ROI

Ce document décrit la version actuelle de la session de conseil Rennesdev.fr, les choix techniques qui ont été faits, pourquoi ils ont été retenus, et ce qu'il faut pour reprendre le projet proprement par un développeur ou un LLM.[^1][^2][^3]

## Objectif du service

Le service vend une session de conseil d'une heure destinée aux dirigeants de PME et TPE qui veulent clarifier leurs besoins en automatisation avant de lancer un projet plus large. La promesse actuelle est simple : réserver, payer, choisir un créneau, tenir la visio, puis recevoir un document clair et exploitable.[^2][^3]

## Parcours client actuel

Le parcours actuel est structuré en séquence courte et sans friction :

1. Le client arrive sur la page de vente de la session.[^3]
2. Il réserve et règle la session en ligne via Stripe.[^1]
3. Il est redirigé vers une page de confirmation.[^2]
4. Il reçoit un email avec récapitulatif et facture HT.[^1][^2]
5. Il choisit son créneau via Calendly.[^2]
6. La visio a lieu sur Google Meet ou Zoom.[^2]
7. Il reçoit ensuite un document clair avec besoins, pistes d'automatisation, outils recommandés et priorités.[^2]

## Fichiers de référence connus

### 1. `Session-Conseil-Automatisation.html`

C'est la page commerciale de l'offre. Elle sert à expliquer la proposition de valeur, à rassurer, et à déclencher la réservation. Son rôle est commercial, pas technique.[^3]

### 2. `merci.html`

C'est la page de confirmation après paiement. Elle sert à guider immédiatement le client vers l'étape suivante, notamment le choix du créneau via Calendly. Elle sert aussi à réduire les frictions post-paiement avec des explications simples, des délais clairs et un point de contact humain.[^2]

### 3. `Booking-Conseil-Stripe-Confirmation.json`

C'est le workflow n8n qui reçoit les données de réservation via webhook, normalise quelques champs, puis envoie deux emails : un email de confirmation au client et une alerte interne à Emmanuel Ruaudel.[^1]

## Choix techniques réalisés

## 1. Pages statiques HTML

Le choix a été fait de travailler avec des pages HTML simples plutôt qu'avec un framework front complet. Ce choix est pertinent ici parce que le besoin principal est une page d'offre claire, rapide à modifier, légère à héberger et facile à maintenir sans chaîne de build complexe.[^3][^2]

### Pourquoi ce choix

- Mise en ligne rapide.
- Hébergement simple sur un site vitrine.
- Peu de dépendances.
- Très bon contrôle du wording marketing.
- Reprise facile par un développeur front ou un LLM.


### Limites

- Pas de gestion centralisée des composants.
- Risque de duplication si plusieurs pages évoluent en parallèle.
- Styling et wording potentiellement dispersés si le projet grossit.


## 2. Paiement via Stripe

Stripe a été retenu comme point d'entrée financier du tunnel. Le choix est cohérent parce qu'il gère le paiement, la facture HT, la traçabilité de la transaction et l'image de sérieux du parcours client. Le test à 1 euro a confirmé que les paiements remontent bien et que la chaîne d'alerte fonctionne.[^1][^2]

### Pourquoi ce choix

- Paiement sécurisé et connu.
- Facturation automatisée.
- Bonne expérience utilisateur.
- Facilité d'intégration dans un tunnel no-code / low-code.


### Point de vigilance

Le README doit toujours préciser où se fait la redirection après paiement, quel événement Stripe déclenche réellement le webhook n8n, et quelles métadonnées sont attendues dans la charge utile. Le JSON actuel montre les champs `email`, `nom`, `type_session`, `booking_id`, `stripe_session_id`.[^1]

## 3. Orchestration via n8n

Le choix de n8n est central dans ce projet. Le workflow actuel s'appuie sur un webhook, un nœud de transformation de données et deux nœuds d'envoi d'emails. Cette architecture est volontairement simple et lisible.[^1]

### Pourquoi ce choix

- Très adapté à l'automatisation d'un tunnel de réservation.
- Lisible visuellement.
- Rapide à tester.
- Facile à faire évoluer vers Airtable, Notion, CRM ou génération documentaire.
- Compatible avec la logique de Rennesdev.fr centrée sur l'automatisation métier.


### Structure actuelle du workflow

- `Webhook` : reçoit l'appel HTTP POST sur `booking-confirmed`.[^1]
- `Edit Fields` : extrait et normalise les champs utiles du payload.[^1]
- `Send an Email` : confirmation envoyée au client.[^1]
- `Send an Email1` : notification interne envoyée à Emmanuel.[^1]


### Pourquoi cette structure simple est bonne

Elle permet de déboguer vite, de comprendre le flux sans effort et de limiter les points de panne. Pour une V1 ou V2 commerciale, la simplicité du workflow vaut souvent mieux qu'une sophistication prématurée.[^1]

## 4. Double email : client + interne

Le système envoie à la fois un email au client et une alerte interne. Ce choix est très pertinent commercialement. Le client est rassuré immédiatement, tandis qu'Emmanuel garde la main sur le suivi humain du dossier.[^1]

### Pourquoi ce choix

- Le client sait que son paiement est pris en compte.[^1]
- Le prospect voit une suite claire.[^2]
- Emmanuel est alerté en temps réel.[^1]
- Le tunnel n'est pas purement automatisé au point de devenir froid.


### Limite actuelle

Le contenu actuel du mail indique encore qu'Emmanuel contactera le client sous 24h pour caler le créneau, alors que la page `merci.html` pousse déjà vers un choix autonome via Calendly. Il y a donc un léger décalage à corriger entre le message email et l'expérience réelle.[^2][^1]

## 5. Calendly pour la prise de rendez-vous

Calendly est utilisé pour le choix du créneau après paiement. Le choix est bon car il supprime les allers-retours manuels et rend la réservation immédiatement exploitable.[^2]

### Pourquoi ce choix

- Disponibilités visibles immédiatement.
- Zéro échange manuel pour fixer un rendez-vous.
- Expérience plus fluide pour un prospect froid.
- Très bonne cohérence avec la promesse d'automatisation.


### Point de vigilance

Le README doit documenter l'URL exacte du calendrier, les règles de fuseau horaire, la durée par défaut, les buffers éventuels et ce qui déclenche l'envoi du lien Meet ou Zoom. La page actuelle mentionne un lien Calendly public vers `session-conseil-1h`.[^2]

## 6. Google Meet ou Zoom pour la visio

Le choix de ne pas enfermer la visio dans une solution unique apporte de la souplesse. Cela dit, le canal effectif doit être documenté plus précisément pour éviter les ambiguïtés lors d'une reprise.[^2]

## 7. Promesse de livrable après la session

Le tunnel ne vend pas seulement une heure de visio. Il vend aussi un résultat tangible : un document clair avec automatisations identifiées, outils recommandés et priorités. Cette promesse justifie une partie du prix et distingue l'offre d'un simple appel découverte.[^2]

### Extension métier envisagée

Dans l'évolution actuelle du service, des livrables complémentaires sont également évoqués : prompt adapté, JSON métier, n8n preview et rapport client. Cette dimension doit être décrite comme un livrable de cadrage ou d'extraction, pas comme un développement complet.[^3]

## Décisions produit derrière les choix techniques

### 1. Réduire les frictions

Le parcours a été pensé pour supprimer un maximum d'étapes manuelles : paiement, facture, confirmation, réservation. C'est cohérent avec la cible TPE/PME qui veut un service simple, lisible et rapide.[^2][^1]

### 2. Rassurer vite

Le client doit recevoir très vite un signal de confiance après paiement. C'est le rôle combiné de Stripe, de la page merci et de l'email de confirmation.[^2][^1]

### 3. Garder une posture humaine

Même si le système est automatisé, la promesse reste incarnée. Les textes gardent un ton humain, parlent du quotidien du client et évitent le jargon technique.[^3][^2]

### 4. Permettre l'industrialisation progressive

L'ensemble a été pensé pour pouvoir être enrichi petit à petit : ajout d'un CRM, d'un stockage structuré, d'un rapport généré, d'un scoring des prospects ou d'une préqualification. n8n facilite cette montée en puissance.[^1]

## Incohérences et points à corriger

### 1. Décalage email / page merci

L'email client promet encore une prise de contact sous 24h pour caler le créneau, alors que la page merci invite déjà à réserver immédiatement. Ce point doit être harmonisé.[^2][^1]

### 2. Nommage de la session et du prix

Le projet a déjà connu plusieurs versions de wording et de prix. Le README doit imposer une source de vérité unique pour :

- nom public de l'offre,
- promesse commerciale courte,
- prix de lancement,
- prix standard,
- durée,
- livrables inclus.


### 3. Données minimales non formalisées

Le JSON n8n laisse voir les champs minimums attendus, mais le contrat de données n'est pas encore formalisé sous forme de schéma. Cela peut gêner une reprise propre par un développeur ou un LLM.[^1]

## Ce qu'il faut documenter pour une reprise par un développeur

Un développeur doit pouvoir reprendre le projet sans relire tous les échanges. Il faut donc documenter clairement :

### Environnement fonctionnel

- URL publique de la page d'offre.
- URL de la page merci.
- URL du paiement Stripe ou mode de génération du checkout.
- URL du Calendly.
- Canal de visio final.
- Adresse email d'envoi.


### Contrat de données

- Champs attendus dans le webhook.
- Exemple de payload réel.
- Champs obligatoires vs optionnels.
- Normalisation des noms de session.
- Mapping entre Stripe, page merci et n8n.


### Dépendances externes

- Compte Stripe.
- Instance n8n.
- SMTP.
- Calendly.
- Hébergement du site.


### Règles métier

- Quand un paiement est considéré comme valide.
- Ce qui se passe si l'email n'est pas envoyé.
- Ce qui se passe si Calendly n'est pas réservé.
- Politique d'annulation ou de report.
- Délai de livraison du rapport après la session.


## Ce qu'il faut documenter pour une reprise par un LLM

Un LLM reprend mieux un projet si les éléments sont organisés en blocs très explicites.

### Fournir au minimum

- Une carte du parcours client en 5 à 10 étapes.
- Une liste des fichiers et de leur rôle.
- Une liste des outils tiers et de leur fonction.
- Les noms exacts des pages, workflows et livrables.
- Les textes sources de référence.
- Les contraintes à ne pas casser.


### Contraintes à poser noir sur blanc

- Le ton doit rester simple, commercial et orienté dirigeant PME/TPE.
- Le client ne doit pas voir de jargon technique inutile.
- Le tunnel doit rester simple à maintenir.
- Le paiement et la réservation doivent rester séparés mais fluides.
- Toute modification du wording doit être cohérente entre page de vente, page merci, emails et LinkedIn.


### Format conseillé pour aider un LLM

Prévoir dans le dépôt :

- un fichier `README.md` global,
- un fichier `ARCHITECTURE.md`,
- un fichier `DATA-CONTRACT.md`,
- un dossier `copy/` avec les textes de référence,
- un dossier `workflows/` avec les exports n8n,
- un dossier `pages/` avec les HTML,
- un fichier `CHANGELOG.md`.


## Arborescence cible recommandée

```text
session-conseil/
├── README.md
├── ARCHITECTURE.md
├── DATA-CONTRACT.md
├── CHANGELOG.md
├── copy/
│   ├── offre.md
│   ├── email-client.md
│   ├── email-interne.md
│   └── linkedin-posts.md
├── pages/
│   ├── session-conseil-automatisation.html
│   └── merci.html
├── workflows/
│   └── booking-conseil-stripe-confirmation.json
└── docs/
    ├── livrables-client.md
    └── process-interne.md
```


## Contrat de données minimal recommandé

Voici la base minimale qui devrait être formalisée pour éviter toute ambiguïté lors d'une reprise :[^1]

```json
{
  "email": "client@test.fr",
  "nom": "Jean Dupont",
  "type_session": "conseil_1h",
  "booking_id": "booking_abc123def456",
  "stripe_session_id": "cs_test_abc123"
}
```


## Recommandations V3

### 1. Aligner tous les messages

Mettre à jour les emails pour qu'ils correspondent exactement au tunnel réel : paiement validé, choix immédiat du créneau, puis visio.[^2][^1]

### 2. Centraliser la copy

Le wording commercial ne doit plus vivre seulement dans les pages HTML. Il doit être centralisé dans des fichiers de référence pour éviter les divergences entre site, mails et posts.[^3][^2]

### 3. Ajouter une trace structurée

Chaque réservation devrait idéalement être stockée dans Airtable, Notion ou une base légère pour assurer le suivi commercial, les relances et les statistiques.

### 4. Préparer la génération des livrables

Le projet est prêt pour une phase suivante : transformer la session de conseil en système semi-produit avec génération assistée du rapport, du prompt, du JSON métier et du squelette n8n.

### 5. Prévoir une documentation d'exploitation

Il manque encore une documentation simple du type : comment tester, comment simuler un paiement, comment vérifier les emails, quoi faire si un client paie mais ne réserve pas son créneau.

## Résumé opérationnel

La solution actuelle est bonne parce qu'elle est simple, crédible et déjà testée en réel : page d'offre, paiement Stripe, page merci, Calendly, emails et promesse de livrable. Elle a été conçue pour vendre une session de cadrage à forte valeur perçue avec un tunnel léger et pragmatique.[^3][^2][^1]

Pour une reprise propre par un développeur ou un LLM, la priorité n'est pas de refaire la technique mais de mieux documenter les contrats, harmoniser les messages et centraliser les contenus.[^3][^2][^1]

<div align="center">⁂</div>

[^1]: Booking-Conseil-Stripe-Confirmation.json

[^2]: merci.html

[^3]: Session-Conseil-Automatisation.html

