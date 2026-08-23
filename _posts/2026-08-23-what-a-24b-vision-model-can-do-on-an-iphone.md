---
layout: post
title: "What a 2.4B Vision Model Can Actually Do on an iPhone, Offline"
description: "Ten days testing whether a small open-weights vision model can do real field-service work on a phone with no signal"
date: 2026-08-23
read_time: 12
---

I spent ten days finding out whether a small vision model can do real field-service work on a phone with no signal. The short answer is yes, twelve for twelve on six tests. The most useful thing I learned was that one bad default in my own code was costing me sixty seconds per photo.

<div class="readout breakout"><div class="cell"><span class="k">Test cases</span><span class="v">12<small>/12</small></span><span class="n">clean + degraded</span></div><div class="cell hot"><span class="k">Photo, before</span><span class="v">63.3<small> s</small></span><span class="n">full resolution</span></div><div class="cell"><span class="k">Photo, after</span><span class="v">6.3<small> s</small></span><span class="n">one capped default</span></div><div class="cell"><span class="k">Per image</span><span class="v">4.2<small>–7.6 s</small></span><span class="n">on the fixtures</span></div></div>

<div class="takeaways"><span class="t-label">The short version</span><ul><li>A <strong>2.4B open-weights vision model runs entirely on a base iPhone 16, offline</strong>, and clears all six of my field-service test cases. Twelve for twelve across clean and photograph-degraded images, with answers character-for-character identical to a Mac.</li><li><strong>Capping the input pixel budget took a full-resolution phone photograph from 63.3 seconds to 6.3</strong>, and from 5.28 GB of memory to 3.21. It cut my slowest test fixture too, 10.2 seconds to 7.6, and changed not one answer.</li><li><strong>The model earns its 2.17 GB specifically.</strong> Apple's free on-device OCR transcribes better and about 19× faster, but it cannot pick the right field off a plate. That is the job the model is doing.</li></ul></div>

## Why I wanted to know
{: data-eyebrow="The question"}

I lead Field Service Mobile, so the scenario I care about is not a benchmark. It is a technician standing at a switchgear cabinet with a phone, a nameplate covered in six numbers, and no bars. The question is not whether a model can read text. It is whether it can read *the right* text, fast enough to be worth pulling the phone out, on hardware that fits in a pocket.

Most of the published evidence about small vision models stops at a score on a laptop. I wanted to know what happens on the device, in airplane mode, on images that look like something a person actually photographed.

So I set it up as a research project rather than a demo: six jobs, each one image and one prompt, each with a pass bar written down before I started. The output is a filled-in results table and a decision, not an app anyone ships.

## What I was up against
{: data-eyebrow="The challenge"}

The model is Cohere Labs' **North-Micro-Vision-Instruct**, 2.4B parameters, Apache 2.0. On paper it does OCR, visual question answering, document reading, spatial grounding with bounding boxes, and eleven-plus languages. It does not reason, do math, or run agent loops. It is perception only.

Three things stood between "on paper" and "on a phone."

**No Swift implementation existed.** MLX, Apple's on-device machine learning framework, had no port of this architecture. Running it on iOS meant hand-writing about 2,000 lines of Swift and proving they matched the Python reference numerically, not approximately. This was the top risk for most of the project.

**The memory budget was unknown.** I had assumed 1.3 to 1.5 GB of weights. The real figure at 4-bit is 2.17 GB, about 45% larger, on a phone with 8 GB shared with everything else iOS is doing.

**Nobody had run the tasks I cared about.** Not a benchmark suite. Six field jobs with a stated pass bar: read a nameplate, pick the right port, box it, read an analog gauge, pull fields off a Japanese label, pull fields off a paper work order.

## Six jobs, ten images
{: data-eyebrow="The test set"}

I generate the fixtures rather than collect them, so the ground truth is exact and no employer data is anywhere near this. Every scene exists twice: the clean render, and a **field** variant put through perspective warp, blur, glare and sensor noise to approximate a photo taken by hand in a plant room.

