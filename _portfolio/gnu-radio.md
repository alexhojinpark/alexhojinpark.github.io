---
title: "Open Source: GNU Radio 4 & openFrameworks"
excerpt: "Templated bitwise logic blocks with unit tests for GNU Radio 4, and the Korean Xcode setup guide for openFrameworks."
collection: portfolio
tier: more
order: 6
categories: ["Open Source"]
tech: ["C++", "Templates", "Boost.UT", "CMake", "CI", "Docs"]
year: "2023–2025"
links:
  - label: "GNU Radio 4 PR #536"
    url: "https://github.com/fair-acc/gnuradio4/pull/536"
  - label: "openFrameworks Xcode guide (KO)"
    url: "https://openframeworks.cc/ko/setup/xcode/"
---
{% include base_path %}

## GNU Radio 4: bitwise logic blocks

<table class="facts">
  <tr><th>Project</th><td><a href="https://github.com/fair-acc/gnuradio4">fair-acc/gnuradio4</a>, the next-generation GNU Radio runtime</td></tr>
  <tr><th>Pull request</th><td><a href="https://github.com/fair-acc/gnuradio4/pull/536">#536: Add templated bitwise AND block with tests</a> (refs issue #161)</td></tr>
  <tr><th>Size</th><td>14 files, +395 / −24</td></tr>
  <tr><th>Status</th><td>Closed without merging. A maintainer recommended consolidating it into a broader block-family PR.</td></tr>
</table>

I ported GNU Radio 3.10's bitwise operations to GNU Radio 4 as **templated C++ blocks**, so one implementation supports multiple integer types:

- `And`, `AndConst`, `Or`, `Xor` and `Not` blocks in `blocks/basic`
- A **Boost.UT unit test** for each block, covering basic operations, edge cases and boundary conditions
- CMake wiring for the blocks and tests

Working through review taught me the upstream workflow of a large C++ project: CI and `clang-format` (Restyled) checks, rebasing a fork against a fast-moving `main`, and taking maintainer feedback.

## openFrameworks: Korean Xcode setup guide

<table class="facts">
  <tr><th>Project</th><td><a href="https://openframeworks.cc/">openFrameworks</a>, the open-source C++ toolkit for creative coding</td></tr>
  <tr><th>Page</th><td><a href="https://openframeworks.cc/ko/setup/xcode/">Xcode setup guide (Korean)</a> · <a href="https://openframeworks.cc/setup/xcode/">English original</a></td></tr>
  <tr><th>Pull requests</th><td><a href="https://github.com/openframeworks/ofSite/pull/815">ofSite #815</a> and <a href="https://github.com/openframeworks/ofSite/pull/816">#816</a>, both merged (Dec 2023)</td></tr>
</table>

I updated the Korean translation of the official Xcode setup guide to match the current English version (+126 / −85 lines). In a follow-up PR I fixed the image paths so the guide's screenshots display correctly. Korean-speaking beginners now get an up-to-date first step into openFrameworks.
