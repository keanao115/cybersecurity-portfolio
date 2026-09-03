# Module 01: Threat Intelligence, Passive OSINT & Career Alignment

[![Module](https://img.shields.io/badge/Module-01%20OSINT%20%26%20Intel-blue.svg)](#)
[![Methodology](https://img.shields.io/badge/Methodology-Passive%20Reconnaissance-green.svg)](#)
[![Standards](https://img.shields.io/badge/Standards-NIST%20RMF%20%7C%20OSINT%20ROE-orange.svg)](#)

---

## 1. Executive Summary
This project establishes foundational competencies in **Cyber Threat Intelligence (CTI)**, **Open-Source Intelligence (OSINT)** methodologies, and **Threat Landscape Analysis**. By investigating suspicious digital artifacts through non-intrusive reconnaissance techniques, this module illustrates how defensive security analysts evaluate emerging threats, verify file integrity, correlate global indicators of compromise (IOCs), and adhere strictly to legal Rules of Engagement (ROE).

---

## 2. Global Threat Landscape Analysis
Modern enterprise environments face sophisticated threat actors exploiting systemic architectural weaknesses. This research evaluated three critical threat classes currently impacting commercial and government infrastructures:

### 2.1 Ransomware Campaigns & Double Extortion
- **Attack Pattern:** Threat actors leverage initial access brokers (IABs), unpatched external services (e.g., exposed RDP, legacy VPNs), and credential stuffing to gain footholds, followed by lateral movement and mass exfiltration before encrypting storage volumes.
- **Enterprise Impact:** Operational paralysis, regulatory fines, and reputational damage. In manufacturing and healthcare sectors, operational technology (OT) downtime represents significant hourly financial losses.

### 2.2 Supply Chain & Third-Party Compromises
- **Attack Pattern:** Infiltrating lower-security third-party vendors or compromising upstream open-source software libraries to distribute backdoors to hundreds of downstream clients simultaneously.
- **Mitigation:** Strict software supply chain validation, zero-trust network access (ZTNA), and continuous vendor risk management.

### 2.3 Identity & Credential Exploitation
- **Attack Pattern:** Exploitation of default configurations, missing multi-factor authentication (MFA), and legacy broadcast protocols (e.g., LLMNR/NBT-NS) to harvest and crack authentication hashes offline.

---

## 3. Passive OSINT Investigation: Artifact Analysis & IOC Correlation

### 3.1 Digital Artifact Verification
An unknown graphic file provided during intelligence gathering was analyzed to establish forensic integrity and correlate threat metadata without actively probing target systems.

`
+----------------------------------------------------------------------------------------------------+
| ARTIFACT PROPERTY           | FORENSIC VALUE                                                       |
+----------------------------------------------------------------------------------------------------+
| File Name                   | 600x200.jpg                                                          |
| File Format                 | JPEG Image Data (JFIF standard 1.01)                                 |
| File Dimensions             | 600 x 199 pixels                                                     |
| File Size                   | 41,560 bytes (41.56 KB)                                              |
| Cryptographic Hash (SHA256) | 3068ee023fc27c07fa4a561ec3b822b3f627978e5d64bf0a859e5f45846c557b     |
+----------------------------------------------------------------------------------------------------+
`

### 3.2 Investigated Artifact Evidence
![Investigated Digital Artifact](images/artifact-600x200.jpg)
*Figure 1.1: Cryptographically verified digital artifact analyzed for forensic metadata and threat indicators.*

### 3.2 Threat Intelligence Correlation (VirusTotal Telemetry)
The SHA-256 hash was queried against global threat intelligence repositories:
- **Detection Ratio:** 0/72 security vendors flagged the file as malicious (benign benchmark artifact).
- **First Seen in the Wild:** Earliest telemetry recorded submissions dating back several years with multiple identical cryptographic matches.
- **Infrastructure Correlation:** Passive DNS and IP metadata associated with historical distributions mapped to IP address 212.23.151.164 (Autonomous System: AS13037 - Zen Internet Ltd, United Kingdom).

`mermaid
graph LR
    A[Suspicious File: 600x200.jpg] -->|SHA-256 Calculation| B[Hash: 3068ee02...c557b]
    B -->|Passive Threat Intel Query| C[VirusTotal Intelligence Base]
    C -->|Telemetry Correlated| D[Clean Verdict: 0/72 Engines]
    C -->|Network Association| E[IP: 212.23.151.164 - AS13037]
`

---

## 4. Passive OSINT Rules of Engagement (ROE)
To ensure compliance with the **Computer Fraud and Abuse Act (CFAA)** and international cyber legislation, strict boundaries separate passive reconnaissance from active probing:

| Activity | Passive OSINT (Permitted without authorization) | Active Reconnaissance (Requires explicit written ROE) |
| :--- | :--- | :--- |
| **Domain Research** | Querying WHOIS databases, Certificate Transparency (CT) logs | Executing zone transfer attempts (dig AXFR) |
| **Search Engine Discovery** | Google dorking (site:domain.com filetype:pdf) | Submitting automated directory brute-forcing payloads |
| **Threat Intelligence** | Querying file hashes via VirusTotal, AlienVault OTX | Sending crafted packets to target endpoints |
| **Network Footprinting** | Reviewing historical BGP routing tables and ARIN records | Running port sweeps via Nmap (-sS, -A) |

---

## 5. Professional Competency & Career Framework Alignment
To build an effective roadmap toward senior cybersecurity roles, key market opportunities across both private and public sectors were scoped against the **NIST Risk Management Framework (RMF)**:

- **Target Career Path:** Entry-Level Information Security Analyst / Junior Security Engineer transitioning toward Senior Security Engineering.
- **Core Technical Pillars:**
  1. **Cloud Security Architecture & Infrastructure as Code (IaC):** Auditing IAM policies, VPC security boundaries, and zero-trust principles.
  2. **Automation & Scripting:** PowerShell and Python scripting for log analysis, threat hunting, and automated vulnerability triage.
  3. **Compliance & Risk Assessment:** Operational understanding of NIST SP 800-53 controls, NIST RMF steps, and CIS Benchmarks.
