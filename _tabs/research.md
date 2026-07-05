---
layout: page
title: Research
---

<style>
  .card-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
    margin-bottom: 40px;
  }
  .pwn-card {
    border: 1px solid #333;
    padding: 20px;
    border-radius: 12px;
    background: #1a1a1a;
    transition: all 0.3s ease;
    border-left: 4px solid #00FF00;
    text-decoration: none !important;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 100%;
  }
  .pwn-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0, 255, 0, 0.1);
    border-color: #00FF00;
  }
  .tag {
    background: #333;
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.7em;
    color: #00FF00;
  }
</style>

<div class="research-container card-container">
  {% for post in site.posts limit:5 %}
    <a href="{{ post.url | relative_url }}" target="_blank" class="pwn-card">
      <div>
        <h3 style="color: #00FF00; margin: 0;">{{ post.title }}</h3>
        <p style="font-size: 0.85em; color: #bbb;">{{ post.excerpt | strip_html | truncate: 140 }}</p>
      </div>
      <div style="margin-top: 15px;">
        <span class="tag">{% if post.tags %}{{ post.tags | first }}{% else %}#Research{% endif %}</span>
      </div>
    </a>
  {% endfor %}
</div>
