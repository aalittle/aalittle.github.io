---
layout: post
title: "The Circularity of Error: Why Agentic Dev Needs a Verification Loop"
description: "A green test suite only proves the code agrees with the tests. Here's the four-part framework we built to break the self-consistency trap."
date: 2026-07-06
---

An agent writes the story, the code, and the tests. The suite goes green. What a great feeling.

But what did that actually prove?

It proved the code agrees with the tests. That is not the same as proving the code is correct.

If an AI agent misreads a requirement, it will write tests that confirm its misreading, then build the code to pass them. Internally consistent, externally wrong.

Researchers at York University recently put a number on this: only 35 to 51% of LLM-generated tests are valid, because the model uses its own implementation as the oracle rather than the intended specification. They call it *circularity of error*.

Most teams scaling agentic development haven't solved this yet. At least, there isn't much public discourse on such a critical question. Here is how we break the self-consistency trap, using human judgment and agentic looping to meet a clear definition of "done."

## The shape problem came first

Before we could build a verification loop, we had to fix a structural problem in our test coverage.

We had strong unit test coverage and hundreds of E2E tests, but almost nothing in between. What's worse, our E2E tests lacked reliability. That isn't a testing pyramid; it's an hourglass with sand getting stuck in its neck.

The gap existed because the tooling to write integration tests easily just didn't exist. When an AI agent inherits an hourglass architecture, it makes the hourglass worse, faster. We had to fix the shape before we could trust the output.

## The four pieces of our verification framework

### 1. SLOs define done

We defined Service Level Objectives (SLOs) for every expected performance indicator in the application experience: launch time, sync time, screen transitions, and offline behavior. Engineers and agents now share one definition of what "great" looks like. When an agent picks up a story, acceptance is no longer a vibe. It is a concrete number the agent can test against and iterate toward.

### 2. A quality strategy rebuilt for cheap tests

Our old strategy assumed tests were expensive to write. They aren't anymore, so we rewrote the strategy around that fact. We defined exactly which tests we want at which layers:

- Q1: Unit tests
- Q2: Integration and headless API tests
- Q3: Feature-level E2E
- Q4: Performance

Every PR gates on Q1 and Q2 (targeting a 99.9% pass rate). Q3 runs daily and at release (targeting 95%). Our agents are explicitly informed of this strategy.

### 3. Frameworks agents can write to

We built a new integration test framework featuring headless API tests and cross-layer integration tests. Both are structured so an agent can learn the pattern and produce correct tests without hand-holding. For E2E, we adopted Maestro, driving everything off YAML.

Agents write these tests fast, they port easily, and they run highly parallelized on real or virtual devices. We are writing tests at a pace we have never hit before, covering layers we never could before. At maturity, we expect E2E execution to be 2x faster, scaling integration tests from dozens to thousands by the end of the year.

### 4. A human validates the test plan

This is the piece that breaks the self-consistency trap. The agent generates the test plan first: acceptance criteria, error handling, offline behavior, accessibility, and edge cases.

Before the build starts, a human reviews that plan. We don't review the test code; we review the test intent. Does this plan actually verify the requirement, or does it verify the agent's misreading of it?

At the epic level, we review the collection of story-level plans together to catch gaps between stories (e.g., Does offline work end-to-end? Are all integration points covered?).

Tests written from validated intent verify behavior. Tests written from unvalidated intent verify assumptions. The review takes only a few minutes, but it requires rigorous attention.

## Looping to success

When you put these four pieces together, the agent's inner loop looks like this: Plan -> Build -> Test -> Validate against SLOs -> Repeat until the numbers pass.

Only then does the agent call the story done and open a PR. The engineer walks away knowing the work meets expectations, because those expectations are written down as SLOs and a human-validated test plan.

This keeps human review narrow but high-impact. We review the agent's development plan, the test plan, and the resulting code.

## The results so far

Effective output (using the Stanford measure) per engineer is tracking at 1.88x since we started measuring.

Our target is 3x, and the constraint is no longer code generation. It is feeding the agents enough well-specified work and context.

We expect our maintainability score to rise 15% to 25% by the end of the year because, for the first time, our test coverage will be growing faster than our codebase.

## The takeaway

Writing code got fast. Quality does not come along for the ride without explicit thought and intention.

If your team is scaling agentic development, the question to ask is not how much code your agents can produce. It is this: how do you verify quality?

Build the verification loop before you scale the generation. The rigor is what makes the speed worth having.
