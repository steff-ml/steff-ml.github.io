---
title: "Product Stages"
layout: single
permalink: /stages/
author_profile: true
---

Every product on this site is tagged with one of three stages. They're not a
progress bar — a product can stay at pretotype forever if the test fails, and
none of this implies the next stage is guaranteed to happen.

<div class="stage-list">
{% for stage in site.data.maturity_stages %}
<div class="stage-list__item">
  <div class="stage-list__title">{{ stage.title }}</div>
  <p class="stage-list__desc">{{ stage.description }}</p>
</div>
{% endfor %}
</div>

The distinction between pretotype and prototype follows Alberto Savoia's
usage: a pretotype tests whether an idea is worth building at all: is there
a real problem and a real audience for a solution to it, tested as cheaply
as possible. A prototype comes after that question is answered "yes" —
it tests *how* to build it, with something someone can actually use.
