---
title: "Notes"
permalink: /notes/
author_profile: true
---

Short things I learned or read along the way: TILs and monthly link roundups.

{% assign notes = site.posts | where_exp: "post", "post.categories contains 'til' or post.categories contains 'links'" %}

{% if notes.size > 0 %}
<div class="search-box">
  <svg viewBox="0 0 20 20"><circle cx="9" cy="9" r="6.5"/><line x1="14" y1="14" x2="18" y2="18"/></svg>
  <input type="text" id="note-search" placeholder="Search notes…" oninput="applyNoteFilters()">
</div>

<div class="filter-chips" id="note-chips">
  <button class="filter-chip active" data-kind="all" onclick="toggleNoteChip(this)">All</button>
  <button class="filter-chip" data-kind="til" onclick="toggleNoteChip(this)">TIL</button>
  <button class="filter-chip" data-kind="links" onclick="toggleNoteChip(this)">Links</button>
</div>

<p class="results-count" id="note-count"></p>
<p class="search-empty" id="note-search-empty">No notes match that search.</p>

<ul class="recent-list" id="note-rows">
  {% for post in notes %}
  {% assign kind = post.categories[0] %}
  <li data-kind="{{ kind }}" data-search="{{ post.title }} {{ post.tags | join: ' ' }} {{ post.concepts | join: ' ' }}">
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d" }}</time>
    <span class="recent-kind">{% if kind == "til" %}TIL{% else %}Links{% endif %}</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>

<script>
function noteActiveKind() {
  var active = document.querySelector('#note-chips .filter-chip.active');
  return active ? active.getAttribute('data-kind') : 'all';
}
function applyNoteFilters() {
  var q = (document.getElementById('note-search').value || '').trim().toLowerCase();
  var kind = noteActiveKind();
  var visible = 0;
  document.querySelectorAll('#note-rows li').forEach(function (item) {
    var matchesText = q === '' || (item.getAttribute('data-search') || '').toLowerCase().indexOf(q) !== -1;
    var matchesKind = kind === 'all' || item.getAttribute('data-kind') === kind;
    var match = matchesText && matchesKind;
    item.style.display = match ? '' : 'none';
    if (match) visible++;
  });
  document.getElementById('note-search-empty').style.display = visible === 0 ? '' : 'none';
  document.getElementById('note-count').textContent = visible + (visible === 1 ? ' note' : ' notes');
}
function toggleNoteChip(button) {
  document.querySelectorAll('#note-chips .filter-chip').forEach(function (c) { c.classList.remove('active'); });
  button.classList.add('active');
  applyNoteFilters();
}
applyNoteFilters();
</script>
{% else %}
<p class="home2-empty">No notes yet.</p>
{% endif %}
