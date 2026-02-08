---
layout: post
title: "picoctf-buffer-overflow-0"
date: 2026-02-07 10:00:00 +0100
---



### Summary:
This challenge is a classic introduction to the **Buffer Overflow** vulnerability. The program allocates a small memory buffer for user input. By providing a string much longer than expected, the program "chokes" and triggers a **Segmentation Fault**. Since the system is configured to print the flag upon a crash, this overflow leads directly to the win.

### Exploitation:
Instead of manual entry, I used a Python one-liner to pipe 600 characters into the connection. This was more than enough to overflow the buffer and force the crash.

**Command:**
python3 -c 'print("A"*600)' | nc saturn.picoctf.net 64825

### Proof of Concept (PoC):

<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/img/screenshote.jpg' | relative_url }}" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; width: 100%; max-width: 800px; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>

### Result:
The program crashed as expected and printed the flag.
---

<div style="margin-top: 50px; padding: 20px; border: 1px solid #1a1a1a; border-radius: 10px; background: #0a0a0a; display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h4 style="margin: 0; color: #ffffff; font-size: 1.1em;">Full Writeup & Files</h4>
    <p style="margin: 5px 0 0 0; color: #666; font-size: 0.9em;">View the exploit code and notes on GitHub.</p>
  </div>
  
  <a href="https://github.com/VayloBat/My_pwn_journey/blob/main/picoctf/buffer_overflow_0/" target="_blank" style="text-decoration: none !important;">
    <div style="display: flex; align-items: center; gap: 10px; background: #ffffff; color: #000000; padding: 10px 20px; border-radius: 6px; font-weight: 600; font-size: 0.9em; transition: 0.3s;" onmouseover="this.style.background='#00FF00'" onmouseout="this.style.background='#ffffff'">
      <i class="fab fa-github" style="font-size: 1.2em;"></i>
      View on GitHub
    </div>
  </a>
</div>
