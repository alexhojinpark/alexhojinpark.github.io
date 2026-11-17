---
title: "Csound Lab"
excerpt: "57 Csound instruments and effects across 10+ synthesis methods: FM, granular, FOF formant, phase vocoder, MPE and an envelope-following auto-wah."
collection: portfolio
tier: more
order: 1
categories: ["DSP & Plugins"]
tech: ["Csound", "CsoundQt", "Cabbage", "MIDI / MPE"]
year: "2024–2025"
links:
  - label: "GitHub"
    url: "https://github.com/alexhojinpark/csound-lab"
---
## Overview

A collection of sound synthesis techniques implemented in Csound, organized by method, from basic oscillators to granular clouds, formant voices and spectral processing. It has 57 `.csd` instruments and studies, many with MIDI or GUI control. Most run directly in Csound 6+ or in CsoundQt for interactive control.

## Featured: envelope-following auto-wah

An auto-wah that moves a resonant filter with the dynamics of the playing, with no pedal or manual control. It's tested on bass, electric guitar and piano recordings.

1. **Input:** stereo audio file (`diskin2`)
2. **Envelope following:** track each channel's amplitude (`follow2`, with attack and release)
3. **Envelope processing:** scale and shape the envelope at control rate (`downsamp`)
4. **Frequency mapping:** map the envelope to a filter frequency range
5. **Resonant band-pass filtering:** the "wah" sweep
6. **Dry/wet mix**, then stereo output

## What's inside

| Folder | Techniques | Highlights |
|---|---|---|
| **Basic synthesis** | Oscillators, envelopes, noise, panning | Siren glissando |
| **Wavetable** | Table generation, phase accumulation, real-time morphing | GUI wavetable controller, arpeggiators |
| **Additive** | Harmonic partials, per-partial envelopes | Risset bell, Hammond tonewheel organ, Shepard-tone illusion |
| **Subtractive** | Moog-style and resonant filters, formant filtering | Real-time and file-based vocoders, percussion synthesis |
| **FM** | Chowning carrier/modulator ratios, index envelopes | Chowning FM brass |
| **Phase modulation & waveshaping** | PM, waveshaping, ring modulation | PM/FM hybrid, stacked PM, MIDI PM synth |
| **Granular** | Synchronous and asynchronous grains, clouds | Live granular performance and ambient instruments |
| **Formant (FOF)** | Vocal formant modeling, bandwidth control | FOF tenor voice with GUI controls |
| **Spectral** | Phase vocoder (PVOC) | Spectral additive, MIDI spectral processing |
| **MIDI performance** | Expression mapping, MIDI modes | MPE controller, performance synth |
| **Live performance** | Live-coding setup | Performance template |

<!--
TODO (hidden from the site):

- [ ] Short audio clip per highlight (render the auto-wah dry/wet like the VLC page)
- [ ] 12_Compositions README lists two .csd files that are not in the repo yet
-->
