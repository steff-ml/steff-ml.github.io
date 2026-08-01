---
title: "Work"
layout: single
permalink: /work/
entries_layout: list
author_profile: true
---

What I'm building: governed data infrastructure for bioprocess, chemical and life-science systems.

{% for mission in site.data.missions %}
<h2 id="{{ mission.slug }}">{{ mission.title }}</h2>
{% if mission.description %}<p>{{ mission.description }}</p>{% endif %}

{% assign mission_projects = site.projects | where: "mission", mission.slug %}
{% if mission_projects.size > 0 %}
<div class="entries-{{ entries_layout | default: 'list' }}">
  {% for post in mission_projects %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>
{% else %}
<p><em>No projects yet.</em></p>
{% endif %}
{% endfor %}