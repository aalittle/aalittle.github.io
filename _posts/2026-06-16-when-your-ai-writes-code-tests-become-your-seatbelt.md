---
layout: post
title: "When Your AI Writes Code, Tests Become Your Seatbelt"
description: "Adding snapshot tests, service tests, and Maestro E2E flows to a small iOS app in one afternoon, and writing the documentation an AI maintainer actually reads."
date: 2026-06-16
---

I have a small, yet unreleased, iOS app called Daily Pulse — a daily self-check-in tool lets me keep an eye on my wellbeing. Important in the age of AI. I always have a small project to practice and learn. The project had 181 unit tests covering models, services, and accessibility. But it had zero visual regression tests and zero end-to-end tests.

I'd been meaning to fix that for months (I hadn't worked on it in months). I sat down yesterday and did it all (with Claude): snapshot testing infrastructure, 29 visual regression tests across every major view, notification service coverage, 7 Maestro E2E flows, and documentation to keep it all maintainable. I thought I'd share how as I feel the tests will save us when it comes to developing with AI.

## The Starting Point

Daily Pulse is a single-screen-per-question check-in app. You answer 9 questions about sleep, movement, mood, focus, and executive function. There's an onboarding flow, a trends dashboard with charts and insights, and a widget extension.

The existing test suite was solid for logic — data model validation, correlation engine math, trend calculations, accessibility label coverage. All written with Swift Testing (`@Suite`, `@Test`, `#expect`), which I'd migrated to earlier this year.

What was missing:

- **No visual regression tests.** If someone changed a color token or broke a layout, only a manual test would catch it.
- **No end-to-end tests.** The entire multi-screen check-in flow had never been tested as a user would experience it.
- **No notification tests.** The reminder scheduling service was completely untested.

## Writing Tests for an AI Maintainer

One thing I've been thinking about is who actually maintains these tests now. It's me, but most of the typing is Claude, and that changes what "maintainable" means. When a model is writing a lot of code quickly, the tests are the thing standing between you and a regression you didn't notice, so the suite isn't a nice-to-have anymore, it's the seatbelt. That's really what drove the investment.

But a test suite an AI writes will rot just as fast as one a human writes if the next session doesn't know the rules. So the most valuable file I added wasn't a test at all, it was `DailyPulseTests/CLAUDE.md`. I think of it as scar tissue. Every time we hit something non-obvious, which view can't be snapshot tested because it needs a live model container, how to add a new file to the Xcode project, the concurrency requirements, it goes in that file as a rule. The next time Claude opens the project it reads the scar tissue first and doesn't make the same mistake twice, and neither do I.

The decision table is the center of it. When the rule for "unit vs snapshot vs E2E" is written down in one place, Claude picks the right layer on its own instead of guessing, and I'm not re-explaining the strategy every session. The beautiful thing that happens is the documentation stops being something you write for humans who might read it later, and becomes something the AI reads every single time it works.

## The Three-Layer Strategy

Before writing any code, I thought about what each testing layer should cover, and more importantly, what it *shouldn't*.

The key insight: **don't test the same thing at two layers** and **push tests down the stack as much as possible**. Unit tests verify that `CorrelationEngine` computes the right numbers. Snapshot tests verify that `InsightCardsView` renders those numbers correctly. E2E tests verify that a user can actually complete a check-in and see the dashboard update. Each layer has a job. None of them overlap.

## Layer 1: Snapshot Testing

### The infrastructure trap I avoided

swift-snapshot-testing from Point-Free felt like a decent choice. Though I'm new to snapshot testing and may need to experiment.

Not every view is snapshot-testable. Views that use SwiftData's `@Query` property wrapper need a live model container, which makes them painful to set up in isolation. My `TrendsDashboardView` and `DayDetailView` both fall into this category.

The solution: **snapshot the leaf components, not the containers.** `TrendsDashboardView` is composed of `InsightCardsView`, `TrendSummaryView`, and several metric cards. Each of those accepts data through props and bindings — no `@Query` needed. I snapshot-tested every leaf component and accepted that the composition layer is covered by E2E tests instead.

29 snapshot tests across 5 files. Each one runs in under a second.

## Layer 2: Service Tests

While I was building test infrastructure, I tackled the notification service — the biggest untested surface area in the app.

`NotificationService` manages daily reminders through `UNUserNotificationCenter`. It schedules, reschedules, and cancels notifications based on user settings. None of this had tests.

10 tests covering scheduling, rescheduling, content validation, and cancellation.

## Layer 3: Maestro E2E

Maestro was the final piece. It drives the actual iOS Simulator UI using YAML flow files — no compiled test code, no Xcode project integration, no XCUITest boilerplate. That's it. 11 lines to test the entire first-launch experience. Maestro handles the simulator, waits for elements, and reports pass/fail.

I wrote 7 flows total:

- **Onboarding** — fresh install through dashboard
- **Check-in happy path** — all 9 questions with every field filled
- **Check-in minimal** — selecting "None" for movement (skips duration picker)
- **Check-in navigation** — forward/backward preserves state
- **Dashboard empty state** — correct empty state after fresh onboarding
- **Dashboard with data** — full onboarding + check-in + verify dashboard populates
- **Redo today** — re-entry flow when today's entry already exists

The key decision: **use accessibility labels for element targeting.** Since Daily Pulse already has thorough accessibility labels (they're tested in the unit suite), Maestro can tap "3, Fair" instead of trying to hit a pixel coordinate. This makes the tests resilient to layout changes and also validates that accessibility labels are working correctly in production — a nice side effect.

## The Documentation Layer

Tests rot without documentation. I added three things to prevent that:

- `DailyPulseTests/CLAUDE.md` — rules for the test directory: which framework to use when, how to add a new test file to the Xcode project, what can and can't be snapshot-tested, concurrency requirements.
- `docs/snapshot-testing.md` — a complete guide: how to write a snapshot test, required variants, how to update reference images after intentional UI changes, what `record: .missing` does.
- `docs/e2e-testing.md` — Maestro conventions: flow file naming, when to use `clearState`, target flow list, CI integration pattern.

The decision table is the most useful part: "when do I write a unit test vs. snapshot vs. E2E?" Answer that without thinking, and you'll write the right test every time.

## What It Cost

One afternoon. Total with Claude Opus 4.8: ~4 hours. 6 PRs. 46 new tests plus 7 E2E flows.

The app went from "we test logic" to "we test logic, appearance, and user journeys" in a single sitting.

## The Takeaway

Testing infrastructure feels like a big project until you actually do it. The tools are mature — swift-snapshot-testing records its own reference images, Maestro reads your accessibility labels, Swift Testing's `@Suite` and `#expect` are genuinely pleasant to use. The hard part isn't the technology. It's deciding what to test where, writing it down, and keeping it consistent.

Yes, this is a small app so an afternoon was enough. A large enterprise app would take longer. Months? I don't think so. It all comes down to bringing the right context to the party along with a thoughtful testing strategy.
