---
title: "Concepts"
permalink: /concepts/
author_profile: true
---

Short pages that each explain one idea. Posts link here instead of explaining it again.

<div class="search-box">
  <svg viewBox="0 0 20 20"><circle cx="9" cy="9" r="6.5"/><line x1="14" y1="14" x2="18" y2="18"/></svg>
  <input type="text" id="concept-search" placeholder="Search all concepts…" oninput="applyConceptSearch()">
</div>

<p class="results-count" id="concept-count"></p>
<p class="search-empty" id="concept-search-empty">No concepts match that search.</p>

<div id="concept-rows">
{% for area in site.data.concepts %}
{% assign area_page = site.pages | where: "group", area.slug | first %}
{% assign area_concepts = site.concepts | where: "group", area.slug | sort: "title" %}
{% if area_concepts.size > 0 %}
<section class="concept-group" data-group>
  <h2 class="concept-group__title"><a href="{{ area_page.url | relative_url }}">{{ area.title }}</a></h2>
  <ul class="concept-index">
    {% for concept in area_concepts %}
    <li data-search="{{ concept.title }} {{ concept.excerpt | strip_html }} {{ concept.tags | join: ' ' }}">
      <a href="{{ concept.url | relative_url }}">{{ concept.title }}</a>
      {% if concept.excerpt %}<span class="concept-index__hook">{{ concept.excerpt | strip_html }}</span>{% endif %}
    </li>
    {% endfor %}
  </ul>
</section>
{% endif %}
{% endfor %}
</div>

<script>
function applyConceptSearch() {
  var q = (document.getElementById('concept-search').value || '').trim().toLowerCase();
  var visible = 0;
  document.querySelectorAll('#concept-rows [data-group]').forEach(function (group) {
    var inGroup = 0;
    group.querySelectorAll('li').forEach(function (item) {
      var match = q === '' || (item.getAttribute('data-search') || '').toLowerCase().indexOf(q) !== -1;
      item.style.display = match ? '' : 'none';
      if (match) inGroup++;
    });
    group.style.display = inGroup > 0 ? '' : 'none';
    visible += inGroup;
  });
  document.getElementById('concept-search-empty').style.display = visible === 0 ? '' : 'none';
  document.getElementById('concept-count').textContent = visible + (visible === 1 ? ' concept' : ' concepts');
}
applyConceptSearch();
</script>
