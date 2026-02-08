---
layout: post
title: "picoCTF - buffer overflow 0"
date: 2026-02-08 12:00:00 +0100
categories: [Writeups, picoCTF]
tags: [pwn, buffer-overflow, ctf]
---

### Summary
This challenge is a classic introduction to the **Buffer Overflow** vulnerability. The program allocates a small memory buffer for user input. By providing a string much longer than expected, the program "chokes" and triggers a **Segmentation Fault**. Since the system is configured to print the flag upon a crash, this overflow leads directly to the win.

### Exploitation
Instead of manual entry, I used a Python one-liner to pipe 600 characters into the connection. This was more than enough to overflow the buffer and force the crash.

**Command:**
```bash
python3 -c 'print("A"*600)' | nc saturn.picoctf.net 64825
