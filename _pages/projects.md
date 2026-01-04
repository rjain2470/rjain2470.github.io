---
layout: page
title: projects
permalink: /projects/
description:
nav: true
nav_order: 4
display_categories:
horizontal: false
---

<!-- pages/projects.md -->

### autoformalization

These are my projects related to _autoformalization_ and _autoreasoning_ in [Lean](https://en.wikipedia.org/wiki/Lean_(proof_assistant)). Autoformalization refers to the process of translating mathematical statements and proofs into a formal language such as Lean, whereas autoreasoning denotes the ability of a system to produce original formal proofs with minimal human intervention. 

Among other motivations, the most ambitious possible outcome of this work would be a system capable of producing novel mathematical results, which, having been written in a formal language, would be _a priori_ correct.

<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>

### number theory

TBA
