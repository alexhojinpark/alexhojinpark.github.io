---
title: "VLC 3D Stereo Widener (GSoC 2026)"
excerpt: "An SRS WoW-style stereo widener with psychoacoustic bass (TruBass) for VLC, written in C. In review as MR !9573."
collection: portfolio
tier: featured
order: 1
categories: ["Open Source", "DSP & Plugins"]
tech: ["C", "VLC module API", "DSP", "M/S processing", "Biquads"]
year: "2026"
header:
  teaser: "/files/vlc-srs-wow/ui_preferences.png"
links:
  - label: "Final documentation"
    url: "https://code.videolan.org/ahp1/gsoc-2026-final-documentation"
  - label: "Merge request !9573"
    url: "https://code.videolan.org/videolan/vlc/-/merge_requests/9573"
  - label: "Source"
    url: "https://code.videolan.org/ahp1/vlc/-/blob/feat/stereo-widening/modules/audio_filter/stereo_srs_wow.c"
  - label: "GSoC project"
    url: "https://summerofcode.withgoogle.com/programs/2026/projects/01ooIHOz"
---
{% include base_path %}

<!-- Adapted from https://code.videolan.org/ahp1/gsoc-2026-final-documentation (README.md). -->

<table class="facts">
  <tr><th>Organization</th><td>VideoLAN (VLC)</td></tr>
  <tr><th>Mentor</th><td>Jean-Baptiste Kempf</td></tr>
  <tr><th>Reviewers</th><td>Robert Stone, Thomas Guillem, Steve Lhomme</td></tr>
  <tr><th>Status</th><td>In review as <a href="https://code.videolan.org/videolan/vlc/-/merge_requests/9573">!9573</a>. The core approach is approved by a maintainer; not yet merged into master.</td></tr>
</table>

## Summary

This project adds a new audio effect to VLC that makes sound feel more spacious. It widens the stereo image so music sounds wider and more open, and it makes the low end feel deeper, even on small speakers that cannot physically reproduce deep bass.

Technically, it is a stereo widening filter (SRS WoW style) built on Mid/Side decomposition, with an optional psychoacoustic bass enhancement stage (TruBass).

## Hear it

The same 15-second excerpt, with the filter off and on. Listen back to back on headphones or stereo speakers. The clips are loudness matched (equal RMS), so the difference you hear is the effect, not a change in volume.

<div class="audio-ab">
  <figure>
    <figcaption>Filter off <span>original audio</span></figcaption>
    <audio controls preload="metadata" src="{{ base_path }}/files/vlc-srs-wow/srs_OFF.mp3"></audio>
  </figure>
  <figure>
    <figcaption>Filter on <span>widening 1.0 · side shelf 10 dB · TruBass on</span></figcaption>
    <audio controls preload="metadata" src="{{ base_path }}/files/vlc-srs-wow/srs_ON.mp3"></audio>
  </figure>
</div>

On the ON clip, instruments spread further left and right, while the centre vocal and the bass stay in place.

| | Side/Mid width | L/R correlation |
|---|---|---|
| Off | 0.197 | 0.925 |
| On | 0.528 | 0.564 |

A higher Side/Mid ratio and a lower correlation both mean a wider image.

## Technical approach

The filter widens the perceived stereo image and adds perceived low end, using a Mid/Side (M/S) approach:

1. **M/S decomposition.** The stereo input is split into Mid `M = (L + R) / 2` (content common to both channels, such as vocals and bass) and Side `S = (L - R) / 2` (the difference, which carries the stereo image). Boosting the Side widens the image while leaving the centre content unchanged.
2. **Bass mono-lock.** Low frequencies are removed from the Side with a high-pass filter, so the low end stays mono. The Side cancels when a system sums to mono (`L + R = 2M`), so widened bass would disappear on mono playback, and low frequencies carry no directional cue anyway.
3. **High-shelf widening.** Above the mono-lock frequency the Side is boosted with a high-shelf EQ. The same frequency is the crossover: mono below, widened above. A widening amount of zero gives a flat shelf, which is a true bypass.
4. **TruBass (psychoacoustic bass).** Instead of boosting deep bass that small speakers cannot reproduce, the filter synthesises the harmonics of the bass so the ear reconstructs the missing fundamental. The bass is isolated with a low-pass pair, shaped by a nonlinear waveshaper that generates the 2nd and 3rd harmonics, cleaned by a high-pass pair that removes DC and the original fundamental, and levelled by a dual follower so the effect stays consistent across loud and quiet passages.
5. **Re-matrix and soft limiter.** Mid, Side and bass are recombined into L and R. A `tanh` soft limiter keeps the stacked gains from clipping while staying transparent at normal levels.

