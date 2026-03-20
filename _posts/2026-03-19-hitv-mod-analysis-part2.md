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

## 📑 Executive Summary & Methodology Shift (Improved)

In Part I, the investigation established that the modded HiTV APK communicates with non-official infrastructure and exhibits indicators of unauthorized data collection. The initial plan was to proceed directly into dynamic analysis by executing the application.

However, during the deep **Static Analysis** phase within the **REMnux** environment, multiple critical technical artifacts (IoCs) were uncovered within both the **Assets** and **Native Libraries**, requiring an immediate shift in methodology and deeper inspection before execution.

<div align="center">
  <img src="/assets/img/hitv/1.png" alt="REMnux Environment Analysis" width="85%">
  <p><i><strong>Figure 1:</strong> Forensic analysis environment using REMnux toolkit.</i></p>
</div>

These findings indicate that the attacker did not rely on superficial modifications alone. Instead, malicious logic appears to be embedded within lower-level components, specifically designed to evade traditional static inspection and mislead less experienced analysts.

This part focuses on exposing this hidden **infrastructure layer** and correlating it with the suspicious network behavior identified in Part I, before proceeding to runtime execution.

---

## 0x1 Low-Level Anatomy: The Native Binary Layer (Clarified)

While the original application is built on the **Flutter** framework, the modded version includes manipulated Native Library files (**Shared Objects - .so**), which are not typically modified in benign scenarios.

**Target Architecture & Files:**

* **Target Files:** `lib/arm64-v8a/libapp.so` & `lib/armeabi-v7a/libapp.so`
* **Attack Technique:** **Code Injection** within compiled C++ native libraries

### Binary Artifact Extraction

<div align="center">
  <img src="/assets/img/hitv/native_binary_strings.png" alt="Extracting Hardcoded Artifacts" width="85%">
  <p><i><strong>Figure 2:</strong> Extracting hardcoded artifacts from the Native Binary Layer using strings and grep utilities.</i></p>
</div>

The extracted data reveals multiple hardcoded artifacts, including API endpoints and potential encryption-related strings embedded directly in the compiled binaries.

<div align="center">
  <img src="/assets/img/hitv/ful.png" alt="Native String Analysis" width="85%">
  <p><i><strong>Figure 3:</strong> Native-level string analysis revealing hidden Command & Control (C2) endpoints within the arm64-v8a architecture.</i></p>
</div>

**Key Findings:**

* Using `strings` and `grep` within REMnux, previously hidden API endpoints and keys were extracted from the native layer—artifacts not visible within the Java/Dalvik layer.
* This separation strongly suggests intentional concealment of critical functionality.

> **Technical Insight (Strengthened):** Relocating C2 endpoints into native binaries is a known evasion technique used to bypass traditional static analysis tools and reduce detection rates in automated environments. This behavior aligns with patterns observed in advanced malware.

👉 **Security Implication:** This is not a typical modification for feature unlocking; it indicates deliberate effort to hide network communication logic.

---

## 0x2 Unmasking the Command & Control (C2) Network (Strengthened)

Building on Part I observations, the extracted artifacts confirm the existence of a structured network of endpoints that the application attempts to communicate with.

### 🌐 Hidden DNS Records & Endpoints

| Resource                    | Probable Function                  | Status during Analysis         |
| :-------------------------- | :--------------------------------- | :----------------------------- |
| `alpha-mobile-api.hitv.ltd` | Data Exfiltration Endpoint         | `NXDOMAIN` (Inactive/Rotating) |
| `dev-mobile-api.netpop.app` | Secondary/Fallback C2              | Under Observation              |
| `http://192.168.1.75:3000`  | Local Testing / Dev Infrastructure | Found in Source                |

<div align="center">
  <img src="/assets/img/hitv/4.png" alt="Infrastructure Grep Analysis" width="85%">
  <p><i><strong>Figure 4:</strong> Identified C2 infrastructure and network endpoints extracted from binary analysis.</i></p>
</div>

**Behavioral Interpretation (Improved):**
The presence of inactive (`NXDOMAIN`) domains suggests the use of **Domain Fluxing** or campaign-based infrastructure activation, where endpoints are enabled only during active data collection phases.

👉 **Security Conclusion:**
The combination of hidden endpoints + native-level concealment strongly indicates **intentional infrastructure design for covert communication**, rather than normal application behavior.

---

## 0x3 Third-Party SDK Exploitation (Clarified & Stronger Framing)

A configuration file (`ShareSDK.xml`) reveals integration with the **MobTech** platform.

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

👉 **Security Interpretation (Stronger):**
While MobTech is a legitimate analytics provider, its integration in a modded APK—combined with hidden infrastructure—raises significant concerns.

> The attacker does not need to build custom spyware; instead, they can leverage a legitimate SDK tied to a private account, allowing data collection to occur through trusted channels.

👉 **Risk Classification:**
This behavior is consistent with **abuse of legitimate services for covert data collection**, a pattern frequently observed in supply-chain-style attacks.

---

## 0x4 Advanced Obfuscation Techniques (Tighter Framing)

The Assets folder contains obfuscated JavaScript files designed to hinder analysis.


**Observed Techniques:**

* ASCII-based URL encoding (`[104,116,116,112...]`)
* Runtime string construction
* Anti-debugging / anti-emulator checks

👉 **Security Implication:**
These techniques are specifically designed to **delay or prevent detection**, and are not typical for standard media streaming functionality.

---

## 0x5 The "Smoking Gun": Persistence & Lateral Movement (Strengthened Verdict)

### Persistence Mechanisms

<div align="center">
  <img src="/assets/img/hitv/7.png" alt="Android Manifest Persistence Analysis" width="85%">
  <p><i><strong>Figure 7:</strong> Boot persistence via Android Manifest.</i></p>
</div>

**Key Findings:**

* `RECEIVE_BOOT_COMPLETED` enables automatic execution on device startup
  👉 Ensures continuous background activity without user interaction

* Excessive permissions:
  * `READ_EXTERNAL_STORAGE`
  * `ACCESS_FINE_LOCATION`

👉 **Security Interpretation:**
For a streaming application, these permissions are not functionally justified and significantly expand the potential for **data harvesting**.

---

### Lateral Movement Indicators

<div align="center">
  <img src="/assets/img/hitv/8.png" alt="Local Network Reconnaissance Evidence" width="85%">
  <p><i><strong>Figure 8:</strong> Evidence of local network targeting via hardcoded IP.</i></p>
</div>

* Hardcoded local IP: `192.168.1.75`
* Potential internal network scanning behavior

👉 **Security Interpretation (Stronger):**
This suggests the application may not be limited to device-level data collection, but could attempt **local network reconnaissance**, which significantly elevates the threat level.

---

## 🔥 Final Security Verdict (Added - Critical)

Based on the combined findings from Part I and Part II:

* Hidden C2 infrastructure embedded in native binaries
* Use of obfuscation and anti-analysis techniques
* Integration of third-party SDKs for potential covert data collection
* Persistence mechanisms and excessive permissions
* Indicators of internal network targeting

👉 **Conclusion:**
The modded HiTV APK demonstrates behavior consistent with **covert data collection and unauthorized communication with external infrastructure**.

While dynamic analysis is required for full confirmation of active data exfiltration, the current evidence strongly indicates that the application operates beyond its stated functionality.

👉 **Risk Classification:**

* Potential **Data Harvester**
* Possible **Spyware-like behavior**
* High risk to user privacy

---
