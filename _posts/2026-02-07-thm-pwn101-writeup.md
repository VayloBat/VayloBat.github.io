---
title: "THM - Pwn101 Writeup"
date: 2026-02-07 17:00:00 +0000
categories: [Writeups, Pwn]
tags: [buffer-overflow, thm, linux-exploitation]
image:
  path: /assets/img/posts/pwn101/screenshot.png
  alt: Pwn101 Shell Pop
---

## 0x01: Initial Recon
Started with a quick `checksec` to see what I'm dealing with. The binary is **x86-64** and most protections are stripped or disabled.
- **NX is ON:** So no shellcode on the stack.
- **Canary is OFF:** This is the green light for a Buffer Overflow.
- **No PIE:** Memory addresses are static.

## 0x02: Looking under the hood (Static)
Threw the binary into **Ghidra**. The `main` function uses `gets()`, a classic "security sin" that doesn't care about buffer limits—perfect for smashing the stack.

## 0x03: Finding the Crash (Dynamic)
Using **GDB-GEF**, I found the offset using a cyclic pattern:
- **Offset identified: 64**

## 0x04: The Attack Plan
Since I'm on **Arch Linux**, I'll use `pwntools` to:
1. Send 64 bytes of junk.
2. Redirect execution to pop a shell `/bin/sh`.

## 0x05: Exploit Code
You can download the full exploit here: [exploit.py](/assets/img/posts/pwn101/exploit.py)
