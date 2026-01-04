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

These are my projects related to autoformalization and autoreasoning in [Lean](https://en.wikipedia.org/wiki/Lean_(proof_assistant)). Autoformalization can be refers to the process of translating mathematical statements and proofs into a formal language such as Lean, whereas autoreasoning denotes the ability of a system produces original formal mathematics with minimal human intervention. 

Following the advent of LLMs in recent years, these areas has been the subject of intense interest in the mathematical community, see for instance these [papers](https://arxiv.org/search/?query=autoformalization&searchtype=all&source=header). Among various motivations, the most ambitious possible outcome one could envision is the creation of an LLM-based system which can produce novel mathematical proofs. Assuming that the input statements are accurate, these proofs would be _a priori_ logically sound, having been written in a formal language. If LLMs continue to progress at their current rate, this would, without a doubt, change mathematics and the way it is practiced forever.

As for my work, I have largely been interested in the augmentation of so-called foundation models such as ChatGPT and Claude for the tasks of autoformalization and autoreasoning.

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
