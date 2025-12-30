---
layout: post
title: "The Bottleneck Was Never Writing Code"
description: "How small PRs and human-centered review become more important, not less, in an AI-assisted world"
date: 2025-12-30
---

I've spent my evenings over the holidays vibe coding. This is the longest sustained run I've had building something with Claude Code, and it has sparked some thoughts. Ever evolving.

The biggest thing I've noticed: it's shockingly easy to produce code that looks pretty good, and might even be good. But that ease of creation brings a temptation to produce large swaths of code in a single shot, which puts a heavy burden on deep review of that code. At my age, the attention span just isn't there to review 2,000 line PRs. Was it ever?

This got me thinking about something that's been a persistent challenge on my team: cycle time. We're running longer than we'd like on both the inner loop (development time) and the outer loop (review and merge time). And I realized that the same discipline that would fix our cycle time problem (small, frequent pull requests) is exactly what's needed to make AI-assisted development actually work and be safe.

## Review Is the New Writing

Here's the mental shift I've been sitting with: in an AI-forward workflow, reviewing code becomes more important than writing code.

Writing code is no longer the scarce resource. AI can generate code faster than any human. But understanding whether that code is right (whether it fits the architecture, handles edge cases, respects the product requirements, and won't break in production) requires human judgment.

This means the team's bottleneck shifts to review throughput. If everyone can generate four PRs a day instead of one, but review capacity stays the same, you've just created a traffic jam in a different location.

The implication: reviewing isn't a chore that interrupts "real work." Reviewing is the primary work. The work with the greatest priority and need for applied human intelligence. Unblocking your teammates by reviewing their code is at least as valuable as producing your own.

And this is where we have to be careful about AI assistance. Yes, AI can help with code review. I use it for: "Summarize this PR. What are the riskiest changes? Where should I focus my attention?"

But AI-assisted review is not the same as AI-replaced review. If we hand review entirely to the machine, we lose the institutional knowledge, the product context, the customer empathy that only the team has. We go to the lowest common denominator. The AI doesn't know that this particular code path has broken in production three times, or that this customer segment uses the feature in an unexpected way, or that the team decided six months ago to avoid this pattern for good reasons.

The human reviewer brings judgment that no model has. The goal is to use AI to accelerate that judgment. Get your bearings faster, focus your attention better. Not to replace it. In fact, in academia they now have peer reviewers sign attestations confirming that a human did indeed review the article, to ensure they don't run afoul of AI generating and AI reviewing. [This article from the National Library of Medicine](https://pmc.ncbi.nlm.nih.gov/articles/PMC11858604/) concludes, "Rather than replacing human expertise, AI should serve as a supportive tool, guided by comprehensive policies and adequate training." Hopefully, there was a human in the loop.

## The Seduction of Big PRs

AI makes it trivially easy to generate large amounts of code quickly. And the natural instinct is to let it rip. Why submit five small PRs when the AI can build the whole feature in one session?

I think the answer is that writing code was never the bottleneck. In software engineering we overindex on idea of writing code faster. However, in reality, it's all the things surrounding the code that take the time.

Understanding code, reviewing code, validating code, integrating code: those are the bottlenecks. And a 2,000-line AI-generated PR doesn't help with any of them. In fact, it makes them worse, because now you have a large change that neither the author nor the reviewer fully understands.

The math of cycle time doesn't care who wrote the code. A big PR takes longer to review, generates more feedback, requires more rounds of revision, contains more latent risk, and sits in the queue longer. Whether a human or an AI wrote it, the downstream costs are the same.

## Small PRs as a Forcing Function

The research on PR size is remarkably consistent. I looked for companies who advocate for larger PRs and couldn't find any.

[Google's publicly documented engineering practices](https://google.github.io/eng-practices/review/developer/small-cls.html) make the same point: reviewers have discretion to reject your change outright for the sole reason of it being too large. More recent [analysis from LinearB](https://linearb.io/blog/reducing-pr-review-time), studying over 700,000 pull requests, found that cycle time doubles when PRs exceed 200 lines, and that small PRs get picked up 20x faster than large ones.

But small PRs aren't just about review efficiency. They're a forcing function for clarity of thought.

When you constrain yourself to 100-200 lines, you have to answer: What's the smallest coherent change I can make? That question forces you to break down work into logical increments, each of which can stand on its own. It forces you to understand the structure of what you're building, not just let the AI generate it wholesale.

This is where AI can actually help. Breaking a task into constituent parts is something AI does reasonably well. You can prompt: "I need to add document scanning to this screen. What are the incremental steps I could take, where each step is a small, mergeable change?" The AI will give you a roadmap. Then you execute each step as a separate PR.

The discipline is: scope first, generate second. You decide the boundaries. The AI works within them.

## Addressing the Objections

### "A PR should be a complete feature"

Some developers argue that each PR should be a self-contained feature or feature slice. Google argues that the right size for a PR is one self-contained change: usually just one part of a feature, rather than a whole feature at once.

Ask any reviewer: would they rather have a series of 150-line self-contained changes throughout the week, or a 750-line PR to review on Friday?

The skill of breaking a feature into a set of small changes makes for safer changes and drives good practices on change management.

### "Small PRs create too much overhead"

While it's true that there is some transactional overhead on each PR, it is far outweighed by much faster pickup time, faster reviews, more bugs found in review, and a greater sense of flow and progress. Larger PRs mean there is a higher cost to rework if the direction taken by the developer is off track.

## A New Shape for the Workday

If we take this seriously, the shape of an engineer's day changes. One deeply focused on reviewing code and keeping the standard high and the codebase clean.

The old model: take a story, build it until it's done, submit a big PR, move on to something else while you wait for review, context-switch back when feedback arrives.

The new model: always be mid-flow. Have something in review, something you're coding, something you're about to start. Check the review queue before you start coding. Address feedback immediately while context is fresh. Submit small increments continuously. Keep work moving.

What does this actually look like? Morning starts with triage. Check PRs waiting for your review, check your own open PRs for feedback, address anything blocking. Then you work on your current increment, not your whole feature. By early afternoon, you've submitted something small. You start the next piece. By the end of day, you might have merged one PR and have another in review. Work flowed.

The goal isn't to finish things. It's to keep things flowing. Doesn't that sound wonderful? I'm always making progress.

This feels different. The dopamine hit of "I built a big feature" is replaced by the quieter satisfaction of "work flowed through me today." It takes some adjustment. But the team-level outcomes are better: faster cycle times, higher quality, fewer bottlenecks, less time waiting, more time actually shipping.

## What Stays Human

There's a broader theme here that I'm still working through: as AI takes over more of the mechanical work of coding, what becomes more valuable is everything that's uniquely human.

Breaking down problems thoughtfully. Reviewing code with product context. Communicating clearly with teammates. Understanding customer needs. Making judgment calls about risk and quality. Coming up with the next truly market making innovation.

Software development has always been a team sport. It's always been interpersonal as much as technical. AI doesn't change that. If anything, it amplifies it. The teams that thrive won't be the ones that generate the most code. They'll be the ones that collaborate most effectively, review most thoughtfully, and ship most reliably.

There's more to explore here. How teams adapt their rituals, how we train engineers for this new world, how we maintain quality when the volume of code increases. That's probably another article(s).

For now, the practical takeaway: if you're using AI to write code, keep your PRs small anyway. The physics of flow don't change just because the writing got faster. And the review, the human, contextual, judgment-laden review, is where you add the value that the machine can't.

## Bonus: Something to Try

This might be something to provide to your tool of choice to help break down tasks and keep PRs small.

```
## Pull Request Size

Keep PRs small. Target 200-400 lines of changed code. Going above this range
is acceptable when there's good reason, but requires greater care and
justification.

Before implementing a feature or significant change:

1. Break the work into the smallest coherent, mergeable increments
2. Propose the breakdown before writing code
3. Implement one increment at a time, confirming scope before proceeding

If a single increment looks like it will be large, stop and consider whether
it can be broken down further.

Never bundle unrelated changes (refactors, cleanups, "while I'm here" fixes)
into a feature PR. Each PR should be one self-contained change.
```

---

*Andrew Little is VP of Engineering for Field Service Mobile at Salesforce.*
