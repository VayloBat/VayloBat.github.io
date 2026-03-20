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
  <p><i>Figure 1: Forensic analysis environment using REMnux toolkit.</i></p>
</div>

I discovered that the attacker did not settle for surface-level modifications; instead, they injected malicious code at a low-level layer that evades standard scans and novice analysts. This part is dedicated to exposing this "ghost infrastructure" before we hit the 'Run' button.

---

## 0x1 Low-Level Anatomy: The Native Binary Layer:
While the original application is built on the **Flutter** framework, the "Modded" version contains manipulated Native Library files (**Shared Objects - .so**).

* **Target Files:** `lib/arm64-v8a/libapp.so` & `lib/armeabi-v7a/libapp.so`.
* **Technique:** **Code Injection** within C++ compiled libraries.

<div align="center">
  <img src="/assets/img/hitv/native_binary_strings.png" alt="Binary Strings Extraction" width="85%">
  <p><i>Figure 2: Extracting hardcoded artifacts from the Native Binary Layer.</i></p>
</div>

* **Findings:** Utilizing advanced `strings` and `grep` utilities in REMnux, I extracted hardcoded API keys and hidden URLs that are completely absent from the standard Java/Dalvik layer.

> **Technical Insight:** Moving C2 endpoints to the Native Code is a deliberate tactic to bypass traditional Static Analysis and mislead automated sandbox environments.

---

## 0x2 Unmasking the Command & Control (C2) Network:
The analysis confirmed a "suspicious call-list" the application attempts to contact for data exfiltration. Here is the breakdown of the intercepted endpoints:

### 🌐 Hidden DNS Records & Endpoints
| Resource | Probable Function | Status during Analysis |
| :--- | :--- | :--- |
| `alpha-mobile-api.hitv.ltd` | Data Exfiltration Point | `NXDOMAIN` (Down/Temporary) |
| `dev-mobile-api.netpop.app` | Secondary/Fallback C2 Server | Under Observation |
| `http://192.168.1.75:3000` | Attacker's Local Lab Environment | Traced in Source Code |

<div align="center">
  <img src="grep_infrastructure.png" alt="Infrastructure Grep Analysis" width="85%">
  <p><i>Figure 3: Identified C2 infrastructure and network endpoints.</i></p>
</div>

**Behavioral Analysis:** The `NXDOMAIN` status for certain domains suggests the attacker employs **Domain Fluxing** or temporary DNS records that are only activated during an active campaign—a hallmark of advanced persistent threats (APTs).

---

## 0x3 Third-Party SDK Exploitation (The "ShareSDK" Cover):
A critical configuration file named `ShareSDK.xml` was discovered. It revealed the unauthorized integration of the **MobTech** platform.

​<div align="center">
<table width="100%">
<tr>
<td width="50%" align="center">
<img src="/assets/img/hitv/sharesdk_xml_evidence.png" alt="ShareSDK Key GUI" width="100%">
<p><i>Figure 4a: Exposed MobTech AppKey and AppSecret in the JADX-GUI.</i></p>
</td>
<td width="50%" align="center">
<img src="/assets/img/hitv/remnux_grep_appkey.png" alt="Grep Results for AppKey" width="100%">
<p><i>Figure 4b: Deep Tracing: The AppKey (558980885691176) embedded across Assets, XML resources, and Native Binaries.</i></p>
</td>
</tr>
</table>
</div>
* **Extracted Evidence:**
    * `AppKey`: `558980885691176`
    * `AppSecret`: `e515ca506252c4468052257aad185021`

**The Risk:** The attacker doesn't need to build a spy system from scratch; they simply leverage a legitimate SDK (ShareSDK) and link it to their private account. Consequently, your data travels through "legitimate" channels, masking the theft.

---

## 0x4 Advanced Obfuscation Techniques:
Within the Assets folder, specifically JavaScript scripts like `omsdk-v1.js` and `omid_validation_verification_script_v1.js`, I found sophisticated code manipulation:

<div align="center">
  <img src="js_obfuscation_code.png" alt="JavaScript Obfuscation" width="85%">
  <p><i>Figure 5: Use of ASCII Integer Arrays to hide malicious URLs.</i></p>
</div>

* **Encoding:** Conversion of URLs into ASCII Integer Arrays (e.g., `[104, 116, 116, 112...]` instead of `http`).
* **Anti-Debugging:** The code contains environment-check functions; if it detects an **Emulator** or a **Debugger**, it alters its behavior or terminates to avoid detection.

---

## 0x5 The "Smoking Gun": Deep Persistence & Lateral Movement:
Beyond the C2 endpoints, the following hidden mechanisms were identified in the `AndroidManifest.xml` and bytecode:

<div align="center">
  <img src="manifest_persistence_checks.png" alt="Manifest Analysis" width="85%">
  <p><i>Figure 6: Automated boot persistence identified in the Android Manifest.</i></p>
</div>

* **Persistence (The Ghost in the Machine):** The inclusion of the `RECEIVE_BOOT_COMPLETED` intent filter allows the malicious background services to auto-start every time the phone is rebooted. This ensures the attacker maintains access even if the app is never opened.
* **Dangerous Permissions:** The app requests excessive access, including `READ_EXTERNAL_STORAGE` and `ACCESS_FINE_LOCATION`. In a modded movie app, these are "Red Flags" that allow for the wholesale theft of personal photos and real-time movement tracking.

<div align="center">
  <img src="local_ip_leak.png" alt="Hardcoded Local IP Evidence" width="85%">
  <p><i>Figure 7: Evidence of local IP address leakage for network reconnaissance.</i></p>
</div>

* **Internal Reconnaissance:** The hardcoded local IP `192.168.1.75` suggests potential **Lateral Movement** capabilities. Malicious scripts often scan the victim's local Wi-Fi network to identify and exploit other connected devices.

---

## 🛡️ How to Protect Yourself
Based on this forensic evidence, users should be aware:
1. **LSPatch/Xposed Artifacts:** If you decompile an APK and find folders named `lspatch`, it is a definitive sign of code injection.

<div align="center">
  <img src="lspatch_artifacts.png" alt="LSPatch Evidence" width="85%">
  <p><i>Figure 8: Injected LSPatch artifacts found within the application resources.</i></p>
</div>

2. **VirusTotal Limitations:** A "Green" scan on VirusTotal can be deceptive. Many scanners fail to analyze the **Native Layer (.so files)** where we found the actual malicious keys.
3. **Legitimacy Check:** A movie streaming app has zero functional requirement for high-level tracking SDKs like MobTech. This discrepancy is the ultimate proof of malicious intent.

---

## 🚩 Indicators of Compromise (IoCs)
For researchers wishing to replicate this analysis, here are the digital fingerprints of the threat:

- **Domains:** `*.hitv.ltd`, `*.netpop.app`
- **Keys:** `MobTech AppKey: 558980885691176`
- **Obfuscation Patterns:** `String.fromCharCode(104, 116, 116, 112, ...)`

---

## ⏭️ What’s Next in Part3 ?
Now that we have mapped the targets and extracted the encrypted addresses, we are ready for the most dangerous phase: **Dynamic Analysis**.
1. Breaking **SSL Pinning** protection using **Frida**.
2. Running the app in a strictly isolated environment and intercepting **HTTP Traffic** via **Burp Suite**.
3. Attempting to intercept "stolen data packets" mid-transit to determine the full scale of the breach.

---
**Enjoyed this deep dive? Follow me to catch Part III as soon as it drops.**

