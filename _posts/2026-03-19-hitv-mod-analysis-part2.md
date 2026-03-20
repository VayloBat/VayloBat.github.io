---
title: "Security Research: Infrastructure Analysis of HiTV Modded APK (Part 2)"
layout: post
date: 2026-03-19 10:00:00 +0100
categories: [Research, Malware-Analysis]
tags: [Android, ReverseEngineering, Security, REMnux, C2]
---

## 🛡️ Legal Disclaimer

> **This research is intended for educational purposes and security awareness only. The information provided is the result of reverse engineering analysis to uncover latent threats in modified applications. The author assumes no responsibility for the misuse of this information in any illegal activity. Remember: "Privacy is a right, but protection is your personal responsibility."**

---

## 📑 Executive Summary & Methodology Shift

In Part I, the investigation established that the modded HiTV APK communicates with non-official infrastructure and exhibits indicators of unauthorized data collection. The initial plan was to proceed directly into dynamic analysis by executing the application.

However, during the deep **Static Analysis** phase within the **REMnux** environment, multiple critical technical artifacts (IoCs) were uncovered within both the **Assets** and **Native Libraries**, requiring an immediate shift in methodology and deeper inspection before execution.

<p><i><strong>Figure 1:</strong> Forensic analysis environment using REMnux toolkit. (Image pending)</i></p>

These findings indicate that the attacker did not rely on superficial modifications alone. Malicious logic is embedded in lower-level components, specifically designed to evade traditional static inspection and mislead less experienced analysts.

This part focuses on exposing this hidden **infrastructure layer** and correlating it with the suspicious network behavior identified in Part I, before proceeding to runtime execution.

---

## 0x1 Low-Level Anatomy: The Native Binary Layer

While the original application is built on the **Flutter** framework, the modded version includes manipulated Native Library files (**Shared Objects - .so**).

**Target Architecture & Files:**
* **Target Files:** `lib/arm64-v8a/libapp.so` & `lib/armeabi-v7a/libapp.so`
* **Attack Technique:** **Code Injection** within compiled C++ native libraries

### Binary Artifact Extraction

<div align="center">
  <img src="/assets/img/hitv/native_binary_strings.png" alt="Extracting Hardcoded Artifacts" width="85%">
  <p><i><strong>Figure 2:</strong> Extracting hardcoded artifacts from the Native Binary Layer using strings and grep utilities.</i></p>
</div>

The analysis revealed suspicious API endpoints and encryption keys embedded directly in the compiled binaries.

<div align="center">
  <img src="/assets/img/hitv/ful.png" alt="Native String Analysis" width="85%">
  <p><i><strong>Figure 3:</strong> Native-level string analysis revealing hidden Command & Control (C2) endpoints within the arm64-v8a architecture.</i></p>
</div>

**Key Findings:**
* Using `strings` and `grep` within REMnux, previously hidden API endpoints and keys were extracted from the native layer—artifacts not visible in the Java/Dalvik layer.
* Intentional concealment is evident.

> **Technical Insight:** Relocating C2 endpoints into native binaries is a known evasion tactic used to bypass standard static analysis and automated sandbox detection.

---

## 0x3 Third-Party SDK Exploitation (The "ShareSDK" Cover)

A critical configuration file (`ShareSDK.xml`) reveals unauthorized integration of the **MobTech** platform.

### ShareSDK Configuration Analysis

<div align="center">
  <img src="/assets/img/hitv/sharesdk_xml_evidence.png" alt="ShareSDK Configuration Exposure" width="85%">
  <p><i><strong>Figure 5a:</strong> Exposed MobTech AppKey and AppSecret within the Assets folder.</i></p>
</div>

<div align="center">
  <img src="/assets/img/hitv/remnux_grep_appkey.png" alt="AppKey Cross-Reference Tracing" width="85%">
  <p><i><strong>Figure 5b:</strong> Deep tracing of AppKey across Assets, XML, and Native binaries.</i></p>
</div>

**Extracted Evidence:**
* `AppKey`: `558980885691176`
* `AppSecret`: `e515ca506252c4468052257aad185021`

> **Security Interpretation:** Using a legitimate SDK to collect data via a private account masks malicious activity, a sophisticated supply-chain abuse pattern.

---

## 0x4 Advanced Obfuscation Techniques

<div align="center">
  <img src="/assets/img/hitv/js_obfuscation_code.png" alt="JavaScript Obfuscation Analysis" width="85%">
  <p><i><strong>Figure 6:</strong> ASCII-based obfuscation hiding URLs and C2 endpoints.</i></p>
</div>

* ASCII-based URL encoding  
* Runtime string assembly  
* Anti-debug/emulator checks  

**Implication:** Prevents static analysis detection and slows down automated scanning.

---

## 0x5 The "Smoking Gun": Persistence & Lateral Movement

<div align="center">
  <img src="/assets/img/hitv/manifest_persistence_checks.png" alt="Android Manifest Persistence Analysis" width="85%">
  <p><i><strong>Figure 7:</strong> Boot persistence via Android Manifest.</i></p>
</div>

**Key Findings:**
* `RECEIVE_BOOT_COMPLETED` ensures background service auto-start  
* Excessive permissions (`READ_EXTERNAL_STORAGE`, `ACCESS_FINE_LOCATION`) → High data harvesting potential

<div align="center">
  <img src="/assets/img/hitv/local_ip_leak.png" alt="Local Network Reconnaissance Evidence" width="85%">
  <p><i><strong>Figure 8:</strong> Evidence of local network targeting via hardcoded IP.</i></p>
</div>

**Security Interpretation:**  
Potential local network reconnaissance and lateral movement.

---

## 🔥 Final Security Verdict

- Native C2 endpoints  
- Obfuscation & anti-analysis  
- Third-party SDK misuse  
- Persistence & excessive permissions  
- Local network targeting  

**Conclusion:** The modded HiTV APK demonstrates behavior consistent with **covert data collection** and unauthorized communication with external infrastructure, indicating potential spyware-like activity.

---
