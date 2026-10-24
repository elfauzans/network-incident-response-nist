# Network Security Incident Response: ICMP Flood Mitigation (NIST CSF)

## 📌 Executive Summary
This repository contains a comprehensive incident response case study analyzing an ICMP Flood Denial of Service (DoS) attack against a multimedia services firm. The incident compromised internal network availability for two hours due to a misconfigured edge firewall. Using the **NIST Cybersecurity Framework (CSF)**, this analysis establishes proactive and reactive security controls across all five core functions: **Identify, Protect, Detect, Respond, and Recover**.

---

## 📄 Incident Report Document
The full analysis report is available in PDF format:
- [View Full Incident Report (PDF)](./ICMP_Flood_Incident_Report_NIST.pdf)

---

## 🔍 Incident Breakdown
- **Attack Vector:** Volumetric ICMP echo-request flood from spoofed external IP addresses.
- **Root Cause:** Border firewall configuration lacked inbound ICMP inspection and rate-limiting controls.
- **Impact:** Complete degradation of internal service availability for 120 minutes.
- **Containment:** Blocked malicious inbound ICMP traffic, isolated non-critical assets, and prioritized operational network bandwidth.

---

## 🛡️ NIST CSF Implementation Matrix

| NIST Core Function | Key Measures & Controls Implemented |
| :--- | :--- |
| **Identify** | Analyzed network flow, audited boundary firewall access rules, and cataloged critical internal services impacted by downtime. |
| **Protect** | Enforced inbound ICMP rate limiting, enabled strict source IP address verification (anti-spoofing), and hardened edge device configurations. |
| **Detect** | Deployed continuous network traffic monitoring and integrated IDS/IPS with signature and anomaly-based rules for ICMP packet spikes. |
| **Respond** | Executed rapid perimeter containment, quarantined non-essential services, and extracted PCAP logs for threat triage. |
| **Recover** | Staged gradual restoration of internal workloads under telemetry observation, confirmed baseline stability, and updated the incident response playbook. |

---

## ⚙️ Perimeter Hardening Example (iptables)

Sample configuration applied on perimeter systems to throttle abusive ICMP traffic:

```bash
# Allow legitimate ICMP echo requests with rate limiting (1/sec, burst 4)
iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s --limit-burst 4 -j ACCEPT

# Drop all excess ICMP flood traffic
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
