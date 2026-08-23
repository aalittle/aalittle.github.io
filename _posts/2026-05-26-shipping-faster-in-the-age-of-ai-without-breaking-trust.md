---
layout: post
title: "Shipping Faster in the Age of AI, Without Breaking Trust"
description: "The problem was never shipping too often. It was shipping disruption too often. A change taxonomy, a soak lifecycle, and giving users a way to opt out."
date: 2026-05-26
---

## A cautionary tale

A few years ago we moved a mobile product I lead onto continuous delivery. I think we learned something important from it. We communicated the change to customers, but I don't think we got them to a deep understanding of what we were doing to keep those deliveries safe. So when we shipped a couple of customer-impacting bugs, the reaction wasn't "that's a bug," it was "this is what continuous delivery gets us." When we looked back, none of those bugs were actually related to the delivery cadence. They were bugs that would have shipped either way. But customers had clubbed the two things together, and we started hearing real pushback. They wanted a slower cadence. We had to listen.

The lesson stuck with me, and it's worth holding onto as we think about doing this again, only more so.

Quick context: I lead engineering for an enterprise mobile product in the Salesforce ecosystem, so words like "admin" and "org" show up below. The mechanisms apply broadly to any B2B product where the buyer, the administrator, and the end user are different people, but the vocabulary is shaped by where I sit.

## The premise

If we believe we can move faster and deliver more value, and I do, then a lot of what we've thought about and implemented as engineers over the last decade in terms of continuous delivery and continuous integration becomes even more important. For those of us lagging a bit behind on what an ideal system looks like, it probably behooves us to take a look at where the gaps are and progress toward closing them.

The harder question, especially in an enterprise context, is whether our customers are ready for more change. I've seen examples where we can be seen as moving too fast for an enterprise customer who is already having enough trouble adopting major releases. That said, I don't know a customer in the world that wouldn't appreciate a bug fix, a performance improvement, or a feature that gets just a little bit easier to use.

So ultimately, for me, it comes down to judgment, and to not repeating that mistake. We need to push out the kinds of things customers are really going to appreciate. In an enterprise world, that means probably not disrupting a random Monday with a new way they have to work within our application. They need to plan for those things, and change management, especially in a mission critical world, is going to continue to be paramount.

## The standard can only go up

As we think about shipping more value more often, the standard for how we ship can only go up. Because as soon as we start to disrupt users outside of a major release, we are going to hear from them.

The problem was never shipping too often. The problem was shipping disruption too often. If you can decouple the frequency from the disruption, you get the best of both worlds: fast fixes and stable workflows.

## A change taxonomy

The way we get there is by categorizing what we ship and how it should roll out. Not every change is the same, and our rollout behavior should reflect that.

- **Invisible** (performance, stability, bug fixes): Ship immediately, no gate needed. No customer ever said "please wait three weeks to fix my crash."
- **Additive** (new capability, no disruption to existing flows): Off by default, admin opt-in. The customer decides when they're ready.
- **Refinement** (UI modernization, improvements to existing workflows): Gradual rollout with user opt-out. I'll come back to this one.
- **Transformative** (new way of working, requires change management): Major release cadence, full change management ceremony.

This gives teams a decision framework rather than case-by-case judgment calls every time something ships.

## Putting control in the customer's hands

A couple of things I've been thinking about. We need to put more control in the hands of our customers, communicate the changes that are coming and have landed, and give both us and our customers a way to respond when something isn't working.

New innovation should be off by default and rolled out over a soak period. That gives us a calm and controlled rollout where we can hear from customers, see their adoption and engagement, measure the performance and health of the feature, and then roll back or pause if we see anything of concern.

For features that require change management or disruption, the customer and the admin opt in. That's a pretty standard pattern. The more interesting opportunity is on the other side. As we roll out things like slight UI improvements that we feel are non-disruptive but could impede the user in some unexpected way, we should look at allowing the user themselves to opt out.

Here's the case I keep coming back to. Imagine modernizing a workflow UI. The team is excited and thinks it's all upside. But what if the new experience just doesn't work the way a particular user expects? What if a user doesn't like the way something was redesigned, or finds the performance to be suboptimal? We'd like to allow that user to opt out. We're in the business of making the end user more productive and happier, and we should consider, when we can, allowing them to vote on whether something is improving or making things worse.

## What the opt-out signal gives us

Obviously this wouldn't be on every feature. Customers wouldn't like it if every feature could be opted out of by the end user. But when we do offer it, the opt-out itself is a highly valuable signal, and giving the user a quick way to say why makes it more powerful. If users start telling us they're opting out because of performance, that's something we can act on.

The feedback mechanism should be extremely lightweight. One tap from a short list: "Too slow," "Confusing," "Breaks my workflow," "Not now." Optional free text. Anything more and the user won't bother. But the categories alone give you actionable signal at scale.

## Automatic circuit breakers

One thing we shouldn't rely on is humans noticing problems. If opt-out rate crosses a threshold, say 15% within the first 48 hours of a rollout cohort, we should auto-pause the rollout and alert the team. That turns the opt-out signal into a safety mechanism, not just a feedback channel. It means the user's voice actually has power, and we don't need someone watching a dashboard at all hours to catch a bad rollout.

## The soak, signal, stabilize lifecycle

There's a named lifecycle here that's worth being explicit about. Every non-invisible change goes through:

- **Soak:** Small cohort, monitoring health and adoption.
- **Signal:** Watch opt-out rates, performance, support tickets, and engagement.
- **Stabilize:** Address the feedback, iterate, and then graduate to default-on.
- **Sunset the old path:** With advance notice, move everyone forward.

We eventually stabilize the feature, take the feedback, and move everyone into it. But that's a much better experience than forcing them in from day one of the new release.

## Admin visibility

The other piece is communication. Admins should have a single view of what's rolling out to their org, what percentage of their users have it, opt-out rates, and what's coming next. That replaces the anxiety of "what changed?" with transparency, and it gives them ammunition for their own internal change management.

## Why this raises the quality bar

This part is important to call out. If teams know their feature will be exposed to user opt-out and visible rollout metrics, they're going to invest more in polish before shipping. The process itself raises the quality bar. You're not going to ship something half-baked if you know users can vote with their fingers and the team gets an alert when they do.

And that's a good thing. The implicit deal we're making with our customers when we ship more often is that we're only shipping things that make their life better. The moment we start shipping things that make their life harder, we've broken the trust, and we're going to hear about it.

## Where this leaves us

The taxonomy, the soak lifecycle, the user-level opt-out, the admin visibility, and the automatic circuit breakers give us a foundation where we can ship value faster without sacrificing the trust enterprise customers have placed in us. The judgment still matters. Not every feature gets every mechanism. But we need the mechanisms available so we can apply that judgment thoughtfully. The standard goes up, the frequency goes up, and the user's voice in the process gets louder. I think that's something worth building toward.
