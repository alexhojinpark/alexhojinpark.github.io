---
permalink: /
layout: single
excerpt: "Audio Developer: real-time DSP in C/C++ and game audio middleware."
author_profile: false
redirect_from:
  - /about/
  - /about.html
---

{% include base_path %}

<section class="hero">
  <p class="hero__eyebrow">Audio Developer · Vancouver, Canada</p>
  <h1 class="hero__title">Alex Hojin Park</h1>
  <p class="hero__lead">I’m an audio developer and a live sound engineer.<br>One job writes the algorithms, the other tells me if they actually work.</p>
  <p class="hero__actions">
    <a class="btn-solid" href="{{ base_path }}/portfolio/">View projects</a>
    <a class="btn-pill" href="{{ base_path }}/cv/">Resume</a>
    <a class="btn-pill" href="mailto:alexhojinpark@gmail.com">Email</a>
    <a class="btn-pill" href="https://github.com/alexhojinpark" target="_blank" rel="noopener">GitHub ↗</a>
  </p>
  <div class="now">
    <span class="now__dot" aria-hidden="true"></span>
    <p><strong>Open to work:</strong> I recently completed Google Summer of Code as an audio software developer at <a href="{{ base_path }}/portfolio/vlc-audio-filters/">VideoLAN (VLC)</a>, and I'm now looking for audio software &amp; DSP roles.</p>
  </div>
</section>

<section class="home-section">
  <h2 class="section-label">Featured projects</h2>
  <div class="pgrid pgrid--2">
    {% assign featured = site.portfolio | where: "tier", "featured" | sort: "order" %}
    {% for p in featured %}{% include project-card.html project=p %}{% endfor %}
  </div>
  <p class="more-link"><a href="{{ base_path }}/portfolio/">See all projects →</a></p>
</section>

<section class="home-section">
  <h2 class="section-label">About</h2>
  <p>Most recently I built a 3D stereo widener filter for the <strong>VLC media player</strong> through Google Summer of Code. Before that I commissioned AV systems for Canada's largest hospital project, tested audio for AAA games like <em>Kingdom Come: Deliverance</em> and <em>V Rising</em>, and earned a <strong>first-class MSc in Sound and Music Computing</strong>.</p>
  <p class="more-link"><a href="{{ base_path }}/experience/">Experience &amp; skills →</a></p>
</section>
