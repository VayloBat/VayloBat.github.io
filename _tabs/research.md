---
layout: page
title: Research
icon: fas fa-microscope
order: 1
---


<style>
  .research-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
    margin-top: 20px;
  }
  .res-card {
    border: 1px solid #333;
    padding: 20px;
    border-radius: 12px;
    background: #1a1a1a;
    transition: all 0.3s ease;
    border-left: 4px solid #00FF00;
    text-decoration: none !important;
    display: flex;
    flex-direction: column;
    height: 100%;
  }
  .res-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0, 255, 0, 0.1);
    border-color: #00FF00;
  }
  .res-title {
    color: #00FF00;
    font-family: 'Fira Code', monospace;
    margin: 0 0 10px 0;
    font-size: 1.2rem;
  }
  .res-desc {
    color: #ccc;
    font-size: 0.9rem;
    line-height: 1.5;
  }
</style>

<div class="research-container">
  
  <a href="{{ '/posts/embedded-insecurity-zte-modem/' | relative_url }}" class="res-card">
    <h3 class="res-title">🛡️ ZTE Modem Research</h3>
    <p class="res-desc">
      Dissecting Forgotten Devices: A Deep Dive Into ZTE Modem Architecture Analysis (Part 1).
    </p>
    <div style="margin-top: auto; padding-top: 15px;">
      <span style="background: #333; color: #00FF00; padding: 2px 8px; border-radius: 4px; font-size: 0.7rem;">#Hardware</span>
      <span style="background: #333; color: #00FF00; padding: 2px 8px; border-radius: 4px; font-size: 0.7rem;">#Exploitation</span>
    </div>
  </a>

</div>
