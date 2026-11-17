---
title: "JX11: Polyphonic Synthesizer (JUCE)"
excerpt: "A VST/AU polyphonic synth in C++/JUCE: two oscillators, glide, resonant filter with its own envelope, ADSR, LFO and voice management."
collection: portfolio
tier: featured
order: 4
categories: ["DSP & Plugins"]
tech: ["C++", "JUCE", "MIDI", "VST3 / AU"]
year: "2024"
header:
  teaser: "/files/jx11/synth_schematic.jpg"
  card: "/files/cards/jx11-synth.jpg"
links:
  - label: "GitHub"
    url: "https://github.com/alexhojinpark/JX11"
---
{% include base_path %}

## Overview

A polyphonic synthesizer plugin (VST/AU) built with C++ and JUCE. It follows the book [*Creating Synthesizer Plug-Ins with C++ and JUCE*](https://github.com/TheAudioProgrammer/synth-plugin-book/tree/main). The focus was mainly DSP, plus some UI.

## What I implemented

- **Oscillators:** two detunable oscillators with a mix control, plus noise
- **Plug-in parameters:** the full set exposed to the host and automatable
- **Glide:** glide modes, rate and bend
- **Filters:** resonant filter with its own ADSR, LFO and velocity modulation
- **ADSR envelopes** for amplitude and filter
- **Polyphony and voice management:** mono and poly modes

## Signal flow

<figure class="doc-figure">
  <img src="{{ base_path }}/files/jx11/synth_schematic.jpg" alt="Hand-drawn schematic of the synth: MIDI note, pitch bend, glide and tuning set the pitch of two oscillators; the oscillators and noise are mixed into a resonant filter, modulated by an LFO, velocity, key tracking and a filter envelope; then an amplitude envelope and the output amplifier" loading="lazy">
  <figcaption>The full schematic of the synth, from the book.</figcaption>
</figure>

MIDI note, pitch bend, glide and master tuning set the pitch of **two oscillators**, with vibrato/PWM from an LFO or the mod wheel. The oscillators and **noise** are mixed and go into a **resonant filter**. Its cutoff is modulated by the LFO, velocity, key tracking, its own **filter envelope**, MIDI CC, pitch bend and aftertouch. An **amplitude envelope** and the output amplifier finish the voice.

## The plugin

<div class="figure-pair">
  <figure class="doc-figure">
    <img src="{{ base_path }}/files/jx11/ui_1.png" alt="JX11 plugin window, top half: polyphony mode, oscillator tune, fine and mix, glide mode, rate and bend, filter frequency, resonance, envelope, LFO and velocity" loading="lazy">
  </figure>
  <figure class="doc-figure">
    <img src="{{ base_path }}/files/jx11/ui_2.png" alt="JX11 plugin window, bottom half: filter ADSR, amplitude ADSR, LFO rate, vibrato, noise, octave, tuning and output level" loading="lazy">
  </figure>
</div>

## What I learned

Synthesizer DSP, MIDI handling and real-time audio programming in C++. Adding, testing and listening to each feature was educational and fun.

<!--
TODO (hidden from the site):

- [ ] Short audio demo
-->
