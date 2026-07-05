---
layout: page
title: Research
icon: fas fa-microscope
order: 1
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
  .category-title {
    color: #00FF00;
    border-bottom: 1px solid #333;
    padding-bottom: 10px;
    margin-bottom: 20px;
    font-family: 'Fira Code', monospace;
    display: flex;
    align-items: center;
  }
  .tag {
    background: #333;
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 0.7em;
    color: #00FF00;
  }
</style>


<div class="research-container">
  
  <a href="{{ '/posts/embedded-insecurity-zte-modem/' | relative_url }}" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">🛡️ ZTE Modem Research</h3>
      <p style="font-size: 0.85em; color: #bbb;"> Dissecting Forgotten Devices: A Deep Dive Into ZTE Modem Architecture Analysis (Part 1).</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#Hardware</span></div>
  </a>

  
  <a href="{{ '/posts/hitv-mod-analysis-part1/' | relative_url }}" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">📱 HiTV Modded APK Architecture Research</h3>
      <p style="font-size: 0.85em; color: #bbb;"> Security Research: Deep Dive into HiTV Modded APK Architecture (Part1).</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#AppReverseEngineering</span></div>
  </a>

  
  <a href="{{ '/posts/hitv-mod-analysis-part2/' | relative_url }}" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">📱 HiTV Modded APK Architecture Research(part2)</h3>
      <p style="font-size: 0.85em; color: #bbb;"> Security Research: Deep Dive into HiTV Modded APK Architecture (Part2).</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#AppReverseEngineering</span></div>
  </a>
  
  
  <a href="{{ '/posts/lorawan-parser-vulnerability-analysis/' | relative_url }}" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">lorawan-parser-vulnerability-analysis</h3>
      <p style="font-size: 0.85em; color: #bbb;">  Security Research: lorawan-parser-vulnerability-analysis.</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#analysisICS</span></div>
  </a>

</div>
