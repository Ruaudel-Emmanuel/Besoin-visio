# Atelier ROI Visio — Evolution from V2.1 to V2.8

This document explains how the Atelier ROI Visio interface evolved from version 2.1 to version 2.8, and why each change was introduced.

The project did not evolve only to add features. Each version was designed to solve a real usage problem observed during client-facing video calls: too much information on screen, weak separation between consultant view and client view, insufficient print quality, or a visual presentation that was still too close to an internal tool instead of a premium advisory deliverable.

## Product intent

The original idea behind the tool is simple: during a discovery call, the consultant should be able to clarify the client need, shape a first solution direction, estimate a rough ROI, and turn the conversation into reusable outputs.

The challenge is that the tool is used live, on screen, in front of a prospect. That changes everything. A form that works for internal note-taking is not automatically suitable for a shared screen. A technical export is not automatically a good client deliverable. A usable interface is not automatically a premium-looking consulting artifact.

That is why the evolution from V2.1 to V2.8 follows a clear logic:

- Reduce visual overload during calls.
- Separate consultant logic from client-facing presentation.
- Preserve work through local autosave.
- Turn the interview into downloadable outputs.
- Add a premium printable report.
- Improve perceived value through visual direction and theme control.

## Version 2.1 — Lightweight screen-first experience

Version 2.1 focused on simplification.

The main goal was to make the screen easier to share during a call. Earlier thinking showed that if too much information is visible at once, the client starts reading instead of talking. That breaks the flow of the discovery session. V2.1 therefore reduced the visible content to the essentials: one step at a time, one main question, only the fields needed in the moment, and a short reformulation block to keep the conversation structured.

### Why this mattered

The tool was no longer treated as a generic form. It became a conversation support interface.

That shift was important because the objective during a live call is not to impress with completeness. It is to guide the conversation, avoid confusion, and keep the client focused on business issues rather than on UI complexity.

## Version 2.4 — Local autosave and direct file downloads

Version 2.4 introduced two practical improvements:

- local autosave,
- direct download of the three extraction files.

Autosave was added because losing a long discovery draft after a browser refresh or accidental close would immediately damage trust and waste time. A multi-step interview needs persistence, especially when the call itself is producing valuable structured information.

Direct downloads were added because the session was already producing usable outputs:

- a project extraction JSON,
- an LLM prompt,
- an n8n preview JSON.

### Why this mattered

This version moved the tool from “guided conversation support” to “guided conversation support with reusable artifacts.”

That was a major step because it connected the live discovery session to actual downstream delivery. Instead of ending the call with notes trapped in the interface, the consultant could leave with files that could feed internal work, future automation, or AI-assisted drafting.

## Version 2.5 — Grouped export and consultant/client modes

Version 2.5 responded to two important realities.

First, exporting several files one by one was clunky. A grouped export made more sense, especially for post-call handling. Second, the same interface could not serve both the consultant and the client equally well at the same time.

This version introduced:

- grouped export through a single package,
- consultant mode,
- client mode,
- a cleaner extraction area focused on downloading files instead of displaying raw technical outputs.

### Why this mattered

This was a turning point in role separation.

The consultant needs detailed fields, optional technical controls, and visibility into machine-readable outputs. The client does not. When those are displayed during a call, the experience can become noisy, intimidating, or unnecessarily technical.

By separating consultant mode from client mode, the tool became much more appropriate for screen-sharing. It also made the interface feel more intentional. The client-facing state became calmer, cleaner, and more reassuring.

Removing raw output previews from the extraction page followed the same logic. If files are meant to be downloaded, there is no strong reason to expose their internal content during the conversation. That content is useful for operations, not for trust-building during a discovery discussion.

## Version 2.6 — Premium printable client report separated from input UI

Version 2.6 introduced one of the most important conceptual upgrades: the printable premium client report became a separate view.

Instead of trying to print the input interface itself, the tool gained a dedicated report layout designed specifically for printing and PDF export.

This version added:

- a separate report view,
- print-oriented layout rules,
- a dedicated report toolbar,
- a more presentable report structure with sections and cover logic,
- a report export path independent from the data entry UI.

