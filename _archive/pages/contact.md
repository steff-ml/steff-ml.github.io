---
title: "Contact"
layout: single
permalink: /contact/
author_profile: true
---

I'm always up for a conversation about data infrastructure in bioprocess,
chemical manufacturing, or the life sciences — a project idea, a correction to
something I've built or written, or a job.

- **Email:** [{{ site.email }}](mailto:{{ site.email }})
{% for link in site.author.links %}- **{{ link.label }}:** [{{ link.url }}]({{ link.url }})
{% endfor %}
