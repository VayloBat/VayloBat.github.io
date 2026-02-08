---
layout: post
title: "picoctf-input-injection-1"
date: 2026-02-07 11:00:00 +0100
---

### Summary :
I started by looking over the code and found that it has a **stack-based buffer overflow** vulnerability because `strcpy` is being handled poorly.

The program sets up two small buffers on the stack: `buffer` (for the name) and `c` (for the command). Both are limited to only **10 bytes**. The problem is that the code uses `strcpy` to move data into them. Since `strcpy` doesn't check if the input actually fits, we can easily overflow `buffer` and start writing into the memory space of `c`.

### The Attack :
My goal was simple: **Overwrite the `uname` command in memory.**
By sending more than 10 bytes of data, I can "spill over" from the first buffer into the second one. If I time it right, I can replace the default `uname` command with `cat flag.txt`.

**Memory Visual:**
`[ 10 bytes of Junk ] + [ My Command (cat flag.txt) ]`

### Exploit :
so the exploit lives in its own file. Check it out here: 
<p style="text-align: right; margin-top: -15px;">
  <a href="{{ '/assets/img/exploit.py' | relative_url }}" download style="color: #00FF00; font-size: 0.8em; text-decoration: none; font-family: monospace;">
    [ Download Raw File ]
  </a>
</p> 
### Proof of Concept (PoC):

<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/img/screenshoteinput.jpg' | relative_url }}" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; width: 100%; max-width: 800px; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>

### Result:
The program crashed as expected and printed the flag.
---

<div style="margin-top: 50px; padding: 20px; border: 1px solid #1a1a1a; border-radius: 10px; background: #0a0a0a; display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h4 style="margin: 0; color: #ffffff; font-size: 1.1em;">Full Writeup & Files</h4>
    <p style="margin: 5px 0 0 0; color: #666; font-size: 0.9em;">View the exploit code and notes on GitHub.</p>
  </div>
  
  <a href="https://github.com/VayloBat/My_pwn_journey/blob/main/picoctf/input_injection_1/" target="_blank" style="text-decoration: none !important;">
    <div style="display: flex; align-items: center; gap: 10px; background: #ffffff; color: #000000; padding: 10px 20px; border-radius: 6px; font-weight: 600; font-size: 0.9em; transition: 0.3s;" onmouseover="this.style.background='#00FF00'" onmouseout="this.style.background='#ffffff'">
      <i class="fab fa-github" style="font-size: 1.2em;"></i>
      View on GitHub
    </div>
  </a>
</div>