Filter coefficients follow the RBJ Audio EQ Cookbook and use a Transposed Direct Form II biquad.

## The filter in VLC

The filter appears in VLC preferences as "SRS WoW" in the audio filters list. All five parameters can be adjusted live during playback.

<figure class="doc-figure">
  <img src="{{ base_path }}/files/vlc-srs-wow/ui_preferences.png" alt="VLC preferences panel showing the SRS WoW filter with its five sliders" loading="lazy">
</figure>

| Parameter | Range | What it does |
|---|---|---|
| Widening amount | 0.0 to 1.0 | How wide the stereo image becomes; scales the Side boost. 0.0 is a true bypass. |
| Bass mono-lock frequency | 20 to 800 Hz | Frequencies below this stay mono. Also the point above which widening is applied. |
| Side channel shelf gain | 0 to 18 dB | How strongly the Side is boosted above the mono-lock frequency. |
| TruBass amount | 0.0 to 1.0 | Strength of the psychoacoustic bass enhancement. 0.0 is off. |
| TruBass crossover frequency | 40 to 200 Hz | Bass below this frequency is used to generate the harmonics. |

## Measured effect

The behaviour was verified by running the exact filter DSP, outside VLC, on controlled test signals and comparing spectra, rather than by listening alone.

<figure class="doc-figure doc-figure--dark">
  <img src="{{ base_path }}/files/vlc-srs-wow/srs_wow_spectrum_dark.svg" alt="Two spectrum charts, filter off versus on: bass harmonics synthesised by TruBass, and the Side channel widened above the bass mono-lock frequency" loading="lazy">
</figure>

- **TruBass (top).** With a pure 60 Hz tone as input, the filter synthesises new harmonics at 120, 180 and 240 Hz while leaving the 60 Hz fundamental in place. On a pure tone the increase is large (about +78 dB at 120 Hz, since nothing was there before); on full music the same harmonics are more subtle.
- **Widening (bottom).** With decorrelated stereo noise, the Side channel is cut below the mono-lock frequency (200 Hz here) and raised above it by the configured shelf gain.

Because the test signals are simple and fixed, the measurements are exact and repeatable.

## What I learned

- **Thread safety for live parameters.** An early version used one atomic per parameter. My mentor rejected it: atomics protect each variable on its own, but several parameters combine into whole filter coefficients and must be read as a group. The fix was a single mutex guarding the parameters and coefficients as a unit, with coefficients recomputed on the audio thread inside the lock (the pattern VLC's equalizer uses).
- **Real filters, not subtractions.** A high-pass built as `signal - lowpass` only behaves for a first-order filter; at second order it colours the signal. Review required real high-pass and low-pass stages, cascaded where a steeper slope was needed.
- **True bypass.** At zero amount the whole chain is skipped and the block returned untouched, instead of relying on identity coefficients that still leave later stages in the path.
- **Verify DSP with measurements.** Running the exact DSP on known signals produced the before/after numbers above and caught issues early.
- **Upstream review culture.** Idiomatic, readable code, concise comments, minimal forward declarations and consistent style. Adopting reviewer-supplied changes directly and keeping commits scoped made review faster.

## Future work

- **Migrate to a state variable filter (SVF) library.** RBJ biquads click when their coefficients are swapped during a live parameter change, because biquad state only makes sense for one coefficient set. An SVF can ramp smoothly between settings. VLC contributor Robert Stone proposed a small SVF DSP library; the plan is to land this filter first, then migrate in a follow-up MR.
- **Stronger, optional low end.** A dedicated low-shelf boost for users who want more raw bass, alongside TruBass.
- **More TruBass tuning** and listening tests across a wider range of material.
