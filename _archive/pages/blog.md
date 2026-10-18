---
title: "Engineering Tips and Tricks"
layout: single
permalink: /blog/
author_profile: true
---

Hardcore technical write-ups for fellow data engineers — Databricks, pipelines, the how. Written for someone deciding whether to trust your engineering, not someone deciding whether to hire you — that's what [Projects]({{ "/projects/" | relative_url }}) is for.

{% assign engineering_posts = site.posts | where_exp: "post", "post.categories contains 'engineering'" | sort: "date" | reverse %}

{% if engineering_posts.size > 0 %}
<div class="search-box">
  <svg viewBox="0 0 20 20"><circle cx="9" cy="9" r="6.5"/><line x1="14" y1="14" x2="18" y2="18"/></svg>
  <input type="text" id="post-search" placeholder="Search posts by title or tag…" oninput="applyPostFilters()">
</div>

{% assign all_tags = "" | split: "" %}
{% for post in engineering_posts %}
  {% assign all_tags = all_tags | concat: post.tags %}
{% endfor %}
{% assign unique_tags = all_tags | uniq | sort %}

<div class="filter-chips" id="post-chips">
  <button class="filter-chip active" data-tag="all" onclick="togglePostChip(this)">All</button>
  {% for tag in unique_tags %}
  <button class="filter-chip" data-tag="{{ tag }}" onclick="togglePostChip(this)">{{ tag }}</button>
  {% endfor %}
</div>

<p class="results-count" id="post-count"></p>
<p class="search-empty" id="post-search-empty">No posts match that search.</p>

<div class="entries-list" id="post-rows">
  {% for post in engineering_posts %}
  <div class="post-filter-item" data-search="{{ post.title }} {{ post.tags | join: ' ' }}" data-tags="{{ post.tags | join: ' ' }}">
    {% include archive-single.html type="list" %}
  </div>
  {% endfor %}
</div>

<script>
function postSearchQuery() { return (document.getElementById('post-search').value || '').trim().toLowerCase(); }
function postActiveTag() {
  var active = document.querySelector('#post-chips .filter-chip.active');
  return active ? active.getAttribute('data-tag') : 'all';
}
function applyPostFilters() {
  var q = postSearchQuery();
  var tag = postActiveTag();
  var items = document.querySelectorAll('#post-rows .post-filter-item');
  var visible = 0;
  items.forEach(function (item) {
    var haystack = (item.getAttribute('data-search') || '').toLowerCase();
    var tags = (item.getAttribute('data-tags') || '').split(' ');
    var matchesText = q === '' || haystack.indexOf(q) !== -1;
    var matchesTag = tag === 'all' || tags.indexOf(tag) !== -1;
    var match = matchesText && matchesTag;
    item.style.display = match ? '' : 'none';
    if (match) visible++;
  });
  var emptyEl = document.getElementById('post-search-empty');
  if (emptyEl) emptyEl.style.display = visible === 0 ? '' : 'none';
  var countEl = document.getElementById('post-count');
  if (countEl) countEl.textContent = visible + (visible === 1 ? ' result' : ' results');
}
function togglePostChip(button) {
  document.querySelectorAll('#post-chips .filter-chip').forEach(function (c) { c.classList.remove('active'); });
  button.classList.add('active');
  applyPostFilters();
}
applyPostFilters();
</script>
{% else %}
<p class="empty-lane">Nothing published in this lane yet.</p>
{% endif %}
