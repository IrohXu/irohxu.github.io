---
layout: page
title: Workstreams
permalink: /workstreams/
description: An overview of ongoing projects in the aiaudit.org cooperative.
nav: false
nav-order: c
---

<div class="projects grid">

  {% assign sorted_projects = site.projects | sort: "importance" %}
  {% for project in sorted_projects %}
  <div class="grid-item">
    {% if project.redirect %}
    <a href="{{ project.redirect }}" target="_blank">
    {% else %}
    <a href="{{ project.url | relative_url }}">
    {% endif %}
      <div class="card hoverable {{ project.worktype}}">
        {% if project.img %}
        <img src="{{ project.img | relative_url }}" alt="project thumbnail">
        {% endif %}
        <div class="card-body">
          <h2 class="card-title text-white">{{ project.title }}</h2>
          <!-- <p class="card-title text-white">Life cycle steps: {{ project.lifecyclesteps }}</p> -->
          <p class="card-text text-white">{{ project.description }}</p>
          <p class="card-title text-white">Meetings: {{ project.coordinates }}</p>
          <!-- <a href="{{ project.url | relative_url }}" class="btn btn-primary">Details</a> -->
          <div class="row ml-1 mr-1 p-0">
            {% if project.github %}
            <div class="github-icon">
              <div class="icon" data-toggle="tooltip" title="Code Repository">
                <a href="{{ project.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
              </div>
              {% if project.github_stars %}
              <span class="stars" data-toggle="tooltip" title="GitHub Stars">
                <i class="fas fa-star"></i>
                <span id="{{ project.github_stars }}-stars"></span>
              </span>
              {% endif %}
            </div>
            {% endif %}
          </div>
        </div>
      </div>
    </a>
  </div>
{% endfor %}

</div>
