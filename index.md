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
## Hello, I'm Steff.

I'm a data engineer. 
By day, I design and build governed data products on Databricks, from the first intake to pipelines, quality checks, access control and lineage in production.

By night, I apply data and AI engineering to socially relevant problems and blog about my work.

This blog is for you if you:
- want to learn how to build data products.
- want to know how to apply generative AI tools to accelerate your data engineering workflow.
- want to apply these powers to creating products that answer meaningful questions.


Steff Horemans is a data engineer with a PhD in the life sciences and six years of building data products, from Tableau dashboards for a pharmaceutical client to data platforms on Databricks. Before that, he studied how a cell's metabolism regulates DNA replication. Now, he wants to bring all this together to build data solutions that directly improve the lives of others.  Based in Antwerp, Belgium.
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
      {% assign linkedin_link = site.author.links | where: "label", "Linkedin" | first %}
      {% if linkedin_link %}<a href="{{ linkedin_link.url }}"><span class="home2-icon">🔗</span> LinkedIn</a>{% endif %}
    </div>
    <a class="home2-story-btn" href="{{ '/assets/cv/cv_steff_horemans_EN_2.pdf' | relative_url }}">Download my CV →</a>
  </aside>

  <main class="home2-main">

    <!-- 1. Intro (text: the capture block at the top) -->
    <section class="home-intro">{{ intro | markdownify }}</section>

    <!-- 2. The current product -->
    <section class="section" id="products">
      <div class="section-head">
        <h3>Products</h3>
        <a class="see-all" href="{{ '/products/' | relative_url }}">See all products →</a>
      </div>

      {% assign latest_projects = site.products | sort: "date" | reverse %}
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
            <span class="product-card__cta">Open the product →</span>
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
        {% if group_count > 0 %}<a class="concept-area" href="{{ group_page.url | relative_url }}">{{ group.title }} <span class="concept-area__count">{{ group_count }}</span></a>{% endif %}
        {% endfor %}
      </div>
    </section>

    <!-- 4. Calls to action -->
    <div class="section-head">
      <h3>Follow along</h3>
    </div>
    <div class="signup-box">
      <p>No schedule. I write when I've built something worth writing about.</p>
      <a class="cta-btn cta-btn--primary" href="{{ '/feed.xml' | relative_url }}">Subscribe via RSS</a>
      <p class="cta-note">Add this link to a feed reader, such as Feedly, Inoreader or NetNewsWire, and every new post shows up there by itself.</p>
    </div>

    <div class="learn-box">
      <p class="learn-title">Want to learn this?</p>
      <p>Tell me what you're stuck on. It shapes what I build, and you get first access.</p>
      <a class="cta-btn" href="mailto:{{ site.email }}?subject=What%20I%27m%20stuck%20on">Tell me what you're stuck on</a>
    </div>

  </main>
</div>