### Why this mattered

A data-entry interface and a client-ready document are not the same thing.

Printing the working interface almost always creates a weak result: navigation controls, tool buttons, and layout constraints designed for interaction pollute the final document. By separating the report from the input UI, the project stopped behaving like an internal app and started behaving like a consulting tool that produces a deliverable.

This change improved perceived professionalism. Instead of sharing “the filled form,” the consultant could share “the report.”

## Version 2.7 — Higher-end visual direction and living tab menu

Version 2.7 focused on visual perception.

At this point, the tool already had structure, persistence, exports, and a dedicated report. The next issue was perceived value. The interface needed to look less like a practical internal utility and more like something aligned with premium consulting or studio work.

This version introduced:

- a more premium visual direction,
- a more editorial hero section,
- a stronger premium report cover,
- a “living” tab menu effect with controlled vertical motion.

### Why this mattered

Clients judge quality very quickly from visual cues.

Even if the underlying workflow is good, a flat or generic interface weakens trust and lowers perceived positioning. The visual upgrade helped express a more high-end identity.

The tab menu update addressed a specific UX and visual problem. The request was not simply to keep the tabs rigidly aligned. The goal was to create a menu that felt alive and premium, with a subtle vertical movement similar to editorial composition, without becoming chaotic. The result was a stable track with controlled vertical offsets, so the menu gained personality while remaining readable and structurally sound.

## Version 2.8 — Three selectable premium themes and wide desktop layout

Version 2.8 expanded the visual system by introducing three selectable premium themes:

- Cabinet,
- Studio,
- Premium Industrial.

It also adapted the layout for a desktop-only context, allowing the top titles to become wider and more expressive.

### Why this mattered

Once the tool started to act as both a working interface and a client-facing experience, visual direction became strategic.

Different prospects may respond better to different tones. A conservative SME may prefer a reassuring “Cabinet” look. A more design-sensitive client may respond better to “Studio.” A more process-driven company may feel more comfortable with “Premium Industrial.”

Allowing theme switching made the tool more flexible without rebuilding its structure.

The wide desktop layout was also important because the usage context was clarified: this HTML page is intended for PC display only. That meant the layout no longer needed to compromise for narrow screens. Wider headings, more breathing room, and larger visual composition became not only acceptable, but desirable.

## Strategic reading of the whole evolution

From V2.1 to V2.8, the product followed a very coherent path.

It started by reducing cognitive load. Then it protected the work through autosave. Then it turned outputs into downloadable assets. Then it separated consultant and client visibility. Then it separated the printable report from the working interface. Then it elevated visual perception. Finally, it introduced theme-driven positioning.

This is not feature accumulation for its own sake.

It is the progressive transformation of a discovery-call helper into a higher-value advisory interface that can:

- support a live conversation,
- reassure a prospect,
- produce reusable technical outputs,
- generate a clean client-facing report,
- and visually support premium positioning.

## What these evolutions reveal

The evolution from V2.1 to V2.8 reveals four product principles.

### 1. Live usage matters more than theoretical completeness

The interface is used during calls, not in isolation. Therefore, clarity, rhythm, and emotional reassurance matter as much as data capture.

### 2. Different audiences need different layers of visibility

The consultant needs operational depth. The client needs confidence, clarity, and structure. Mixing both layers in the same visible state weakens the experience.

### 3. Outputs are part of the value proposition

The session should not end with captured information only. It should produce assets that can be reused immediately: structured data, prompts, workflow previews, and a client-ready report.

### 4. Visual quality influences perceived expertise

A better-looking interface does not replace substance, but it helps the substance feel credible, premium, and intentional. In consulting contexts, this matters.

## Closing note

The journey from V2.1 to V2.8 is the story of a tool becoming more specific to its real job.

At first, it mainly supported structured discussion. By V2.8, it had become a fuller advisory surface: part live interview guide, part reusable extraction engine, part premium client report generator, and part positioning tool.

That is why each evolution matters. Each version reduced friction, clarified roles, increased deliverable quality, or strengthened perceived value.
