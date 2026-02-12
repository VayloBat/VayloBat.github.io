---
layout: post
title: "dissecting forgotten devices: a deep dive into ZTE dodem architecture (part 1)"
date: 2026-02-12 10:00:00 +0100

---

## 0x01: The Motivation
Why would a security researcher bother with a legacy device? The answer is simple: **Embedded systems don't die; they are just forgotten.** These devices, still powering many networks today, contain "software fossils" that provide a perfect environment for understanding vulnerability development from the ground up. In this series, I will dismantle a ZTE 3G Modem step-by-step—not just to find a bug, but to understand the mindset of the developer who wrote the code a decade ago.

---

## 0x02: Reconnaissance (Mapping the Battlefield)
The process began with **Attack Surface Mapping**. The goal was to identify every possible entry point into the operating system.

Using `nmap` with precise parameters, I performed a full port scan and service fingerprinting:

<div class="img-wrapper">
  <img src="/assets/img/p01.png" alt="Nmap Scan Results" class="shadow rounded-10 w-100">
  <p class="text-center font-italic">The technical output clearly indicates a MediaTek-based architecture running services from a bygone era.</p>
</div>

### Critical Findings:
1.  **GoAhead WebServer 2.5.0:** This isn't just a web server; it's a goldmine for vulnerabilities. This specific version lacks modern mitigations against Buffer Overflows and Remote Code Execution (RCE).
2.  **dnsmasq 2.55:** An ancient version, opening the possibility for memory corruption attacks during DNS packet processing.

---
## 0x03: Behavioral Analysis (Fingerprinting the Target)
I didn't stop at port scanning; I wanted to see how the device reacts to raw requests. By analyzing the HTTP headers using `curl`:

<div class="img-wrapper">
  <img src="/assets/img/p03.png" alt="Curl Header Analysis" class="shadow rounded-10 w-100">
  <p class="text-center font-italic">Notice the system date is set to 1970. This confirms we are dealing with a "Raw Environment" that has likely never seen a security patch.</p>
</div>

---
## 0x04: The Attack Plan
Based on the gathered intelligence, I have formulated the following **Attack Vectors**:

* **Vector A (Static Analysis):** Firmware acquisition and extraction to hunt for hidden CGI scripts and logic flaws in the management interface.
* **Vector B (Fuzzing):** Sending malformed inputs to the DNS service to test memory stability and boundary conditions.

---

## 0x05: The Research Status

<div class="img-wrapper">
  <img src="/assets/img/p03.jpg" alt="Research Workflow" class="shadow rounded-10 w-75 mx-auto d-block">
</div>

```yaml
Current Phase: 01 (Reconnaissance & Mapping)
Confidence Level: High
Next Target: Firmware Binwalk & Extraction
Estimated Risk: Critical (Due to outdated service versions)
```
