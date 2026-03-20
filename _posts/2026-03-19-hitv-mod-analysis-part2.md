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

In Part I, the initial plan was to transition directly into dynamic analysis with the app running. However, during the deep **Static Analysis** phase within the **REMnux** environment, I uncovered critical technical artifacts (IoCs) within the **Assets** and **Native Libraries** that demanded an immediate pause.

<div align="center">
  <img src="remnux_setup.png" alt="REMnux Environment Analysis" width="85%">
  <p><i><strong>Figure 1:</strong> Forensic analysis environment using REMnux toolkit.</i></p>
</div>

I discovered that the attacker did not settle for surface-level modifications; instead, they injected malicious code at a low-level layer that evades standard scans and novice analysts. This part is dedicated to exposing this "ghost infrastructure" before we hit the 'Run' button.

---

## 0x1 Low-Level Anatomy: The Native Binary Layer

While the original application is built on the **Flutter** framework, the "Modded" version contains manipulated Native Library files (**Shared Objects - .so**).

**Target Architecture & Files:**
* **Target Files:** `lib/arm64-v8a/libapp.so` & `lib/armeabi-v7a/libapp.so`
* **Attack Technique:** **Code Injection** within C++ compiled libraries

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
* Utilizing advanced `strings` and `grep` utilities in REMnux, I extracted hardcoded API keys and hidden URLs that are completely absent from the standard Java/Dalvik layer.
* These artifacts indicate intentional obfuscation at the native code level.

> **Technical Insight:** Moving C2 endpoints to the Native Code is a deliberate tactic to bypass traditional Static Analysis and mislead automated sandbox environments. This technique is commonly employed in advanced malware.

---

## 0x2 Unmasking the Command & Control (C2) Network

The analysis confirmed a "suspicious call-list" the application attempts to contact for data exfiltration. Here is the breakdown of the intercepted endpoints:

### 🌐 Hidden DNS Records & Endpoints

| Resource | Probable Function | Status during Analysis |
| :--- | :--- | :--- |
| `alpha-mobile-api.hitv.ltd` | Data Exfiltration Point | `NXDOMAIN` (Down/Temporary) |
| `dev-mobile-api.netpop.app` | Secondary/Fallback C2 Server | Under Observation |
| `http://192.168.1.75:3000` | Attacker's Local Lab Environment | Traced in Source Code |

<div align="center">
  <img src="grep_infrastructure.png" alt="Infrastructure Grep Analysis" width="85%">
  <p><i><strong>Figure 4:</strong> Identified C2 infrastructure and network endpoints extracted from binary analysis.</i></p>
</div>

**Behavioral Analysis:** The `NXDOMAIN` status for certain domains suggests the attacker employs **Domain Fluxing** or temporary DNS records that are only activated during an active campaign—a hallmark of advanced persistent threats (APTs).

---

## 0x3 Third-Party SDK Exploitation (The "ShareSDK" Cover)

A critical configuration file named `ShareSDK.xml` was discovered, revealing unauthorized integration of the **MobTech** platform—a legitimate data analytics service weaponized for malicious purposes.

### ShareSDK Configuration Analysis

<div align="center">
  <img src="/assets/img/hitv/sharesdk_xml_evidence.png" alt="ShareSDK Configuration Exposure" width="85%">
  <p><i><strong>Figure 5a:</strong> Exposed MobTech AppKey and AppSecret within the Assets folder.</i></p>
</div>

<div align="center">
  <img src="/assets/img/hitv/remnux_grep_appkey.png" alt="AppKey Cross-Reference Tracing" width="85%">
  <p><i><strong>Figure 5b:</strong> Deep Tracing: The AppKey (558980885691176) embedded across Assets, XML resources, and Native Binaries.</i></p>
</div>

**Extracted Evidence:**
* `AppKey`: `558980885691176`
* `AppSecret`: `e515ca506252c4468052257aad185021`

**The Risk:** The attacker doesn't need to build a spy system from scratch; they simply leverage a legitimate SDK (ShareSDK) and link it to their private account. Consequently, your data travels through "legitimate" channels, masking the theft. This is a sophisticated supply-chain attack pattern.

---

## 0x4 Advanced Obfuscation Techniques

Within the Assets folder, specifically JavaScript scripts like `omsdk-v1.js` and `omid_validation_verification_script_v1.js`, I found sophisticated code manipulation designed to evade detection.

<div align="center">
  <img src="js_obfuscation_code.png" alt="JavaScript Obfuscation Analysis" width="85%">
  <p><i><strong>Figure 6:</strong> Use of ASCII Integer Arrays to hide malicious URLs and C2 endpoints.</i></p>
</div>

**Obfuscation Methods Identified:**

* **URL Encoding via ASCII Arrays:** Conversion of URLs into ASCII Integer Arrays (e.g., `[104, 116, 116, 112...]` instead of readable `http` strings)
* **Anti-Debugging Mechanisms:** The code contains environment-check functions; if it detects an **Emulator** or a **Debugger**, it alters its behavior or terminates to avoid detection
* **Runtime String Assembly:** Dynamic string construction prevents static analysis tools from identifying malicious payloads

