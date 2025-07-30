---
layout: page
permalink: /seminars/
title: seminars
description: talking about math together!
nav: true
nav_order: 3
---

<!-- pages/seminars.md -->
<div class="projects">
<!-- Display projects without categories -->
  {%- assign sorted_seminars = site.seminars | sort: "importance" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_seminars -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_seminars -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
</div>


<!-- For now, this page is assumed to be a static description of your courses. You can convert it to a collection similar to `_projects/` so that you can have a dedicated page for each course. -->

<!-- Organize your courses by years, topics, or universities, however you like! -->

