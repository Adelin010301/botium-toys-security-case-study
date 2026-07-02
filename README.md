# Botium Toys — Security Risk Assessment \& Incident Response Case Study

A fictional company case study completed as part of hands-on cybersecurity training, covering a full risk assessment/compliance audit followed by two simulated network security incidents. This project demonstrates practical application of the **NIST Cybersecurity Framework (CSF)**, compliance standards (**PCI DSS, GDPR, SOC 2**), and packet-level incident investigation using `tcpdump`.

## Table of Contents

* [Overview](#overview)
* [Part 1: Risk Assessment \& Compliance Audit](#part-1-risk-assessment--compliance-audit)
* [Part 2: Incident Response — DoS Attack](#part-2-incident-response--dos-attack)
* [Part 3: Incident Response — Brute Force \& Malware Delivery](#part-3-incident-response--brute-force--malware-delivery)
* [Key Takeaways](#key-takeaways)
* [Skills Demonstrated](#skills-demonstrated)

\---

## Overview

**Scenario:** Botium Toys, a mid-sized company with an on-site retail store, warehouse, and e-commerce operation, requested a full security audit of its assets, controls, and compliance posture. Shortly after, the organization experienced two separate security incidents that required investigation and response.

This writeup walks through the assessment methodology and the incident investigation process, root cause analysis, and remediation recommendations for each scenario.

\---

## Part 1: Risk Assessment \& Compliance Audit

### Scope

The audit covered Botium Toys' entire security program, including employee equipment, internal network, systems, data storage, and legacy infrastructure.

### Assets Reviewed

* On-premises equipment and employee devices
* Storefront/warehouse inventory systems
* Accounting, telecom, database, e-commerce, and security systems
* Internal network and internet access
* Data retention/storage and legacy (end-of-life) systems

### Findings

|Control|In Place?|Notes|
|-|-|-|
|Firewall|✅ Yes|Defined rule set in place|
|Antivirus software|✅ Yes|Regularly monitored|
|Data integrity controls|✅ Yes|Availability maintained|
|Locks / CCTV / fire detection|✅ Yes|Physical security adequate|
|GDPR 72-hour breach notification|✅ Yes|Policy documented and enforced|
|Least privilege access|❌ No|All employees can access cardholder data \& PII/SPII|
|Encryption (cardholder data)|❌ No|Credit card data stored/processed unencrypted|
|Separation of duties|❌ No|Not implemented|
|Intrusion detection system (IDS)|❌ No|Not installed|
|Disaster recovery plan / backups|❌ No|No backups of critical data|
|Password policy|⚠️ Partial|Exists but below modern complexity standards|
|Centralized password management|❌ No|Manual reset process via IT ticket|
|Legacy system monitoring schedule|⚠️ Partial|Monitored, but no defined cadence|

### Risk Score: **8 / 10** (High)

Driven primarily by the absence of encryption, access controls, IDS, and disaster recovery planning — all of which create compliance exposure under **PCI DSS** and **GDPR**, in addition to direct security risk.

### Recommendations

* Apply the **Identify** function of NIST CSF: formally inventory and classify all assets to understand impact of loss/compromise
* Implement encryption for cardholder data at rest and in transit (PCI DSS requirement)
* Enforce least privilege and separation of duties for data access
* Deploy an IDS and establish a documented disaster recovery/backup plan
* Modernize password policy and adopt a centralized password management system

\---

## Part 2: Incident Response — DoS Attack

### Summary

Botium Toys' network services suddenly became unresponsive due to an incoming flood of ICMP packets, exploiting an unconfigured firewall. The internal network was down for approximately **2 hours**.

### Investigation

* Packet capture (`tcpdump`) confirmed a high volume of inbound ICMP traffic overwhelming network resources
* Root cause: firewall lacked rate-limiting rules, allowing the ICMP flood to reach internal systems unchecked
* Classified as a **Denial of Service (DoS)** attack

### NIST CSF Response Mapping

|Function|Action Taken|
|-|-|
|**Identify**|Confirmed malicious actor(s) targeting the network with an ICMP flood; entire internal network affected|
|**Protect**|Implemented firewall rule to rate-limit incoming ICMP traffic; deployed IDS/IPS to filter suspicious ICMP traffic|
|**Detect**|Configured source IP verification to catch spoofed addresses; added network monitoring for abnormal traffic patterns|
|**Respond**|Isolated affected systems, restored critical services first, reviewed logs for further suspicious activity, reported to management|
|**Recover**|Blocked attack at firewall → stopped non-critical services → restored critical services → brought remaining systems back online once traffic normalized|

\---

## Part 3: Incident Response — Brute Force \& Malware Delivery

### Summary

Customers reported being prompted to download a file from the company website that claimed to unlock "new recipes." Shortly after, their machines began running slowly, and the site administrator discovered they were locked out of their own account.

### Investigation

Using a sandboxed environment, the analyst reproduced the issue and ran `tcpdump` to capture traffic:

1. Browser resolved and connected to `yummyrecipesforme.com` over **HTTP**
2. A file was downloaded and executed, disguised as a browser update
3. Traffic was redirected to a spoofed lookalike domain, `greatrecipesforme.com`
4. Source code review confirmed the legitimate site had been altered to serve the malicious download
5. The admin lockout indicated the attacker had used a **brute force attack** (via a default/weak password) to gain admin access and change credentials

### Root Cause

A default password on the admin account allowed unauthorized access, which the attacker used to inject malicious code serving fake "updates" that redirected users to a spoofed domain and compromised their machines.

### Remediation Recommendations

* **Disallow reuse of previous/default passwords** during password resets
* **Enforce more frequent password rotation** to reduce the window of exposure for leaked credentials
* **Implement two-factor authentication (2FA)** on all administrative accounts, requiring a one-time passcode in addition to the password — significantly reducing the success rate of brute force attempts

\---

## Key Takeaways

* A single missing control (encryption, IDS, or MFA) rarely causes a breach on its own — but Botium Toys' *combination* of gaps (weak passwords + no MFA + unpatched firewall) created multiple simultaneous attack paths.
* Compliance and security controls are directly linked: PCI DSS and GDPR requirements map almost one-to-one onto practical technical controls (encryption, access management, breach notification).
* Incident response is most effective when structured around a consistent framework (NIST CSF's Identify–Protect–Detect–Respond–Recover) rather than ad hoc troubleshooting.
* Brute force attacks are best mitigated with layered controls — password policy alone is insufficient without MFA and account lockout thresholds.

## Skills Demonstrated

`Risk Assessment` · `NIST CSF` · `PCI DSS` · `GDPR` · `SOC 2` · `Packet Analysis (tcpdump)` · `Incident Documentation` · `Root Cause Analysis` · `Access Control Design` · `Compliance Auditing`

\---

*This is a fictional case study completed for cybersecurity skills development and portfolio purposes.*

