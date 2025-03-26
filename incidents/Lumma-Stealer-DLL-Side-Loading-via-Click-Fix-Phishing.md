# 🛡️ Lumma Stealer – DLL Side-Loading via Click Fix Phishing

**Date Detected:** Mar 13, 2025  
**Severity:** 🔴 Critical  
**Detection Rule:** SOC338 – Lumma Stealer via DLL Side-Loading  
**Category:** Data Leakage  
**EventID:** 316  
**Analyst Level:** Security Analyst  

---

## 🧠 Analyst Summary

A phishing email pretending to offer a free upgrade to Windows 11 Pro was sent from a spoofed/malicious sender. The email redirected the victim to a fake update website that hosted a disguised script (`maloy.mp4`) which utilized `mshta.exe` to deliver the **Lumma Stealer** payload via **DLL side-loading**. Post-access, the system showed indicators of credential harvesting and data exfiltration attempts, confirmed by EDR logs and threat intelligence tools.

---

## 📧 Email Details

| Field | Value |
|-------|-------|
| **SMTP IP** | `132.232.40.201` *(Flagged as malicious – VirusTotal)* |
| **Sender** | `update@windows-update.site` *(Spoofed / Malicious Domain)* |
| **Recipient** | `dylan@letsdefend.io` |
| **Subject** | `Upgrade your system to Windows 11 Pro for FREE` |
| **Device Action** | Allowed *(concerning)* |
| **Malicious Link** | `https://windows-update.site` *(Flagged by VirusTotal, IPQualityScore, Cisco Talos)* |

---

## 🔗 Malicious Redirect Link Behavior

- URL: `https://overcoatpassably.shop/Z8UZbPyVpGfdRS/maloy.mp4`
- Delivered using: `mshta.exe` via PowerShell
- Behavior: The `.mp4` extension disguises an executable payload.
- Analysis via **Any.Run**:
  - 25+ threat detections (mostly social engineering/phishing)
  - `svchost.exe` used post-execution (PID: 2196)
  - Confirmed phishing behavior (SURICATA alert)
- File Hash:  
  15c80b5be235bf2a8c38291eb697a702c07dde087eb459e9ea46a2bee17c5f03
  ## 🖥️ Log Observations

| Time | Event |
|------|-------|
| 2025-03-13 09:44 AM | Alert triggered via Email Security |
| 2025-03-13 23:26:08 | Malicious site accessed via browser |
| - | mshta.exe observed calling external link |
| - | Chrome utility unzipper process spawned (post-execution behavior) |
| - | svchost.exe initiated under suspicious context |

---
## 📌 Conclusion & Recommendations

- Email filtering should be configured to **block spoofed domains and flagged IPs**.
- The endpoint allowed execution despite clear signs of phishing — **review policy enforcement**.
- Block both domains:
- `windows-update.site`
- `overcoatpassably.shop`
- Monitor for outbound C2 connections and unusual PowerShell activity.
- Add IOCs to threat intel feeds for future correlation.
