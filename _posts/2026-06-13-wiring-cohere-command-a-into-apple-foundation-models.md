---
layout: post
title: "Wiring Cohere's Command A+ into Apple's Foundation Models Framework"
description: "A first-experience writeup on making Command A+ a drop-in replacement for Apple's SystemLanguageModel"
date: 2026-06-13
---

Apple opened the `LanguageModel` protocol at WWDC 2026, so a third-party model can sit behind the same `LanguageModelSession` that runs Apple's on-device model. The session that walks through it is Bring an LLM provider to the Foundation Models framework. I spent a few days building CohereLanguageModel, a package that makes Cohere's Command A+ a drop-in replacement for Apple's `SystemLanguageModel`, and the short version is that it came together more cleanly than I expected. This is my first-experience writeup: what fit, what did not, and the deployment flexibility it opens up.

## The Clean Fit

Two things had to come together. First I got Command A+ running against Cohere's hosted production endpoint, with Claude and Cohere North Mini Code doing a lot of the building alongside me (of course). Then I looked at how closely Cohere's API lines up with what Apple expects from a provider, and the answer is: closely. A pleasant surprise.

Apple hands your executor a `LanguageModelExecutorGenerationChannel` and you push events into it with `channel.send()`, rather than returning an `AsyncSequence`. Cohere streams its responses as server-sent events. So most of the work was translating one stream into the other. I take Cohere's SSE events, turn them into a small set of actions (append text, append reasoning, report usage, finish), and replay those onto Apple's channel. The shapes are different, but the mapping is direct, and once it clicked the wiring was short.

## The Disconnects

There were a few disconnects. None of them turned out to be showstoppers, at least not so far, but they are the kind of thing worth knowing before you start.

The first is that Command A+ reasons before it answers. The reasoning comes back as thinking blocks interleaved with the text, and it spends tokens. I set a tight token budget on an early test, 16 tokens, and the model used all of them thinking and never said a word. The fix was to give it room. The lesson is that a token limit means something different when the model has a reasoning phase you cannot turn off, and if you expose a token control to your users, they will hit this.

The second is usage reporting order. Apple recommends sending metadata, then usage, then the text. Cohere reports usage only at the end of the message, after the last text delta. I documented it as a deliberate deviation and emit usage right before I finish the stream. The framework does not enforce the order, so it works, but it would surprise you if you assumed Apple's recommended order was the only valid one.

The third is per-fragment token counts. Apple wants a token count with every text append. Cohere does not report counts per delta, so I send zero for each fragment and then report accurate totals at the end. It is an honest representation of what the API actually gives you, and the framework handles it fine.

## Speed and Generation Quality

One thing that stood out was speed. Cohere's hosted production endpoint felt fast, with render times that held up well in interactive use. I want to put real numbers behind that rather than a gut feel, so I am going to run a proper timing test against the endpoint and add the measured render times here once I have them.

I also wired the package into a personal project and ran it against my own use case. The generations were really nice for what I needed, and my own evaluations looked good. That is not a benchmark, it is one developer's read, but it was enough to make me want to keep going.

## Why This Matters

What this package gives you is a mechanism to drop a Swift framework into your project, point it at a Cohere API key hosted on your own server (for safety), and call Command A+ directly from an iOS app. That on its own is powerful. You get a frontier model behind Apple's native session API, and your key never sits in the app binary.

It goes further if you are a Cohere customer running your own infrastructure. If you have an air-gapped, self-hosted, or VPC-hosted Cohere deployment, the same iOS app can talk to it with a single change: the `baseURL` you pass to the framework. Point it at `api.cohere.com` and you get the hosted endpoint. Point it at your internal deployment and the app authenticates against your own infrastructure, the prompts and documents stay inside your network, and nothing about the app code changes. One URL is the difference between the public endpoint and a fully private one.

So, a pretty neat first experience integrating with Cohere. I think they are doing something quite interesting. And real credit to Apple for opening up the framework, because this is what makes the flexibility possible. Different use cases carry different security and privacy concerns, and that is only going to become more true as more of what we build runs through AI. Being able to choose where the model lives and where the data goes, without rewriting your app, is not a nice-to-have. I think that flexibility is going to be one of the things that decides what you can responsibly ship.

The package is open source at [github.com/aalittle/CohereLanguageModel](https://github.com/aalittle/CohereLanguageModel) if you want to look at how the pieces fit together. I wrote separately about why this control question matters to me.