These techniques significantly complicate the analysis process for security researchers and automated detection systems.

---

## 0x5 The "Smoking Gun": Deep Persistence & Lateral Movement

Beyond the C2 endpoints, the following hidden mechanisms were identified in the `AndroidManifest.xml` and bytecode:

### Persistence Mechanisms

<div align="center">
  <img src="manifest_persistence_checks.png" alt="Android Manifest Persistence Analysis" width="85%">
  <p><i><strong>Figure 7:</strong> Automated boot persistence identified in the Android Manifest configuration.</i></p>
</div>

**Key Findings:**

* **Persistence (The Ghost in the Machine):** The inclusion of the `RECEIVE_BOOT_COMPLETED` intent filter allows malicious background services to auto-start every time the phone is rebooted. This ensures the attacker maintains access even if the app is never opened.

* **Dangerous Permissions Abuse:** The app requests excessive access, including:
  - `READ_EXTERNAL_STORAGE` (access to all media files)
  - `ACCESS_FINE_LOCATION` (real-time GPS tracking)
  
  In a modded movie app, these are "Red Flags" that allow for wholesale theft of personal photos and real-time movement tracking.

### Lateral Movement Capabilities

<div align="center">
  <img src="local_ip_leak.png" alt="Local Network Reconnaissance Evidence" width="85%">
  <p><i><strong>Figure 8:</strong> Hardcoded local IP address evidence indicating network reconnaissance capabilities.</i></p>
</div>

* **Internal Network Reconnaissance:** The hardcoded local IP `192.168.1.75` suggests potential **Lateral Movement** capabilities within the victim's network
* **Device Enumeration:** Malicious scripts often scan the victim's local Wi-Fi network to identify and exploit other connected devices
* **Router/Gateway Targeting:** This pattern indicates the malware may attempt to pivot from the compromised phone to other IoT devices or computers on the network

---

## 🛡️ How to Protect Yourself

Based on this forensic evidence, users should be aware of the following detection indicators:

### Detection Indicators

**1. LSPatch/Xposed Artifacts**
If you decompile an APK and find folders named `lspatch`, it is a definitive sign of code injection. LSPatch is a tool used to inject code into APKs post-compilation.

**2. VirusTotal Limitations**
A "Green" scan on VirusTotal can be deceptive. Many scanners fail to analyze the **Native Layer (.so files)** where malicious code often resides. This is why manual analysis is crucial.

**3. Legitimacy Check**
A movie streaming app has zero functional requirement for high-level tracking SDKs like MobTech. This discrepancy is the ultimate proof of malicious intent. Always question unnecessary permissions and SDKs.

### Recommended Security Practices

- Use legitimate app stores only
- Regularly review app permissions
- Monitor network traffic using tools like mitmproxy or Burp Suite
- Keep your Android OS updated
- Consider using a dedicated security-focused ROM (e.g., GrapheneOS, CalyxOS)

---

## 🚩 Indicators of Compromise (IoCs)

For researchers wishing to replicate this analysis, here are the digital fingerprints of the threat:

**Malicious Domains:**
- `*.hitv.ltd`
- `*.netpop.app`

**Hardcoded Credentials:**
- `MobTech AppKey: 558980885691176`
- `MobTech AppSecret: e515ca506252c4468052257aad185021`

**Technical Signatures:**
- `String.fromCharCode(104, 116, 116, 112, ...)` (ASCII Array Obfuscation Pattern)
- `RECEIVE_BOOT_COMPLETED` + `READ_EXTERNAL_STORAGE` + `ACCESS_FINE_LOCATION` permission combination
- Hardcoded IP: `192.168.1.75:3000`

**File Artifacts:**
- Native library modification in `lib/arm64-v8a/` and `lib/armeabi-v7a/`
- Presence of `ShareSDK.xml` with suspicious MobTech configuration

---

## ⏭️ What's Next in Part 3?

Now that we have mapped the infrastructure and extracted the critical IoCs, we are ready for the most dangerous phase: **Dynamic Analysis**.

**Planned Analysis Steps:**

1. **SSL Pinning Bypass** using **Frida** to hook certificate validation functions
2. **Traffic Interception** running the app in an isolated environment and capturing **HTTP/HTTPS** traffic via **Burp Suite**
3. **Data Exfiltration Monitoring** to intercept "stolen data packets" mid-transit and determine the full scale of the breach
4. **C2 Communication Analysis** to reverse-engineer the protocol and payload structure
5. **Behavioral Profiling** to create a timeline of malicious activities

Stay tuned for Part III where we'll execute these dynamic analysis techniques in a secured sandbox environment.

---

**Enjoyed this deep dive into malware infrastructure? Follow me on GitHub to catch Part 3 as soon as it drops. For questions or collaboration on security research, feel free to reach out.**


---