<div class="fixture-grid breakout"><div class="scene"><div class="scene-hdr"><span class="scene-no">1</span><span class="scene-nm">Nameplate read</span></div><div class="scene-pair"><div class="scene-half"><img src="/assets/img/vision-ai/nameplate.png" alt="Equipment nameplate, clean render"><span>clean</span></div><div class="scene-half"><img src="/assets/img/vision-ai/nameplate_field.jpg" alt="Equipment nameplate, degraded to look photographed"><span>field</span></div></div><div class="scene-bar"><span class="pass-badge">PASS ×2</span><span>Serial, model and rating all correct</span></div></div><div class="scene"><div class="scene-hdr"><span class="scene-no">2 · 3</span><span class="scene-nm">Field pick + grounding</span></div><div class="scene-pair"><div class="scene-half"><img src="/assets/img/vision-ai/panel.png" alt="Hydraulic manifold panel with six numbered ports, clean render"><span>clean</span></div><div class="scene-half"><img src="/assets/img/vision-ai/panel_field.jpg" alt="Hydraulic manifold panel, degraded"><span>field</span></div></div><div class="scene-bar"><span class="pass-badge">PASS ×2</span><span>Names the right port, and boxes it</span></div></div><div class="scene"><div class="scene-hdr"><span class="scene-no">4</span><span class="scene-nm">Gauge read</span></div><div class="scene-pair"><div class="scene-half"><img src="/assets/img/vision-ai/gauge.png" alt="Analog pressure gauge, clean render"><span>clean</span></div><div class="scene-half"><img src="/assets/img/vision-ai/gauge_field.jpg" alt="Analog pressure gauge, degraded"><span>field</span></div></div><div class="scene-bar"><span class="pass-badge">PASS ×2</span><span>Within one unit of 6.5 bar</span></div></div><div class="scene"><div class="scene-hdr"><span class="scene-no">5</span><span class="scene-nm">Multilingual</span></div><div class="scene-pair"><div class="scene-half"><img src="/assets/img/vision-ai/label_ja.png" alt="Japanese equipment label, clean render"><span>clean</span></div><div class="scene-half"><img src="/assets/img/vision-ai/label_ja_field.jpg" alt="Japanese equipment label, degraded"><span>field</span></div></div><div class="scene-bar"><span class="pass-badge">PASS ×2</span><span>Fields correct, in English</span></div></div><div class="scene"><div class="scene-hdr"><span class="scene-no">6</span><span class="scene-nm">Doc to fields</span></div><div class="scene-pair"><div class="scene-half"><img src="/assets/img/vision-ai/workorder.png" alt="Paper work order, clean render"><span>clean</span></div><div class="scene-half"><img src="/assets/img/vision-ai/workorder_field.jpg" alt="Paper work order, degraded"><span>field</span></div></div><div class="scene-bar"><span class="pass-badge">PASS ×2</span><span>All five fields correct</span></div></div></div>

All ten passed on the iPhone 16, and the degradation broke no test. The only measurable cost was grounding precision.

## How I built it
{: data-eyebrow="The build · 10 days"}

Ninety-five commits over ten days, every change landing as a pull request. The order mattered more than the speed.

**A desktop reference run came first.** Six tests, three repeats, both variants, on an M5 Mac, with run-to-run spread under 0.04 seconds. That gave me not just a score but a fixed target: exact strings, exact box coordinates, exact latencies for the phone to be measured against later.

**Then the Swift port, validated in halves.** One check compares the vision tower's forward pass against a dumped Python reference. The other checks that the real checkpoint decodes into the right architecture, and needs no weights and no network, which matters because MLX cannot initialise on a hosted CI runner at all. That second half is what gates my continuous integration.

**Then the device.** The Simulator was never an option. With no GPU to name, MLX crashes before producing a number, so every inference figure here comes from hardware.

**Then airplane mode.** Recorded, not asserted. The app stamps the radio state onto every answer and traps any network request made during inference, so the offline claim is something a reader can check rather than something I promise.

## What the phone does
{: data-eyebrow="The loop"}

The finished app is one screen: a photo picker, a prompt field, the six-test suite, and a run log that exports to JSON and CSV. Every number below came out of that export rather than being transcribed by hand.

<ol class="steps"><li>Photograph a nameplate, gauge, panel or work order.</li><li>Ask in plain language, "What is the serial number?"</li><li>The image is capped to a fixed pixel budget and prefilled.</li><li>The model answers in text, 4.2 to 7.6 seconds on my test fixtures, 6.3 on a full-resolution camera photo.</li><li>The answer, latency, memory headroom and radio state are written to the run log.</li></ol>

