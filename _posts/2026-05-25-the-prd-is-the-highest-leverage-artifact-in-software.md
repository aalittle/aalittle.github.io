---
layout: post
title: "The PRD Is the Highest-Leverage Artifact in Software"
description: "Why great PRDs are finally within reach, why the prototype is not their replacement, and why both points sharpen when an agent is the reader."
date: 2026-05-25
---

I've always believed the PRD is the highest-leverage place to invest in good software. Clarity, quality, and a clear "why" up front are the best predictors of a great outcome. More on why I feel that way comes later. The trouble has been that getting there is hard and inconsistent. That's the part I see changing fast with AI. And it's important, because we're about to ask AI agents to read these documents too, and their judgment is questionable at best. This post is about why great PRDs are finally within reach, why the prototype is not their replacement, and why both points sharpen when an agent is the reader.

## What good looks like now

A few definitions. A PRD (Product Requirements Document) describes what a feature must do and why. NFRs (Non-Functional Requirements) describe how it must behave: load time, scale, availability, accessibility, failure modes. Personas describe who the feature is for.

Any great PRD I receive today will have a recognizable shape:

- A clear narrative on why we're building the feature and which persona it serves
- Numbered, traceable functional requirements
- A comprehensive NFR section covering performance, scale, reliability, and failure behavior
- Success metrics defined in numbers, not adjectives
- Explicit linkage back to the personas

When I read one of these as an engineer, I want to build it. I can see the customer and the outcome. I also know what "done" means, because the NFRs answer the questions that quietly determine whether a feature ships well or ships badly. How fast does it need to load? What scale does it need to serve? What happens when a technician loses signal halfway through a job? Too many of those questions tend to get answered in the eleventh hour of development. Now they're getting answered up front, while we still have time to design for the answers.

On my team, we've built Claude Code Skills that help us both evaluate PRDs against a rubric and generate strong ones from a well developed idea. The ideas and customer insight still come from the product team. The tools help us pressure-test those ideas, surface gaps, and work out ambiguity, thanks to the LLM's command of the English language. The result is consistent PRDs that are clear, where they used to be uneven.

## The prototype is a tool, not the spec

In parallel, there's strong momentum toward prototyping early. AI has made prototypes nearly free. You can click through a working sketch in an afternoon and learn things no document can teach you. You feel the interaction. You spot edge cases. You ask better questions. Prototypes are good and getting better.

But the prototype is an alignment tool, not a source of truth. A prototype shows what a feature looks like. It doesn't say:

- How fast it must load
- How many concurrent users it must support
- What happens when the network drops mid-transaction
- How it behaves under low memory or low battery
- What the accessibility contract is

The PRD says those things. Treat the prototype as one of several inputs that help write a better PRD, not as a replacement for the PRD itself. If the prototype becomes the input to development, we've taken a step backwards.

## Why this matters more for agents than for humans

Large language models are not deterministic. The output is statistical. You cannot guarantee a correct answer. The only lever you have is the clarity of the context you feed in, and the relationship is direct: cleaner context produces better output, fuzzier context produces plausible nonsense that compiles, runs, and is quietly wrong.

A good PRD is the best context you can hand an engineering agent:

- Numbered requirements give the agent something to map its work against
- NFRs give the agent acceptance criteria
- Success metrics give the agent something to validate its own output against
- Personas give the agent the audience model it needs to make trade-offs

The prototype, in my view, probably doesn't add much for the agent. The 2D designs plus the PRD likely cover what it needs. That's the piece I'd genuinely like to debate with people working deeper in the agentic space.

The real payoff arrives when the PRD is sharp enough to support an automated loop. The agent writes the code, runs it against the metrics and NFRs, finds the gaps, and iterates. That loop only works if the spec is clear enough to validate against. If the requirements are fuzzy, you've automated the production of code that nobody can confidently call done.

## A hat tip to an early manager

One of my early managers was Neil Erskine, who wrote a 1992 paper at McMaster called "The Usefulness of the Trace Assertion Method for Specifying Device Module Interfaces." Neil then brought this mathematical way of specifying software into our R&D team and used it to write requirements for embedded software we were building.

The rest of us were kids learning to write code. Neil was specifying software with mathematical precision and zero ambiguity. He could, and would, hand you a spec that left literally no question about what the code had to do. I think he secretly enjoyed watching us catch up.

Producing specs that good took a specialist, and reading them took one too. But if an agent could consume a spec at that level of precision, it could probably write near-perfect code. We might finally have a way to close that gap. Not by turning everyone into formal methods experts, but by letting AI tooling help us write PRDs that are unambiguous enough for agents to act on and clear enough for humans to keep reviewing. Two readers, one document. Neil would be pleased.

## What to do with this

If you're a PM, invest in the PRD. Use AI to find your gaps before engineering does.

If you're an engineer, push back when you get a PRD that doesn't define done. NFRs aren't optional. Ambiguity costs you twice: once when you guess wrong, and again when you rebuild.

If you're building agentic workflows, the quality ceiling of your agent's output is the quality of your context. Spend disproportionately on the inputs.

The better we specify, the closer we get to software that does exactly what we set out to build. That was always true. The difference now is that it's becoming achievable.
