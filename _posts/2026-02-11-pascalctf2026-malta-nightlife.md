---
layout: post
title: " Malta Nightlife PascalCTF 2026"
date: 2026-02-11 12:00:00 +0100
---




This was a classic bank-breaking challenge. You start with 100€, but the flag "drink" costs a billion. Obviously, we aren't going to earn that much legitimately.

### The Logic
Checking the binary in **Ghidra**, I saw that:
* The flag is loaded into the description of the 10th drink (index 9) at startup.
* To see that description, you actually have to buy the drink.
* Price: `1,000,000,000€`. My wallet: `100€`.

### The Bug: Integer Overflow
The vulnerability is in the cost calculation. The program uses signed 32-bit integers for the check:
`if ((int)balance < (int)(quantity * price))`

The max value for a signed 32-bit int is about **2.14 billion**. If you go over that, the number "wraps around" and becomes negative. 

### Exploitation
I needed the total cost to exceed 2.14 billion to trigger the flip.
1. I ordered **3 units** of the Flag drink. 
2. `3 * 1,000,000,000 = 3,000,000,000`.
3. In signed integer terms, 3 billion overflows and becomes **-1.29 billion**.
4. Since `100 < -1,290,000,000` is false, the program thinks I'm rich enough and lets the transaction slide.

Actually, subtracting a negative number from my balance gave me billions in credits. Absolute win.

### Getting the Flag
With my new infinite balance, I just bought 1 unit of drink #10. The program printed the "secret recipe," which was the flag.

**The Strategy:**
* **Step 1:** Order 3 of drink #10 (Overflow the cost to get infinite money).
* **Step 2:** Order 1 of drink #10 (Buy it for real to read the flag).


## exploit:
so the exploit lives in its own file. Check it out here: 
<p style="text-align: right; margin-top: -15px;">
  <a href="{{ '/assets/img/exploitM.py' | relative_url }}" download style="color: #00FF00; font-size: 0.8em; text-decoration: none; font-family: monospace;">
    [ Download Raw File ]
  </a>
</p> 


### Result:

<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/img/screenshot.jpg' | relative_url }}" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; width: 100%; max-width: 800px; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>




**Flag caught.** 🚩

<div style="margin-top: 50px; padding: 20px; border: 1px solid #1a1a1a; border-radius: 10px; background: #0a0a0a; display: flex; align-items: center; justify-content: space-between;">
  <div>
    <h4 style="margin: 0; color: #ffffff; font-size: 1.1em;">Full Writeup & Files</h4>
    <p style="margin: 5px 0 0 0; color: #666; font-size: 0.9em;">View the exploit code and notes on GitHub.</p>
  </div>
  
  <a href="https://github.com/VayloBat/write-up_ctf_online/tree/main/PascalCTF%202026%20Write-up%20(Pwn)/malta_nightlife"  target="_blank" style="text-decoration: none !important;">
    <div style="display: flex; align-items: center; gap: 10px; background: #ffffff; color: #000000; padding: 10px 20px; border-radius: 6px; font-weight: 600; font-size: 0.9em; transition: 0.3s;" onmouseover="this.style.background='#00FF00'" onmouseout="this.style.background='#ffffff'">
      <i class="fab fa-github" style="font-size: 1.2em;"></i>
      View on GitHub
    </div>
  </a>
</div>