No backend, no accounts, no second model. Once the weights are cached on first launch, nothing leaves the phone.

## Three answers, as the phone returned them
{: data-eyebrow="Reading it back"}

Every string below is copied from the run log, not retyped. The device answers were character-for-character identical to the Mac reference, so the text is the same on both. The timings are the phone's, from the twelve-test run on 14 August. The pixel cap landed later and changed none of these answers.

### The degraded nameplate

<div class="worked breakout"><div class="worked-shot"><img src="/assets/img/vision-ai/nameplate_field.jpg" alt="An equipment nameplate, warped and blurred to look photographed by hand"></div><div class="worked-xchg"><div class="turn-ask"><span class="turn-role">I asked</span><p class="turn-said">What are the serial number, model, and rating on this nameplate?</p></div><div class="turn-ans"><span class="turn-role">It answered</span><p class="turn-said">The serial number is <em>SN-917734-B</em>, the model is <em>AC-4820-XT</em>, and the rating is <em>480V 3PH 60Hz 22kW</em>.</p></div><div class="chips"><span class="chip y">✓ serial</span><span class="chip y">✓ model</span><span class="chip y">✓ rating</span></div><div class="stampline"><span>iPhone 16 · <strong>5.6 s</strong></span><span>prefill <strong>4.34 s</strong></span><span>decode <strong>1.23 s</strong></span></div></div></div>

Three fields lifted off a plate that has been warped, blurred and glared. The answer is identical to the one from the clean render, and identical to the Mac's, character for character. Note what it is *not* doing: that plate also carries a max current and an IP rating, and neither was asked for or returned.

### Where it says the port is

<div class="grounding-figure breakout"><img src="/assets/img/vision-ai/panel.png" alt="Hydraulic manifold panel with six numbered ports, with bounding boxes drawn over port 3"><svg viewBox="0 0 1120 700" preserveAspectRatio="none" aria-hidden="true"><rect x="858" y="178" width="104" height="104" fill="none" stroke="#9AA7AE" stroke-width="4" stroke-dasharray="12 8"></rect><rect x="840" y="147" width="145.6" height="189" fill="none" stroke="#6DB89D" stroke-width="4" stroke-dasharray="3 7"></rect><rect x="873.6" y="140" width="100.8" height="140" fill="rgba(109,184,157,.16)" stroke="#6DB89D" stroke-width="4"></rect></svg></div>

<div class="legend breakout"><span><i class="truth"></i> ground truth</span><span><i class="pred"></i> model, clean</span><span><i class="predf"></i> model, degraded</span></div>

> **I asked:** Give the bounding box coordinates [x0, y0, x1, y1] of the connector labeled 3.
>
> **It answered:** `[780, 200, 870, 400]` — 6.8 s on the phone, overlap with truth 0.57 clean and 0.40 degraded.

Coordinates come back normalised to 1000, not in pixels. I made that fixture 1120×700 rather than a round 1000 wide precisely so the two conventions give different answers and the ambiguity is resolvable. Scored as pixels the overlap is 0.00; scored correctly it is 0.57.

The box lands on the right connector every time, but it is loose. The clean box reaches up over the number badge, the degraded one down into the label beneath. Good enough to point a technician at a port, not good enough to crop with.

### The same gauge, asked two ways

<div class="worked breakout"><div class="worked-shot"><img src="/assets/img/vision-ai/gauge.png" alt="An analog pressure gauge, needle between 6 and 7 bar"></div><div class="worked-xchg"><div class="turn-ask"><span class="turn-role">I asked openly</span><p class="turn-said">What pressure does this gauge read?</p></div><div class="turn-ans"><span class="turn-role">It answered</span><p class="turn-said">The gauge in the image reads a pressure of approximately <em>6.5 bar</em>. This reading is indicated by the needle pointing just past the 6 mark on the scale.</p></div><div class="turn-ask"><span class="turn-role">I asked for a terse answer</span><p class="turn-said">…Answer with the number and unit.</p></div><div class="turn-ans"><span class="turn-role">It answered</span><p class="turn-said">7 bar</p></div><div class="chips"><span class="chip y">✓ true value 6.5 bar</span><span class="chip">± constrained answer off by 0.5</span></div></div></div>

