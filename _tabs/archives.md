---
layout: page
title: Archives
icon: fas fa-archive
order: 10
---

<div style="padding: 20px 0; border-bottom: 1px solid #222; margin-bottom: 40px;">
  <h1 style="color: #ffffff; font-size: 2.2em; font-family: 'Inter', sans-serif; font-weight: 800; margin: 0;">
    Records <span style="color: #00FF00;">.</span>
  </h1>
  <p style="color: #666; font-size: 1em; margin-top: 10px; font-family: 'Inter', sans-serif;">
    A chronological index of research, logs, and technical notes.
  </p>
</div>

<div style="display: flex; flex-direction: column; gap: 5px;">
  {% for post in site.posts %}
    <a href="{{ post.url | relative_url }}" style="text-decoration: none !important; color: inherit; display: block;">
      <div class="archive-item" style="display: flex; justify-content: space-between; align-items: center; padding: 15px 10px; border-bottom: 1px solid #111; transition: 0.2s;">
        
        <div style="display: flex; align-items: center; gap: 15px;">
          <span style="color: #333; font-family: monospace; font-size: 0.85em; width: 40px;">0{{ forloop.index }}</span>
          <span class="archive-title" style="color: #ffffff; font-size: 1.05em; font-family: 'Inter', sans-serif; font-weight: 500;">
            {{ post.title }}
          </span>
        </div>

        <div style="display: flex; align-items: center; gap: 20px;">
          <span style="color: #444; font-size: 0.85em; font-family: monospace; letter-spacing: 1px;">
            {{ post.date | date: "%d %b %Y" | uppercase }}
          </span>
          <i class="fas fa-arrow-right" style="color: #1a1a1a; font-size: 0.8em;"></i>
        </div>

      </div>
    </a>
  {% endfor %}
</div>

<style>
  /* تأثيرات فخمة وبسيطة */
  .archive-item:hover {
    background: #080808;
    border-bottom-color: #222;
    padding-left: 20px !important;
  }
  
  .archive-item:hover .archive-title {
    color: #00FF00 !important;
  }

  .archive-item:hover i {
    color: #00FF00 !important;
    transform: translateX(5px);
    transition: 0.3s;
  }
</style>
