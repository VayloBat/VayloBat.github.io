---
layout: page
title: Research
icon: fas fa-microscope
order: 2
---

<style>
  /* الحاوية الرئيسية للبحوث */
  .research-grid {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-bottom: 40px;
  }

  /* سطر يحتوي على عنصرين بجانب بعضهما */
  .research-row-2 {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 20px;
  }

  /* الستايل الخاص بالبطاقة */
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
    min-height: 140px;
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
    width: fit-content;
  }
</style>

<div class="research-grid">

  <a href="{{ '/posts/embedded-insecurity-zte-modem/' | relative_url }}" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">🛡️ ZTE Modem Research</h3>
      <p style="font-size: 0.85em; color: #bbb; margin-top: 8px;">Dissecting Forgotten Devices: A Deep Dive Into ZTE Modem Architecture Analysis (Part 1).</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#Hardware</span></div>
  </a>

  <div class="research-row-2">
    <a href="{{ '/posts/hitv-mod-analysis-part1/' | relative_url }}" class="pwn-card">
      <div>
        <h3 style="color: #00FF00; margin: 0;">📱 HiTV Modded APK Architecture Research (Part 1)</h3>
        <p style="font-size: 0.85em; color: #bbb; margin-top: 8px;">Security Research: Deep Dive into HiTV Modded APK Architecture(Part 1).</p>
      </div>
      <div style="margin-top: 15px;"><span class="tag">#AppReverseEngineering</span></div>
    </a>

    <a href="{{ '/posts/hitv-mod-analysis-part2/' | relative_url }}" class="pwn-card">
      <div>
        <h3 style="color: #00FF00; margin: 0;">📱 HiTV Modded APK Architecture Research (Part 2)</h3>
        <p style="font-size: 0.85em; color: #bbb; margin-top: 8px;">Security Research: Deep Dive into HiTV Modded APK Architecture (Part 2).</p>
      </div>
      <div style="margin-top: 15px;"><span class="tag">#AppReverseEngineering</span></div>
    </a>
  </div>

  <a href="{{ '/posts/lorawan-parser-vulnerability-analysis/' | relative_url }}" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">📡 LoRaWAN Parser Vulnerability Analysis</h3>
      <p style="font-size: 0.85em; color: #bbb; margin-top: 8px;">Security Research: Critical cryptographic vulnerability analysis for the lorawan-parser project.</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#analysisICS</span></div>
  </a>

</div>
