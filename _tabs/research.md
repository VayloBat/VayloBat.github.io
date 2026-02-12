---
layout: page
title: Research
icon: fas fa-microscope
order: 1
---


<div class="row">
  {% assign my_post = site.posts | where: "title", "ZTE-Modem-Research" | first %}
  
  {% if my_post %}
  <div class="col-12 col-md-8 mb-4">
    <a href="{{ my_post.url | relative_url }}" style="text-decoration: none !important; display: block;">
      <div style="background: #0d0d0d; border: 2px solid #39FF14; border-radius: 12px; padding: 20px; transition: 0.3s; box-shadow: 0 0 15px rgba(57,255,20,0.2);" 
           onmouseover="this.style.transform='scale(1.02)'; this.style.boxShadow='0 0 25px rgba(57,255,20,0.5)';" 
           onmouseout="this.style.transform='scale(1)'; this.style.boxShadow='0 0 15px rgba(57,255,20,0.2)';" >
        
        <div style="display: flex; gap: 20px; align-items: center;">
          <img src="{{ '/assets/img/p.jpg' | relative_url }}" alt="Research" style="width: 100px; height: 100px; border-radius: 10px; object-fit: cover; border: 1px solid #333;">
          
          <div style="flex: 1;">
            <h3 style="color: #39FF14 !important; margin: 0; font-family: 'Courier New', Courier, monospace; font-weight: bold;">
              ZTE MODEM RESEARCH
            </h3>
            <p style="color: #ccc; font-size: 0.9rem; margin: 8px 0;">
              Exploitation & Hardware Analysis (Part 1)
            </p>
            
            <div style="margin-top: 10px;">
               <span style="background: #39FF14; color: #000; padding: 2px 10px; border-radius: 4px; font-size: 0.7rem; font-weight: bold;">READ WRITEUP</span>
            </div>
          </div>
        </div>
      </div>
    </a>
  </div>
  {% else %}
    <div style="color: #ff4444; border: 1px dashed #ff4444; padding: 15px; text-align: center; border-radius: 10px;">
       ⚠️ تأكد أن عنوان المقال داخل الملف هو: <br>
       <strong>title: ZTE-Modem-Research</strong>
    </div>
  {% endif %}
</div>
