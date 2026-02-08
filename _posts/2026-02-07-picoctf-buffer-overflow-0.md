---
layout: post
title: "picoctf-buffer-overflow-0"
date: 2026-02-07 10:00:00 +0100
---

# picoCTF - buffer overflow 0

### Summary
This challenge is a classic introduction to the **Buffer Overflow** vulnerability. The program allocates a small memory buffer for user input. By providing a string much longer than expected, the program "chokes" and triggers a **Segmentation Fault**. Since the system is configured to print the flag upon a crash, this overflow leads directly to the win.

### Exploitation
Instead of manual entry, I used a Python one-liner to pipe 600 characters into the connection. This was more than enough to overflow the buffer and force the crash.

**Command:**
python3 -c 'print("A"*600)' | nc saturn.picoctf.net 64825

### Proof of Concept (PoC)

<div style="text-align: center; margin: 20px 0;">
  <img src="/assets/img/screenshote.jpg" alt="Exploit Screenshot" style="border-radius: 8px; border: 1px solid #333; max-width: 100%; box-shadow: 0 4px 20px rgba(0,0,0,0.5);">
</div>

### Result
The program crashed as expected and printed the flag.

