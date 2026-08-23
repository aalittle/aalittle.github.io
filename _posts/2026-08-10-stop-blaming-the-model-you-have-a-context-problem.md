---
layout: post
title: "Stop Blaming the Model: You Have a Context Problem"
description: "The eight layers of context I bring to every phase of product development, and why the real win is building them into how your team works."
date: 2026-08-10
---

Over the last year, AI tools have created a new kind of product builder, someone who can take an idea from customer insight to shipped code in days. Most of the conversation is about the tools, and I think the more interesting question is the one underneath it. What separates the person who gets generic output from the model, and the person who gets something they can actually ship?

In my experience the answer is context. It's what you bring to the model, not the model itself. An LLM without context produces work that's plausible and generic, and an LLM with the right context produces work that fits your product, your customers, and your engineering standards. The model is necessary but it isn't sufficient, and the difference is what you feed it.

Over the past several months I've been building and refining what I call the context stack. It's a set of inputs I bring to every phase of product development, and when I include them I start at 70% to 80% quality on the first pass instead of 20%. It helps me create better PRDs, better designs, and more correct code. I'm not trying to teach the LLM things it already knows, but instead give it context that it **couldn't possibly know**.

Here's what's in the stack as it continues to evolve.

## The eight layers

1. **Personas, with real grit.** Standard prompts get generic designs. But when I prompt with "design a workflow for a second year HVAC technician working in a basement with poor connectivity, tablet in one hand, needing to capture parts for billing before leaving," I get something usable. Personas force real trade-offs instead of designing for an idealized user who doesn't exist.
2. **Existing product state.** Feed in your help documentation, your feature descriptions, and your current user flows. This stops the model from proposing groundbreaking ideas for features you shipped two years ago.
3. **SLOs and performance expectations.** When I include our service level objectives, the model stops proposing architectures that assume a fast, stable connection, and it starts building for offline sync, local caching, and error handling out of the box.
4. **Non-functional requirements.** Accessibility, compliance, observability, security, and localization rarely show up in user stories, but you can't ship without them. Putting them in the context stack means accessibility labels, contrast ratios, and logging are built in rather than bolted on late in code review.
5. **UX design system.** When the model knows your design system it stops inventing new UI patterns, and your design reviews shift from "this needs a total redesign" to "swap this component and fix the padding."
6. **Architectural patterns.** Code should fit the system you already have. Feeding in your architectural patterns forces the model to respect your module boundaries and use your existing state management instead of inventing a new one.
7. **Coding best practices.** Naming conventions, testing expectations, review criteria. When the model knows how your team writes code, diffs come back with functions named correctly and tests covering the edge cases your team actually cares about.
8. **PRD templates and examples.** Feed the model what good looks like in your org. Output comes back structured properly, with clear success metrics instead of vague fluff, and you save PMs and engineers a great deal of translation time.

## How the layers compound

Any single layer helps, but the real win is in the combination. Bring personas, SLOs, and architectural patterns into a single design session, and the model proposes a feature for a specific user, meeting performance targets, built on patterns your team already understands. That's three rounds of review feedback baked into the first draft.

Personas eliminate designed for nobody, SLOs eliminate works fine in a demo, and architectural patterns eliminate clever but doesn't fit our codebase.

## From individual prompts to team infrastructure

You don't need all eight layers on day one. Start with personas and product docs, then add layers wherever your drafts fall short.

But there's a step past just writing it down, which is making it structural. On my team the context stack isn't something an engineer has to remember to paste into a prompt. We package these layers as skills that install automatically when you clone the repo, and the key security, accessibility, and architectural constraints run as presubmit checks on pull requests. The context stops being a manual habit and becomes something the whole team inherits.

The models and the tools are available to everyone. The differentiator is the context you bring, and I would advocate that the real win comes when you stop bringing it by hand and build it directly into how your team works.
