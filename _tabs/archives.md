---
layout: page
title: Archives
icon: fas fa-archive
order: 10
---

<style>
  /* تحسين الخط العام */
  .post-title-link {
    font-family: 'Fira Code', 'Courier New', monospace;
    font-size: 1.1em;
    color: #00FF00 !important;
    text-decoration: none !important;
    transition: 0.3s;
  }

  /* تصميم المربع الخاص بالأيقونة */
  .square-icon {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 35px;
    height: 35px;
    background-color: #1a1a1a;
    border: 1px solid #00FF00;
    border-radius: 4px; /* زوايا مربعة حادة قليلاً */
    color: #00FF00;
    margin-right: 15px;
    font-size: 0.9em;
    box-shadow: 0 0 5px rgba(0, 255, 0, 0.2);
  }

  /* حاوية الخبر/المقال */
  .post-item {
    display: flex;
    align-items: center;
    padding: 15px;
    margin-bottom: 12px;
    background: rgba(26, 26, 26, 0.5);
    border: 1px solid #333;
    border-radius: 8px;
    transition: 0.3s;
  }

  .post-item:hover {
    border-color: #00FF00;
    background: rgba(0, 255, 0, 0.05);
    transform: translateX(8px);
  }

  .post-date {
    margin-left: auto;
    font-family: 'Fira Code', monospace;
    color: #555;
    font-size: 0.85em;
  }
</style>

<div style="margin-top: 20px;">
  <p style="color: #888; font-family: 'Fira Code', monospace;">[+] Listing all compromised nodes (posts)...</p>
</div>

<div style="margin-top: 30px;">
  {% if site.posts.size > 0 %}
    {% for post in site.posts %}
      <div class="post-item">
        <div class="square-icon">
          <i class="fas fa-terminal"></i>
        </div>
        <a href="{{ post.url | relative_url }}" class="post-title-link">
          {{ post.title }}
        </a>
        <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      </div>
    {% endfor %}
  {% else %}
    <div style="text-align: center; padding: 40px; border: 1px dashed #444; border-radius: 10px;">
      <p style="color: #ff4444; font-family: 'Fira Code', monospace;">[!] System empty. No logs detected.</p>
    </div>
  {% endif %}
</div>
