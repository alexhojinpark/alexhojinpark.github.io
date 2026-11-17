---
title: "MetaSound DSP Nodes for Unreal Engine 5"
excerpt: "Custom C++ MetaSound nodes (stereo delay, lowpass, pan/gain) plus fully procedural sound-design patches in UE 5.5."
collection: portfolio
tier: featured
order: 3
categories: ["Game Audio", "DSP & Plugins"]
tech: ["C++", "Unreal Engine 5", "MetaSound"]
year: "2025"
header:
  teaser: "/files/metasound/custom_nodes.png"
  card: "/files/cards/metasound-nodes.jpg"
links:
  - label: "GitHub"
    url: "https://github.com/alexhojinpark/ap_metasound"
  - label: "Demo"
    url: "https://drive.google.com/drive/folders/1GZph8bLXBqeOceNSVdlN6Ahhsv-IpQpo?usp=drive_link"
---
{% include base_path %}

<table class="facts">
  <tr><th>Context</th><td>MSc final projects, Maynooth University: MU620 Music Systems Programming (custom nodes) and MU616 (signal processing &amp; procedural sound design)</td></tr>
  <tr><th>Engine</th><td>Unreal Engine 5.5.4, C++, MetaSound</td></tr>
  <tr><th>Focus</th><td>Signal processing, custom node integration, procedural sound design</td></tr>
</table>

## Overview

Two connected parts:

1. **A C++ plugin of custom MetaSound nodes**, built with the Unreal Engine SDK. Each node's design comes from DSP techniques I studied and prototyped in the Music Systems Programming module; the goal was to turn those concepts into production-ready nodes for real-time audio in Unreal.
2. **Procedural MetaSound patches** that use those nodes and Unreal's built-in synthesis to make interactive, fully synthesized sound, with no recorded samples.

## Custom nodes

<figure class="doc-figure">
  <img src="{{ base_path }}/files/metasound/custom_nodes.png" alt="The three custom MetaSound nodes in the Unreal editor: Stereo Delay, Stereo Lowpass Filter and Stereo Pan Gain, with their inputs and default values" loading="lazy">
</figure>

| Node | What it does | Parameters |
|---|---|---|
| **Stereo Delay** | Independent left/right delay buffers | Delay Time (ms), Feedback, Wet/Dry Mix |
| **Stereo Lowpass Filter** | Attenuates content above the cutoff, shaping brightness | Cutoff Frequency (Hz) |
| **Stereo Pan Gain** | Stereo balance and amplitude | Gain (linear), Pan (-1.0 left to +1.0 right) |

All three take stereo in and give stereo out. The plugin source lives in `Plugins/ap/Source/ap` in the repo.

### Using the nodes

<figure class="doc-figure doc-figure--small">
  <img src="{{ base_path }}/files/metasound/node_browser.png" alt="MetaSound node browser filtered to the Alex Custom category, listing Stereo Delay, Stereo Lowpass Filter and Stereo Pan Gain" loading="lazy">
</figure>

Clone the repo and open the `ap_metasound` Unreal project. The nodes appear in the MetaSound editor under the **Alex Custom** category.

## Procedural MetaSound patches

**MSS_SoundTest:** a testbed for the custom nodes. It plays a stereo music sample through the Stereo Delay → Stereo Lowpass → Stereo Pan Gain chain, to validate signal flow and check that parameters respond in real time.

**MSS_Music:** procedural music synthesized in real time.

- Oscillator-based sound generation
- ADSR envelopes for note shaping
- Dynamic crossfading between sound layers
- Filtering and delay for tonal colour
- No audio samples: everything is synthesized

**MSS_MotorCycle:** a motorcycle engine designed entirely through synthesis.

- Purely additive synthesis (multiple harmonics and layers)
- Parameter-driven, RPM-like behaviour
- Subtle mixing and filtering for realism
- No pre-recorded samples: 100% procedural

The [demo folder](https://drive.google.com/drive/folders/1GZph8bLXBqeOceNSVdlN6Ahhsv-IpQpo?usp=drive_link) has a walkthrough of each patch.

<!--
TODO (hidden from the site):

- [ ] Embed the demo video
-->
