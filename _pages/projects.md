---
title: "Projects"
layout: single
permalink: /projects/
author_profile: true
---

Everything I'm building. Open-source, in progress, and honest about which stage each one is at.

{% assign all_projects = site.projects | sort: "date" | reverse %}

{% if all_projects.size > 0 %}
<div class="search-box">
  <svg viewBox="0 0 20 20"><circle cx="9" cy="9" r="6.5"/><line x1="14" y1="14" x2="18" y2="18"/></svg>
  <input type="text" id="project-search" placeholder="Search projects by title or tag…" oninput="applyProjectFilters()">
</div>

{% assign all_tags = "" | split: "" %}
{% for project in all_projects %}
  {% assign all_tags = all_tags | concat: project.tags %}
{% endfor %}
{% assign unique_tags = all_tags | uniq | sort %}

<div class="filter-chips" id="project-chips">
  <button class="filter-chip active" data-tag="all" onclick="toggleProjectChip(this)">All</button>
  {% for tag in unique_tags %}
  <button class="filter-chip" data-tag="{{ tag }}" onclick="toggleProjectChip(this)">{{ tag }}</button>
  {% endfor %}
</div>

<p class="results-count" id="project-count"></p>
<p class="search-empty" id="project-search-empty">No projects match that search.</p>

<div class="project-row-list" id="project-rows">
  {% for project in all_projects %}
  <a class="project-row" href="{{ project.url | relative_url }}" data-search="{{ project.title }} {{ project.tags | join: ' ' }}" data-tags="{{ project.tags | join: ' ' }}">
    <div class="project-row__thumb-wrap">
      {% if project.header.teaser %}
      <img src="{{ project.header.teaser | relative_url }}" alt="">
      {% else %}
      <span class="project-row__thumb-empty" aria-hidden="true"></span>
      {% endif %}
    </div>
    <div>
      <div class="project-row__meta">
        {% if project.status %}<span class="project-row__status">{{ project.status }}</span>{% endif %}
        {% for tag in project.tags %}<span class="home-tag">{{ tag }}</span>{% endfor %}
      </div>
      <div class="project-row__title">{{ project.title }}</div>
      <p class="project-row__hook">{{ project.excerpt | strip_html }}</p>
    </div>
    <span class="project-row__arrow">→</span>
  </a>
  {% endfor %}
</div>

<script>
function projectSearchQuery() { return (document.getElementById('project-search').value || '').trim().toLowerCase(); }
function projectActiveTag() {
  var active = document.querySelector('#project-chips .filter-chip.active');
  return active ? active.getAttribute('data-tag') : 'all';
}
function applyProjectFilters() {
  var q = projectSearchQuery();
  var tag = projectActiveTag();
  var rows = document.querySelectorAll('#project-rows .project-row');
  var visible = 0;
  rows.forEach(function (row) {
    var haystack = (row.getAttribute('data-search') || '').toLowerCase();
    var tags = (row.getAttribute('data-tags') || '').split(' ');
    var matchesText = q === '' || haystack.indexOf(q) !== -1;
    var matchesTag = tag === 'all' || tags.indexOf(tag) !== -1;
    var match = matchesText && matchesTag;
    row.style.display = match ? '' : 'none';
    if (match) visible++;
  });
  var emptyEl = document.getElementById('project-search-empty');
  if (emptyEl) emptyEl.style.display = visible === 0 ? '' : 'none';
  var countEl = document.getElementById('project-count');
  if (countEl) countEl.textContent = visible + (visible === 1 ? ' result' : ' results');
}
function toggleProjectChip(button) {
  document.querySelectorAll('#project-chips .filter-chip').forEach(function (c) { c.classList.remove('active'); });
  button.classList.add('active');
  applyProjectFilters();
}
applyProjectFilters();
</script>
{% else %}
<p class="empty-lane">Nothing published yet.</p>
{% endif %}