The model can interpolate between ticks, and it loses that precision the moment I constrain the output format. Both answers pass a ±1 bar bar, so my results table shows two passes and hides the difference entirely.

This is the clearest argument I found against tightening a prompt to make parsing easier.

## Sixty seconds became five
{: data-eyebrow="The impact"}

The largest single finding was a bad default of my own making. The pixel budget had been inherited from the Python reference so that my Swift-versus-Python comparison would be meaningful during the port, and I never revisited it once the port was validated. Every 32×32 pixels costs one vision token, so a full-resolution phone photo was spending about 3,800 tokens where a test fixture spent 490.

| Full-resolution phone photo | Before | After |
|---|---|---|
| Prefill time | 60.6 s | **5.5 s** |
| Total | 63.3 s | **6.3 s** |
| Peak memory | 5.28 GB | **3.21 GB** |
| Headroom remaining | 1.31 GB | **3.32 GB** |

The gain was roughly double what token arithmetic predicted, and the memory column is why. At 1.31 GB free on an 8 GB phone, iOS is reclaiming memory under pressure, so a large share of that minute was the pressure rather than the tokens. Cutting the budget removed both costs at once.

No accuracy was traded for it. Across 70 real photographs the lower budget scored no worse, and my six fixtures returned identical answers before and after.

It was not only about the camera, either. The cap cut the slowest fixture in the suite, the full-page work order, from 10.2 seconds to 7.6, and pulled the whole set into a 4.2 to 7.6 second band. My honest answer to "under a few seconds" went from *small images yes, a full page no* to *everything, roughly*.

## Where the model earns its footprint
{: data-eyebrow="The comparison"}

The obvious challenge to any of this is that Apple ships free on-device OCR, so why carry 2.17 GB of model. I measured it against Apple's Vision framework on the same 70 photographs.

| Approach | Serial read correctly | Median time |
|---|---|---|
| The model, image alone | **80.0%** | 1.57 s |
| The model, image + Vision's transcript | 71.4% | 1.61 s |
| Vision's transcript alone, no image | 17.1% | 0.18 s |

Vision's transcript contains the right serial 83% of the time, and it produces it in 81 milliseconds. But it returns a median of six text regions per photo, up to 63, and says nothing about which one I asked for.

The middle row is the interesting one. Handing the model that transcript made it **worse**. Given the exact characters as text, which contain the right answer 83% of the time, it picks correctly in 17% of cases. Given the picture instead, four times in five.

The difference is everything a transcript discards: where the number sits on the plate, what is printed beside it, how large it is, whether it sits in a stamped box. The model is not doing OCR-then-choose. It is doing layout-aware field selection, and that is what neither Vision nor a regex can do.

So the footprint is earned, and specifically. Transcription is solved, free and fast. Deciding which of six numbers on a plate is the one someone asked for is not.

## What the numbers don't say
{: data-eyebrow="The caveats"}

**Prompt phrasing is load-bearing.** The model has a grounding head, and ordinary phrasing can trigger it. Same image, same model, same settings:

<div class="prompts breakout"><div class="pr pr-miss"><span class="pr-verdict">Coordinates, not an answer</span><p class="pr-q">"Read this nameplate. <strong>Return</strong> the serial number, the model, and the rating."</p><p class="pr-a">[[417,318,417,318], [417,418,417,418], [417,550,417,550]]</p></div><div class="pr pr-hit"><span class="pr-verdict">The answer</span><p class="pr-q">"<strong>What are</strong> the serial number, model, and rating on this nameplate?"</p><p class="pr-a">AC-4820-XT, SN-917734-B, 480V 3PH 60Hz 22kW</p></div></div>

Three of my six use cases failed under the first phrasing and passed under the second. I cannot put a raw text box in front of a technician and hope.

**Tightening the output format cost accuracy**, as the gauge showed.

**Backgrounding the app kills it.** With the model resident, iOS sends `SIGKILL` within seconds of the app leaving the foreground. A technician who switches to a work-order app and comes back pays a full cold start, not a resume. That is a constraint to design around, not a bug to fix.

