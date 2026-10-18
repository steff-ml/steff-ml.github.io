---
layout: single
permalink: /
title: "Steff Horemans"
excerpt: "I build the data infrastructure that lets AI put capability in the hands of scientists and engineers."
author_profile: false
hide_title: true
full_width: true
---

{% comment %} The intro at the top of the main column. Plain Markdown. {% endcomment %}
{% capture intro %}
## Hi, I'm Steff.

AI should put capability in the hands of scientists and engineers solving important problems. I build the data infrastructure that lets it, without locking anyone into a single provider. I'm not against the big AI labs. I'm against lock-in.

I'm a data engineer. I design and build governed data platforms on Databricks, from the first intake to pipelines, quality checks, access control and lineage in production.

I also:

- Run an **independent AI stack**: a gateway that routes between frontier, open and European models, so I can switch without rebuilding.
- Write about **designing AI systems I can explain and trust**, with ideas from [systems thinking]({{ '/concepts/systems/' | relative_url }}) such as Gall's law and single points of failure.
- Apply both to **rare-disease data**: finding the data that exists and making it usable.

Steff Horemans is a data engineer with a PhD in the life sciences and six years of building data products, from Tableau dashboards for a pharmaceutical client to data platforms on Databricks. Before that, Steff studied how a cell's metabolism drives DNA replication. Based in Antwerp, Belgium.
{% endcapture %}

<div class="home2-layout">
  <aside class="home2-sidebar">
    <img class="home2-avatar" src="{{ site.author.avatar | relative_url }}" alt="{{ site.author.name }}">
    <div class="home2-identity">
      <h1>{{ site.author.name }}</h1>
      <p class="home2-tagline">From a scientist that generated new data to an engineer that makes it useful at scale.</p>
      <p class="home2-credentials">PhD life sciences · MSc synbio · industrial data and OR</p>
    </div>
    <div class="home2-contact-row">
      <a href="mailto:{{ site.email }}"><span class="home2-icon">✉️</span> Email</a>
      {% assign github_link = site.author.links | where: "label", "Github" | first %}
      {% if github_link %}<a href="{{ github_link.url }}"><span class="home2-icon">💻</span> GitHub</a>{% endif %}
      <a href="{{ '/contact/' | relative_url }}"><span class="home2-icon">📅</span> Talk to me</a>
    </div>
    <a class="home2-story-btn" href="{{ '/about/' | relative_url }}">Read my full story →</a>
  </aside>

  <main class="home2-main">

    <!-- 1. Intro (text: the capture block at the top) -->
    <section class="home-intro">{{ intro | markdownify }}</section>

    <!-- 2. The current project -->
    <section class="section" id="products">
      <div class="section-head">
        <h3>Products</h3>
        <a class="see-all" href="{{ '/projects/' | relative_url }}">See all products →</a>
      </div>

      {% assign latest_projects = site.projects | sort: "date" | reverse %}
      {% if latest_projects.size > 0 %}
        {% for project in latest_projects limit: 1 %}
        <a class="product-card" href="{{ project.url | relative_url }}">
          <div class="product-card__icon">🧬</div>
          <div class="product-card__body">
            <div class="product-card__top">
              <span class="product-card__title">{{ project.title }}</span>
              {% if project.stage %}
                {% assign proj_stage = site.data.maturity_stages | where: "slug", project.stage | first %}
                {% if proj_stage %}<span class="stage-badge stage-badge--{{ proj_stage.slug }}">{{ proj_stage.title | downcase }}</span>{% endif %}
              {% endif %}
            </div>
            <p class="product-card__hook">{{ project.excerpt | strip_html }}</p>
            <span class="product-card__cta">Open the project →</span>
          </div>
        </a>
        {% endfor %}
        <p class="stage-legend">{% for stage in site.data.maturity_stages %}{{ stage.title | downcase }}{% unless forloop.last %} · {% endunless %}{% endfor %} — <a href="{{ '/stages/' | relative_url }}">what each stage means →</a></p>
      {% else %}
      <p class="home2-empty">Nothing published yet.</p>
      {% endif %}
    </section>

    <!-- 3. Concept areas: one link per area in _data/concepts.yml -->
    <section class="section" id="concepts">
      <div class="section-head">
        <h3>Concepts</h3>
      </div>
      <div class="concept-areas">
        {% for group in site.data.concepts %}
        {% assign group_page = site.pages | where: "group", group.slug | first %}
        {% assign group_count = site.concepts | where: "group", group.slug | size %}
        <a class="concept-area" href="{{ group_page.url | relative_url }}">{{ group.title }}{% if group_count > 0 %} <span class="concept-area__count">{{ group_count }}</span>{% endif %}</a>
        {% endfor %}
      </div>
    </section>

    <!-- 4. Calls to action -->
    <div class="section-head">
      <h3>Follow along</h3>
    </div>
    <div class="signup-box">
      <p>No schedule. I write when I've built something worth writing about.</p>
      <div class="form-row">
        <input type="email" placeholder="your@email.com" disabled>
        <button class="btn-disabled" disabled>Notify me</button>
      </div>
      <p class="coming-soon-note">Signup isn't wired up yet — coming soon.</p>
    </div>

    <div class="learn-box">
      <p class="learn-title">Want to learn this?</p>
      <p>Tell me what you're stuck on. It shapes what I build, and you get first access.</p>
      <button class="btn-disabled" disabled style="width:100%;">Tell me what you're stuck on — coming soon</button>
    </div>

  </main>
</div>
