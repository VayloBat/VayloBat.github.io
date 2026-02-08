---
layout: page
title: Archives
icon: fas fa-archive
order: 2
---

<div style="margin-top: 20px;">
  <p style="color: #888; font-style: italic;">A chronological collection of my research, lab notes, and exploit development journey.</p>
</div>

<div style="margin-top: 30px;">
  {% for post in site.posts %}
    {% capture current_year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% if current_year != last_year %}
      <h2 style="color: #00FF00; font-family: 'Fira Code', monospace; border-bottom: 1px solid #333; padding-bottom: 5px; margin-top: 30px;">{{ current_year }}</h2>
      {% capture last_year %}{{ current_year }}{% endcapture %}
    {% endif %}

    <div style="display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #1a1a1a;">
      <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #eee; font-size: 1.1em; transition: 0.3s;">
        <span style="color: #00FF00; margin-right: 10px;">></span> {{ post.title }}
      </a>
      <span style="color: #555; font-family: monospace; font-size: 0.9em;">{{ post.date | date: "%b %d" }}</span>
    </div>
  {% endfor %}
</div>

<style>
  a:hover {
    color: #00FF00 !important;
    padding-left: 10px;
  }
</style>
