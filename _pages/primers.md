---
title: "Primers"
layout: single
permalink: /primers/
author_profile: true
---

Short explainers of a single concept or system, each written for whichever
side of the data-engineering/bio divide it's unfamiliar to. Filter by who
you are to find the ones aimed at you.

<div class="primer-filters" role="group" aria-label="Filter primers by audience">
  <button type="button" class="primer-filter is-active" data-filter="all">All</button>
  {% for audience in site.data.audiences %}
  <button type="button" class="primer-filter" data-filter="{{ audience.slug }}">{{ audience.title }}</button>
  {% endfor %}
</div>

<div class="primer-list">
{% assign primers = site.posts | where_exp: "post", "post.categories contains 'primer'" %}
{% for post in primers %}
  <article class="primer-entry" data-audiences="{{ post.audience | join: ' ' }}">
    <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
    <p class="primer-entry__date">{{ post.date | date: "%B %-d, %Y" }}</p>
    {% if post.excerpt %}<p>{{ post.excerpt | strip_html }}</p>{% endif %}
    <p class="primer-entry__audiences">
      {% for slug in post.audience %}
        {% assign audience = site.data.audiences | where: "slug", slug | first %}
        {% if audience %}<span class="primer-badge">{{ audience.title }}</span>{% endif %}
      {% endfor %}
    </p>
  </article>
{% endfor %}
</div>

<p class="primer-empty-state" style="display:none;">No primers for this audience yet.</p>

<style>
.primer-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5em;
  margin: 1.5em 0;
}
.primer-filter {
  border: 1px solid #8888;
  background: transparent;
  border-radius: 999px;
  padding: 0.35em 1em;
  font-size: 0.9em;
  cursor: pointer;
}
.primer-filter.is-active {
  background: #6b7280;
  color: #fff;
  border-color: #6b7280;
}
.primer-entry {
  padding: 1em 0;
  border-bottom: 1px solid #8884;
}
.primer-entry__date {
  font-size: 0.85em;
  opacity: 0.7;
  margin: 0.1em 0 0.5em;
}
.primer-badge {
  display: inline-block;
  font-size: 0.75em;
  border-radius: 999px;
  padding: 0.15em 0.7em;
  margin: 0.2em 0.3em 0.2em 0;
  background: #8882;
}
</style>

<script>
(function () {
  var filters = document.querySelectorAll(".primer-filter");
  var entries = document.querySelectorAll(".primer-entry");
  var emptyState = document.querySelector(".primer-empty-state");

  filters.forEach(function (button) {
    button.addEventListener("click", function () {
      filters.forEach(function (b) { b.classList.remove("is-active"); });
      button.classList.add("is-active");

      var filter = button.getAttribute("data-filter");
      var visibleCount = 0;

      entries.forEach(function (entry) {
        var audiences = (entry.getAttribute("data-audiences") || "").split(" ");
        var show = filter === "all" || audiences.indexOf(filter) !== -1;
        entry.style.display = show ? "" : "none";
        if (show) visibleCount++;
      });

      emptyState.style.display = visibleCount === 0 ? "" : "none";
    });
  });
})();
</script>
