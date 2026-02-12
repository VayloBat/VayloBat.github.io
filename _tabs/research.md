---
layout: page
title: Research
icon: fas fa-microscope
order: 1
---


<div class="row">
  {% assign my_post = site.posts | where: "title", "Embedded Insecurity: ZTE Modem Architecture" | first %}
  
  {% if my_post %}
  <div class="col-12 col-md-8 mb-4">
    <a href="{{ my_post.url | relative_url }}" style="text-decoration: none !important; display: block;">
      <div style="background: #0d0d0d; border: 2px solid #39FF14; border-radius: 12px; padding: 20px; transition: 0.3s; box-shadow: 0 0 10px rgba(57,255,20,0.1);" 
           onmouseover="this.style.boxShadow='0 0 20px rgba(57,255,20,0.4)'; this.style.transform='scale(1.01)';" 
           onmouseout="this.style.boxShadow='0 0 10px rgba(57,255,20,0.1)'; this.style.transform='scale(1)';" >
        
        <div style="display: flex; gap: 20px; align-items: center;">
          <img src="{{ '/assets/img/p.jpg' | relative_url }}" alt="ZTE Project" style="width: 100px; height: 100px; border-radius: 8px; object-fit: cover; border: 1px solid #333;">
          
          <div style="flex: 1;">
            <h3 style="color: #39FF14 !important; margin: 0; font-size: 1.3rem; font-weight: bold; font-family: 'Courier New', Courier, monospace;">
              {{ my_post.title }} <i class="fas fa-chevron-right" style="font-size: 0.9rem; margin-left: 5px;"></i>
            </h3>
            <p style="color: #ccc; font-size: 0.9rem; margin: 8px 0; line-height: 1.4;">
              Dissecting Forgotten Devices (Part-1)
            </p>
            
            <div style="margin-top: 10px;">
               <span style="background: rgba(57, 255, 20, 0.1); color: #39FF14; border: 1px solid #39FF14; padding: 2px 10px; border-radius: 5px; font-size: 0.75rem; font-weight: bold;">#ZTE_RESEARCH</span>
            </div>
          </div>
        </div>
      </div>
    </a>
  </div>
  {% else %}
    <p style="color: red;">خطأ: لم يتم العثور على المقال. تأكد من أن العنوان في ملف البحث يطابق العنوان داخل المقال نفسه.</p>
  {% endif %}
</div>
