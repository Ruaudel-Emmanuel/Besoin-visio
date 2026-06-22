# ROI Discovery Workshop for Client Calls

This project is a lightweight client-facing workshop tool designed for live video calls. It helps a consultant move from problem discovery to a first automation concept, then to a simple ROI estimate, without overwhelming the client with too much information at once. Progressive disclosure is a core design principle here: only the information needed for the current step is shown, which helps reduce cognitive load and keeps the conversation focused.\[web:142]\[web:146]

The project also includes a separate consultant guide. That separation matters because workshop facilitation guidance, secondary questions, reformulation prompts, and sales scripting are useful for the facilitator but should not necessarily be visible to the client during screen sharing.\[web:126]\[web:150]

## Project goal

The goal is to support a live discovery conversation, not to replace it. The client sees a clean, structured interface with one step at a time, while the consultant keeps deeper facilitation notes in a separate document.\[web:124]\[web:143]

The tool is designed to help answer three business questions during a call:

* What is the real operational problem?
* What is a realistic first automation step?
* Is that first step likely to pay for itself?

## Why this format

In remote workshops and discovery sessions, participants need to see what is being captured so they can validate or correct it in real time.\[web:143] At the same time, showing every note, every prompt, and every supporting sentence at once can create visual clutter and pull attention away from the actual conversation.\[web:118]\[web:149]

This project addresses that tension by splitting the experience into two layers:

* A **client-facing HTML screen** for the live call.
* A **consultant-only guide** for facilitation, reframing, and sales support.\[web:126]\[web:150]

## Core principles

### 1\. Progressive disclosure

The interface reveals the process step by step instead of exposing the full method at once. This approach is widely used to reduce overload, defer nonessential detail, and help users focus on what matters right now.\[web:141]\[web:142]\[web:146]

### 2\. Visible capture

The client should be able to see the key points being captured during the conversation. This improves alignment, reduces misunderstanding, and makes correction possible in real time.\[web:143]

### 3\. Separation of roles

The participant view and facilitator view should not be identical. A facilitator often needs extra prompts, sequencing notes, and fallback phrasing that would distract the participant if shown on screen.\[web:126]\[web:150]

### 4\. Business-first language

The tool is meant to support business conversations. It should stay grounded in operational reality, time savings, friction, quality of execution, and next-step decisions rather than technical jargon.

## What is included

### Client-facing HTML versions

The HTML files are designed to be opened locally or hosted on a website and shared during a live call. They guide the conversation through four main stages:

* Need analysis
* First solution framing
* ROI estimation
* Final summary

The lighter versions are optimized for screen sharing. They preserve the structure of the analysis while reducing visual noise and keeping the client focused on the current question.\[web:128]\[web:130]

### Consultant guide

The consultant guide is a separate written document. It contains:

* Main objective of each stage
* Primary and secondary questions
* Reformulation help
* Plain-language alternatives to technical terms
* Transition signals between stages
* Suggested talk tracks
* End-of-call checklist\[web:126]\[web:150]

## Current structure

Typical project files include:

```text
atelier-roi-visio-v2.html
atelier-roi-visio-v2-1.html
atelier-roi-visio-v2-1-complet.html
guide-consultant-atelier-roi-v2-1.md
```

Depending on the iteration, earlier standalone pages may also exist for:

* need diagnosis,
* first solution framing,
* ROI estimation.

## Recommended usage

### During a live call

1. Share the client-facing HTML screen.
2. Move through one step at a time.
3. Fill in the fields live with the client.
4. Read the short reformulation aloud.
5. Only move forward when the client agrees with the previous step.
6. End with the summary and the recommended next action.\[web:124]\[web:143]\[web:150]

### Before the call

* Prepare one example scenario.
* Test the page on the target hosting environment.
* Open the consultant guide separately.
* Mute distractions and prepare the sequence in advance, which is especially important in remote workshop settings.\[web:148]\[web:150]

### After the call

Use the captured material as the basis for:

* a proposal,
* a paid discovery session,
* a scoped version 1 automation project,
* or an internal recap document.

## Workshop flow

### Step 1 — Need analysis

This stage captures the company context, business environment, team, process, current workflow, tools, stakeholders, frequency, time spent, friction points, and business consequences. The purpose is to move from vague dissatisfaction to a clearly named operational problem.

### Step 2 — First solution

This stage reframes the problem into a first achievable solution. It captures the main goal, trigger, data sources, expected result, automatable work, human validation boundaries, tools to connect, version 1 deliverable, quick wins, perceived complexity, and out-of-scope items.

A compact **Before / After / Gains** block is used to make the transformation more concrete and easier to discuss live.

### Step 3 — ROI estimate

This stage uses simple inputs such as volume, current time per occurrence, future time per occurrence, hourly value, avoided error cost, and project cost. The goal is not to produce financial perfection, but to create a credible first-order estimate that supports a decision.

### Step 4 — Final summary

This stage consolidates the conversation into a readable summary that can support the next step: proposal, workshop, or scoped implementation.

## Design notes

This project intentionally avoids dense consultant dashboards in the client-facing view. Long pages with too much information can fragment attention, while progressive disclosure and staged layouts help keep the interaction more focused.\[web:118]\[web:149]

At the same time, hiding too much information can increase interaction cost and reduce clarity, so the design should be tested in real conversations and adjusted based on observed client behavior.\[web:144]\[web:149]

## Iteration strategy

A practical way to evolve the project is:

* Keep the client-facing screen visually calm.
* Preserve the analytical depth of the method.
* Break heavy stages into smaller internal sequences when needed.
* Move consultant-only support into the guide or into collapsed sections not shown during the call.\[web:141]\[web:144]\[cite:102]

## Good fit

This format is especially useful for:

* automation discovery calls,
* AI consulting discovery sessions,
* workflow scoping meetings,
* SME process review calls,
* pre-sales diagnosis before sending a proposal.

## Future improvements

Possible next steps for the project include:

* nested substeps inside the need-analysis stage,
* nested substeps inside the solution stage,
* better PDF export for client recaps,
* automatic generation of a proposal draft,
* CRM or Airtable integration,
* saving session outputs into a structured project record.\[web:104]\[web:121]

## License and customization

This project is intended as a practical workshop and sales-support foundation. It can be adapted to different sectors, process types, and automation offers by changing the wording, the data model, and the ROI assumptions.

