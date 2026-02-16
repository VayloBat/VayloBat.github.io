---
layout: post
title: "2026 02 16 pascalctf albo delle eccellenze"
date: 2026-02-16 12:00:00 +0100
---


# Albo delle Eccellenze (Writeup)

This challenge was tagged as Reverse, but honestly, it was a pretty straightforward Pwn.

### What's going on?
I tossed the binary into Ghidra and checked out the main logic in `FUN_004024e4`. It asks for some personal info to generate a "Codice Fiscale." The program then checks your input against a hardcoded string: `Al1D612LPSCBLS37`.

The funny part? The logic is flipped. If the check fails (meaning `cVar1` is NOT 1), it triggers the flag function. So, we basically just need to break the comparison.

### The Vulnerability
The input function `FUN_004159c0` is broken. It reads way more than it should (up to 100 bytes) into buffers that are much smaller. Since the control variable `cVar1` is sitting right there on the stack after our input, it's a classic Buffer Overflow.

### The Lazy Solve
Why bother reversing the algorithm when you can just smash the stack? I sent 600 "A"s to overflow everything, overwrite the control variable, and force the program to jump to the flag.

**The Exploit:**
```bash
python3 -c 'print("A"*600)' | nc albo.ctf.pascalctf.it 7004

```
### Result: 

<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/img/screenshotreverse.png' | relative_url }}" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; width: 100%; max-width: 800px; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>


---

<div style="margin-top: 50px; padding: 20px; border: 1px solid #1a1a1a; border-radius: 10px; background: #0a0a0a; display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h4 style="margin: 0; color: #ffffff; font-size: 1.1em;">Full Writeup & Files</h4>
    <p style="margin: 5px 0 0 0; color: #666; font-size: 0.9em;">View the exploit code and notes on GitHub.</p>
  </div>
  
  <a href="https://github.com/VayloBat/write-up_ctf_online/tree/main/PascalCTF%202026%20Write-up%20(Pwn)/Albo%20delle%20Eccellenze" target="_blank" style="text-decoration: none !important;">
    <div style="display: flex; align-items: center; gap: 10px; background: #ffffff; color: #000000; padding: 10px 20px; border-radius: 6px; font-weight: 600; font-size: 0.9em; transition: 0.3s;" onmouseover="this.style.background='#00FF00'" onmouseout="this.style.background='#ffffff'">
      <i class="fab fa-github" style="font-size: 1.2em;"></i>
      View on GitHub
    </div>
  </a>
</div>



