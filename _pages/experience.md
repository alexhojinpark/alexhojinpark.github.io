---
layout: single
title: "Experience"
permalink: /experience/
author_profile: false
---

{% include base_path %}

<p class="page-intro">Audio software, game industry and live sound. The full timeline is below, or <a href="{{ base_path }}/cv/">see the one-page resume</a>.</p>

{% for g in site.data.experience.groups %}
<section class="xp">
  <h2 class="section-label">{{ g.title }}</h2>
  {% for e in g.entries %}
  <div class="xp__row">
    <p class="xp__dates">{{ e.dates }}</p>
    <div class="xp__body">
      <h3 class="xp__role">{{ e.role }}</h3>
      <p class="xp__org">{% if e.url %}<a href="{{ e.url }}" target="_blank" rel="noopener">{{ e.org }}</a>{% else %}{{ e.org }}{% endif %}</p>
      <ul class="xp__points">
        {% for pt in e.points %}<li>{{ pt | markdownify | remove: '<p>' | remove: '</p>' | strip }}</li>{% endfor %}
      </ul>
      {% if e.project %}<p class="xp__project"><a href="{{ base_path }}{{ e.project }}">Related project →</a></p>{% endif %}
    </div>
  </div>
  {% endfor %}
</section>
{% endfor %}

<section class="xp">
  <h2 class="section-label">Skills</h2>
  <div class="skills">
    {% for s in site.data.experience.skills %}
    <div class="skills__row">
      <p class="skills__label">{{ s.label }}</p>
      <ul class="chips">{% for i in s.items %}<li>{{ i }}</li>{% endfor %}</ul>
    </div>
    {% endfor %}
  </div>
</section>
