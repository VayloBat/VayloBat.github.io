---
layout: post
title: "picoctf-heap-0"
date: 2026-02-07 12:00:00 +0100
---

### The Logic
This challenge demonstrates that overflows aren't exclusive to the stack; the **heap** is just as vulnerable. The program places two variables side-by-side in dynamic memory: one for user input and one as a "safe" check variable. The goal is to overflow the first until it spills over and corrupts the second.

### The Attack
I jumped straight to the "Write to buffer" (Option 2) and blasted it with a long string of "A"s. This massive payload exceeded the allocated space, reached the adjacent memory chunk, and overwrote the target variable. Once the memory was corrupted, I just triggered the win condition (Option 4).

**Steps taken:**
1. Selected `2` to write data.
2. Sent a long string of "A"s to trigger the overflow.
3. Selected `4` to print the flag.

### Proof of Concept (PoC)
<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/img/screenshoteheap.jpg' | relative_url }}" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; width: 100%; max-width: 800px; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>

### Result
The heap corruption was detected, and the flag was successfully captured.
---

<div style="margin-top: 50px; padding: 20px; border: 1px solid #1a1a1a; border-radius: 10px; background: #0a0a0a; display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h4 style="margin: 0; color: #ffffff; font-size: 1.1em;">Full Writeup & Files</h4>
    <p style="margin: 5px 0 0 0; color: #666; font-size: 0.9em;">View the exploit code and notes on GitHub.</p>
  </div>
  
  <a href="https://github.com/VayloBat/My_pwn_journey/blob/main/picoctf/heap_0/" target="_blank" style="text-decoration: none !important;">
    <div style="display: flex; align-items: center; gap: 10px; background: #ffffff; color: #000000; padding: 10px 20px; border-radius: 6px; font-weight: 600; font-size: 0.9em; transition: 0.3s;" onmouseover="this.style.background='#00FF00'" onmouseout="this.style.background='#ffffff'">
      <i class="fab fa-github" style="font-size: 1.2em;"></i>
      View on GitHub
    </div>
  </a>
</div>