**And one of my six passes is not clean.**

<div class="worked breakout"><div class="worked-shot"><img src="/assets/img/vision-ai/label_ja.png" alt="A Japanese equipment label"></div><div class="worked-xchg"><div class="turn-ask"><span class="turn-role">I asked</span><p class="turn-said">This label is in Japanese. Extract all the fields and give them in English.</p></div><div class="turn-ans"><span class="turn-role">It answered</span><p class="turn-said">Product Name: High Pressure Sensor Unit<br>Model: <em>KD-7714-J</em><br>Output Voltage: 24V Direct<br>Manufacturing Number: <em>JP-2260814</em><br>Manufacturing Month: 2026年3月</p></div><div class="chips"><span class="chip y">✓ model</span><span class="chip y">✓ serial</span><span class="chip">± 直流 → "Direct", not "DC"</span><span class="chip">✗ 製造年月 untranslated</span></div></div></div>

My checker verified model, serial and voltage and scored this a pass. A stricter bar, every field correct in English, fails it. It answered a question about translation by leaving a date in Japanese in the middle of an English answer. I treat multilingual as promising and unproven.

## The half I deleted
{: data-eyebrow="What didn't land"}

The project was meant to have two parts. The second was a fine-tune, to show that cheap training beats the base model on a real field job. I never built it, because both premises it rested on turned out to be wrong when I tested them first.

The first premise was that the base model reads stamped and raised metal serials badly. Measured, it reads them at 63 to 78% with *zero* character substitutions. No 8 read as B, no 0 read as O. Those confusions were exactly the before-and-after demo cases I had planned, and they never occurred. In hindsight this is unsurprising: reading text from images is everywhere in public training data.

So I re-aimed it at a private labelling scheme, something the model could not possibly know rather than something it could not see. The replacement premise was that it cannot apply such a scheme even when handed the whole key in the prompt. The probe I built to show that turned out to be measuring something else. It failed by answering from the head of a fixed, shape-grouped list rather than by misreading anything, and the comparison it rested on varied three things at once where I had written it up as varying one. The honest version of that test never got run.

On 23 August I deleted the whole workstream. Twenty-three files, about 13,400 lines: the scheme tooling, the label renderer, the print-sheet generator, the review server, the probe output. Keeping it implied an open workstream that did not exist.

Two things survived the cut, because they had quietly stopped serving the fine-tune and started serving the phone: the 70 real nameplate photographs and their hand transcriptions. They are the entire evidence base for the pixel cap above.

## What I would carry forward
{: data-eyebrow="Lessons"}

**Measure the real input, not the fixture.** An early sweep told me a far lower pixel budget was safe. It was measuring close-up renders where the plate fills the frame, not the wide 2.8 MP scenes people actually photograph. The wrong test set gave me a right-looking answer for the wrong reason.

**Peak memory across a session is the number that matters**, not peak per inference. Every individual inference had headroom, and my twelve-test run was still killed by iOS on test 6, because MLX's GPU cache grows to the largest allocation it has seen and never gives it back. Clearing it between inferences took the footprint from a rising 3.99–5.58 GB to a flat 2.68–2.89 GB. A technician photographing ten nameplates in a row is doing exactly what killed that first run.

**Compare against the free baseline before claiming the model is needed.** Apple Vision is better at transcription and 19× faster at it. Knowing precisely which job the model does that Vision cannot is what turns "we used AI" into a decision I can defend in a design review.

**Delete the work that didn't land.** The fine-tune left behind a working label renderer, a print pipeline, a review server and a scoring contract. Real code, none of it wrong. I removed it anyway, in one commit, because a repository that keeps its abandoned branches lying around tells every future reader that a workstream is still open.

**Ten days is enough to answer a real question**, if the question has a pass bar attached before the work starts. Six tests, one image and one prompt each, a stated threshold. Not a vague goal of trying AI on field service.

What is still open is narrower than where I started. Latency at full resolution is closed. Thermal behaviour over a long session is untested and matters more on a base A18 than a Pro. Prompt templating needs designing so phrasing cannot sabotage a technician. And the iPad and Pro comparison needs hardware I do not own.

The model can do the work, on the device that matters. That was the question, and it has an answer.

Views my own.
