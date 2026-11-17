---
title: "Interactive Facial Control Systems for Audio Processing"
excerpt: "Mouth tracking as a musical controller: webcam → MediaPipe → OSC into Reaper, live audio effects in Python, and a webcam-driven generative ambient patch in Pure Data."
collection: portfolio
tier: more
order: 3
categories: ["Interactive"]
tech: ["Python", "OpenCV", "MediaPipe", "OSC", "Pure Data", "Reaper"]
year: "2024–2025"
header:
  teaser: "/files/interactive/face_tracking_osc_reaper.png"
  card: "/files/cards/interactive-facial-control.jpg"
links:
  - label: "GitHub"
    url: "https://github.com/alexhojinpark/interactive-system"
  - label: "Demo / files"
    url: "https://drive.google.com/drive/folders/1NtCPxzh1aLKPXw2kFC5cW-JI-7fwkCWN?usp=drive_link"
---
{% include base_path %}

## Overview

Can your face be a musical instrument? This project explores interactive systems that use facial tracking, mostly mouth opening and closing, to control audio effects and generate sound.

My first idea was a wah-effect MIDI controller: Csound DSP driven by a real-time Python webcam app. When that ran into problems, I pivoted to a series of three research builds, each testing a different way of turning facial movement into musical control.

## 1. Face tracking to OSC (Python → Reaper)

<figure class="doc-figure">
  <img src="{{ base_path }}/files/interactive/face_tracking_osc_reaper.png" alt="Mouth tracking to OSC: a MediaPipe face mesh with the mouth gap measured, sending /mouth values over OSC on port 8000 to Reaper, where they control a ReaSynth instance" loading="lazy">
</figure>

A webcam tracks the mouth with MediaPipe's facial landmarks and sends the opening as OSC messages (`/mouth`, 0–127). Reaper receives them as a control surface and maps them to plugin parameters, here a ReaSynth instance, through parameter learning.

- **Real-time face tracking** with MediaPipe landmarks and OpenCV, at about 30 FPS
- **Per-user calibration** that captures closed-mouth and open-mouth positions, so the control range fits each face
- **Rate-limited OSC** (configurable, up to 30 messages per second) for smooth control without flooding the DAW
- **Visual feedback:** live preview, mouth-gap readout, OSC target and message rate

## 2. Direct audio processing (Python)

The second build drops the DAW and processes an audio file directly, in real time, from mouth movement:

- **Reverb:** spatial depth grows with mouth openness
- **Low-pass filter:** cutoff follows mouth position
- **Distortion:** amount follows mouth openness

It uses PyAudio with buffer management for low latency, keyboard controls to switch effects and recalibrate, and an FPS counter to watch performance.

**What I learned:** distortion was clearly audible, but reverb and filtering were subtle. This showed the limits of Python for real-time DSP, and how much the parameter mapping matters for an effect to feel responsive.

## 3. Generative ambient with a webcam (Pure Data)

<figure class="doc-figure">
  <img src="{{ base_path }}/files/interactive/pd_generative_ambient.png" alt="Pure Data patch experiment-2.pd: a Gem webcam chain (pix_video, pix_grey, pix_blob) driving three oscillators, plus several randomized loop voices with phasor, low-pass filter and delay" loading="lazy">
</figure>

A composition-focused take, built from scratch in Pure Data:

- **Webcam-controlled oscillators** (top left): Gem's `pix_video → pix_grey → pix_blob` tracks movement, and the blob position and size drive three `osc~` voices.
- **Randomized loop voices:** each has its own note, a randomized loop length and note length, and a `phasor~ → lop~` voice with delay, so the texture keeps evolving.
- Designed for both composition and live performance.

## What it covered

- **Computer vision:** real-time landmark tracking, calibration, keeping latency low
- **Audio:** real-time DSP for several effects, buffer management, mapping control signals to perceptually meaningful parameters
- **Protocols:** OSC message formatting, rate limiting
- **Interaction design:** visual feedback, gesture plus keyboard input, simple calibration
- **Integration:** Python trackers driving a DAW, standalone processing apps, and custom Pd patches
