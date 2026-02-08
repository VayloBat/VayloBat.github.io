---
layout: page
title: Archives
icon: fas fa-archive
order: 10
---

<div style="margin-top: 20px;">
  <p style="color: #888; font-style: italic;">A chronological collection of my research and lab notes.</p>
</div>

<div style="margin-top: 30px;">
  {% if site.posts.size > 0 %}
    {% for post in site.posts %}
      <div style="display: flex; justify-content: space-between; align-items: center; padding: 12px; border: 1px solid #333; border-radius: 8px; margin-bottom: 10px; background: #1a1a1a;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #00FF00; font-weight: bold; font-family: 'Fira Code', monospace;">
          > {{ post.title }}
        </a>
        <span style="color: #555; font-family: monospace; font-size: 0.9em;">{{ post.date | date: "%b %d, %Y" }}</span>
      </div>
    {% endfor %}
  {% else %}
    <div style="text-align: center; padding: 40px; border: 1px dashed #444; border-radius: 10px;">
      <p style="color: #ff4444; font-family: monospace;">[!] No logs found in /_posts/</p>
      <p style="color: #666; font-size: 0.9em;">System ready for new entries...</p>
    </div>
  {% endif %}
</div>

<style>
  a:hover {
    color: #fff !important;
    text-shadow: 0 0 5px #00FF00;
  }
</style>
