---
title: "Security Research: Deep Dive into HiTV Modded APK Architecture"
layout: post
date: 2026-03-17 20:00:00 +0100
categories: [Research, Malware-Analysis]
tags: [Android, ReverseEngineering, Security, resercher, apk]
---

## ⚖️ Disclaimer
This research is provided for educational, security, and awareness purposes only. All analysis procedures were conducted within a strictly isolated virtual environment (Sandbox). The researcher is not responsible for any illicit use of the information contained herein. The primary goal is to alert users to the inherent risks of downloading modified applications (Modded APKs) from untrusted sources.

---

## 1. Methodology & Intent
I observed the widespread distribution of a modified HiTV application claiming to offer "Premium" features for free. I decided to subject this version to a full Reverse Engineering process to verify the actual price the user pays for this "free" access. I employed **Static Analysis** methodology to deconstruct the application’s code before execution.

## 2. Forensic Fingerprinting & Initial Triage
The first step in my investigation was extracting the digital signatures (Hashes) of the sample to ensure reference integrity:
* **SHA-256:** `fdc41bda801b38ad5ccba84fc84fb9d1936b279070e952ff295ff122da3f59b3`
* **MD5:** `8135d7c655848acc1bc45c8fb43d465a`

Following this, I utilized the `strings` tool for initial text segment scanning. The first red flags emerged immediately; I detected calls to libraries unrelated to media streaming, such as `org.lsposed.lspd` and `canyie.pine`.

<div align="center">
  <img src="/assets/img/hitv/hitv4.png" width="95%" style="border: 2px solid #00d9ff; border-radius: 10px;">
  <p><i>Figure 1: Initial search reveals VMRuntime capabilities (hitv4.png).</i></p>
</div>

## 3. Structural Deconstruction & "Injection Engine" Discovery
Using `Apktool` and `JADX-GUI`, I decompiled the APK. What I found was a deliberate "orthogonal planting" of malicious code within the original application:
### A. Manifest Exploitation (Permission Profiling)
I found that the entity responsible for the mod added "aggressive" permissions granting the app total device control:

<div align="center">
  <img src="/assets/img/hitv/hitv1.png" width="95%" style="border: 2px solid #ff4444; border-radius: 10px;">
  <p><i>Figure 2: Manifest audit exposing the dangerous permissions and Gremory provider (hitv1.png).</i></p>
</div>

* **Direct Surveillance:** Access requests for the Camera (`CAMERA`) and Microphone (`RECORD_AUDIO`).
* **Data Harvesting:** The `QUERY_ALL_PACKAGES` permission allows the app to inventory every banking and wallet app on your phone.
* **Persistence:** The `BOOT_COMPLETED` permission ensures the spyware tools launch automatically upon device reboot.

### B. Advanced Injection Engine (LSPatch & Gremory Hook)
The most shocking discovery was the presence of **LSPatch** injected directly into the app. The "developer" planted a class named `com.gremory.hook` acting as a `ContentProvider`.

<div align="center">
  <img src="/assets/img/hitv/hitv2.png" width="95%" style="border: 2px solid #ff4444; border-radius: 10px;">
  <p><i>Figure 3: Technical evidence of the injected com.gremory.hook class (hitv2.png).</i></p>
</div>

> **Technical Verdict:** Utilizing a `ContentProvider` here is a professional tactic to ensure the injection code executes in the device's memory **before** any other component, allowing it to intercept (Hook) system functions and exfiltrate data in total silence.
## 4. Obfuscation Analysis (Dynamic Payload Decryption)
During my deep dive into the source code, I encountered obfuscated functions. I discovered mysterious numerical arrays (e.g., `f0short`) processed via a dynamic decryption function `m6()`.
* **Conclusion:** The developer deliberately hid real URLs and programmatic commands within these arrays to bypass traditional Anti-Virus scanners. These are decrypted only during runtime in the user's memory.

## 5. Indicator of Compromise (IOC) Extraction
I filtered the embedded links in the code using advanced `grep` commands:

<div align="center">
  <img src="/assets/img/hitv/hitv3.png" width="95%" style="border: 2px solid #ffcc00; border-radius: 10px;">
  <p><i>Figure 4: Identification of hardcoded tracking domains (hitv3.png).</i></p>
</div>

```bash
grep -Er "http" . | grep -v "google" | sort -u

```
The results revealed covert communication with Chinese domains and servers belonging to **mob.com** and **sharesdk**—tools typically used for harvesting user data for sale or targeted advertising.

---

## 6. Final Verdict (Part 1)

Based on the technical evidence I have extracted, I confirm that this free "Modded" version is a **Security Trap**. Unknown third parties have injected professional spyware tools (Pine Hooking Engine) into the application to exploit user devices.

> [!CAUTION]
> **⚠️ WARNING:** Downloading Modded programs (Mods) from unofficial sources puts your privacy and finances at immediate risk. You are not getting "Premium" for free; you are paying with your personal data.

---

## ⏳ What's Next?

The investigation is not over. In **Part 2**, I will move from **Static Analysis** to **Dynamic Analysis**. I will execute the application and monitor network traffic using **Wireshark** and **Burp Suite** to reveal with "irrefutable proof" where the harvested data is being sent.
