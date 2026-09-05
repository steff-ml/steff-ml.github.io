---
title: "Blog"
layout: single
permalink: /blog/
author_profile: true
---

Hardcore engineering tips and tricks — Databricks, pipelines, the how. Not
general reading, and not where project-definition or retrospective posts
live; those stay attached to their own project pages.

{% assign engineering_posts = site.posts | where_exp: "post", "post.categories contains 'engineering'" | sort: "date" | reverse %}
{% if engineering_posts.size > 0 %}
<div class="entries-list">
  {% for post in engineering_posts %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% else %}
<p><em>Nothing published here yet — a Databricks user guide is in progress.</em></p>
{% endif %}
