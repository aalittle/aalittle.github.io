---
layout: post
title: "Five Things I'm Seeing on Teams Where Claude Code Is Actually Working"
description: "Some teams are seeing 2x on throughput and some are seeing very little. Five common threads separate the two cohorts."
date: 2026-05-23
---

I lead a 70-person engineering org at Salesforce. Like everyone in tech right now, we're pushing hard on agentic engineering. Some teams are seeing 2x or more on throughput. Some individual engineers are seeing much higher than that. And some are seeing very little benefit at all. We're looking at PRs, work items, cycle time.

So I dug into what separates the two cohorts and found five common threads.

This is what I'm seeing across our pilots, not a comprehensive framework. Your mileage may vary. But if your Claude Code rollout needs tuning, start here.

## 1. Their tests run themselves

The fastest engineers don't manually verify every bit of Claude Code's output. They can't. They're running too many parallel changes for that to work.

What they have in common is solid automated coverage they trust. They treat the test suite as the validation gate, not their eyes. When the suite passes, they review for architecture and ship. When it fails, they iterate.

The teams stuck on manual QA are getting the smallest gains. It's the bottleneck, not the AI. If you're feeling like Claude Code is not paying off, look at where validation actually happens in your loop. If it's still mostly in your head, that's the first thing to fix.

## 2. They make decisions fast

A common thread on fast teams is their level of autonomy and how they make decisions.

Fast teams have tight scopes. PMs who answer questions in Slack instead of scheduling meetings. Tech leads empowered to call architectural decisions without escalation. When ambiguity comes up, someone resolves it within hours. I've also seen technical teams without strong cross-functional dependency move faster because the autonomy sits directly with the engineers.

My recommendation: get senior leaders to lean in on a couple of teams and help them with fast decision making. See if they accelerate as a result. If it proves out, build a more durable decision-making structure going forward.

The agent surfaces decision latency that was always there, hidden before and obvious now.

## 3. High-quality PRDs and user stories

The tickets that go cleanly through Claude Code share a common structure. Specific acceptance criteria. The how is left open for the engineer and the agent to figure out.

Vague PRDs were tolerable when humans filled the gaps with judgment and back-channel context. Claude Code does not have that context. It will fill the gap confidently and incorrectly, and you get a working feature that is not the right feature.

The fast engineers ensure their work items are well specified **before** they start. That ten minutes upstream saves an hour of rework downstream. And the best teams figure out that high-quality PRDs are one of the most critical inputs, because they generate a lot more of the artifacts downstream from them.

Our approach: generate high-quality PRDs and review them meticulously, then generate epics and stories from there.

## 4. They've built skills for their context, not generic ones

A note on terminology: by skills I mean Claude Code skills, the agent capabilities and conventions we configure.

The engineers getting the most lift have invested in skills that encode their conventions. Team specific patterns. Their design system. Their architecture quirks. Their user's jobs to be done. Their testing approach. Things a new engineer would learn in their first six months.

What they have not built: skills for general Swift, general Kotlin, generic best practices. The model already knows that. Skills layered on top of native capability are noise, whereas skills that encode specific ways of working are where you get the real value. I learned this lesson directly from Anthropic when someone there reviewed an early Claude Code Plugin we had created.

Don't teach it how to code. Teach it what's unique about your product and your expectations of your engineers.

## 5. They actually delegate

This is the biggest one and the most common opportunity for the big speedup.

Many engineers "using" Claude Code are asking it questions and then writing the code themselves. That is not delegation. That is consulting an oracle. The lift is small because most of the work is still happening in their hands.

The engineers pulling away hand the ticket over. They let the agent break it down, write the code, run the tests. They step in at review gates. They step in when something is off. They do not step in to type.

This is a hard mental shift. We learned to code by typing. Stopping feels wrong. The first few times you do it, you'll want to take the keyboard back. Don't. Sit with the discomfort. Review what came out. Iterate with the agent. The output gets cleaner as your prompts get tighter.

We built a `/start-work [ticket-id]` command for our team to make this mechanical. It forces the first step so engineers don't have to argue themselves into it every time, and it just removes the option to half-delegate.

## Where this leaves us

If your team is using Claude Code and not getting the gains, the bottleneck is probably one (or more) of these five. Most often it is the last one. Engineers who learned to code by typing are reluctant to stop typing.

Tests, decisions, PRDs, skills, delegation. Fix those and the gains compound.

More to come as we learn. This is the snapshot from where I sit today.
