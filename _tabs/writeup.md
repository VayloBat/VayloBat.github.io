---
layout: page
title: Writeups
icon: fas fa-terminal
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

## 🌐 Online CTFs (Live Events)
<div class="card-container">
  <a href="https://github.com/VayloBat/write-up_ctf_online/tree/main/PascalCTF%202026%20Write-up%20(Pwn)%2Fmalta_nightlife" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">Malta Nightlife</h3>
      <p style="font-size: 0.85em; color: #bbb;">PascalCTF 2026 - Pwn Challenge Analysis.</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#PascalCTF</span></div>
  </a>

  <a href="https://github.com/VayloBat/write-up_ctf_online/tree/main/PascalCTF%202026%20Write-up%20(Pwn)%2FAlbo%20delle%20Eccellenze" target="_blank" class="pwn-card">
    <div>
      <h3 style="color: #00FF00; margin: 0;">Albo delle Eccellenze</h3>
      <p style="font-size: 0.85em; color: #bbb;">Advanced exploitation walkthrough for PascalCTF.</p>
    </div>
    <div style="margin-top: 15px;"><span class="tag">#PascalCTF</span></div>
  </a>
</div>
## 🚩 Platform: picoCTF
<div class="card-container">
  <a href="{{ '/posts/picoctf-input-injection-1/'| relative_url }}" target="_blank" class="pwn-card">
    <div><h3 style="color: #00FF00; margin: 0;">Input Injection 1</h3><p style="font-size: 0.85em; color: #bbb;">Command injection and filter bypassing.</p></div>
    <div style="margin-top: 15px;"><span class="tag">#picoCTF</span></div>
  </a>
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/picoctf%2Fheap_0" target="_blank" class="pwn-card">
    <div><h3 style="color: #00FF00; margin: 0;">Heap 0</h3><p style="font-size: 0.85em; color: #bbb;">Introduction to heap memory corruption.</p></div>
    <div style="margin-top: 15px;"><span class="tag">#Heap</span></div>
  </a>
  <a href="{{ '/posts/picoctf-buffer-overflow-0/'| relative_url }}" target="_blank" class="pwn-card">
    <div><h3 style="color: #00FF00; margin: 0;">Buffer Overflow 0</h3><p style="font-size: 0.85em; color: #bbb;">Basic stack-based overflow analysis.</p></div>
    <div style="margin-top: 15px;"><span class="tag">#Stack</span></div>
  </a>
</div>
## 🧪 Platform: TryHackMe (Pwn101)
<div class="card-container">
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/tryhackme%2Fpwn101" target="_blank" class="pwn-card"><div><h3 style="color: #00FF00; margin: 0;">Pwn 101</h3></div></a>
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/tryhackme%2Fpwn102" target="_blank" class="pwn-card"><div><h3 style="color: #00FF00; margin: 0;">Pwn 102</h3></div></a>
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/tryhackme%2Fpwn103" target="_blank" class="pwn-card"><div><h3 style="color: #00FF00; margin: 0;">Pwn 103</h3></div></a>
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/tryhackme%2Fpwn104" target="_blank" class="pwn-card"><div><h3 style="color: #00FF00; margin: 0;">Pwn 104</h3></div></a>
  <a href="https://github.com/VayloBat/My_pwn_journey/tree/main/tryhackme%2Fpwn105" target="_blank" class="pwn-card"><div><h3 style="color: #00FF00; margin: 0;">Pwn 105</h3></div></a>
</div>
