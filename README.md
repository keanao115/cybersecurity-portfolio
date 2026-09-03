# Enterprise Cybersecurity Portfolio: Vulnerability Assessment & Defensive Engineering

[![Security Focus](https://img.shields.io/badge/Security-VAPT%20%7C%20SOC%20Analysis%20%7C%20Network%20Hardening-red.svg)](#)
[![Standards](https://img.shields.io/badge/Framework-NIST%20CSF%20%7C%20MITRE%20ATT%26CK%20%7C%20PTES-blue.svg)](#)
[![Virtualization](https://img.shields.io/badge/Environment-VirtualBox%20%7C%20Kali%20%7C%20Windows%2011%20%7C%20Ubuntu-orange.svg)](#)
[![Status](https://img.shields.io/badge/Portfolio%20Status-Complete-brightgreen.svg)](#)

---

## Executive Overview
Welcome to my Cybersecurity Engineering & Vulnerability Assessment portfolio. This repository compiles a comprehensive, multi-phase security evaluation conducted within an isolated, simulated enterprise computing environment. 

Rather than isolated tool demonstrations, these projects reflect the complete lifecycle of modern information security operations:
1. **Threat Intelligence & Passive OSINT** — Investigating adversary infrastructure, file integrity, and aligning defensive capabilities.
2. **Controlled Lab Infrastructure** — Architecting hypervisor-isolated Layer 2 broadcast environments and establishing defensive network baselines.
3. **Active Reconnaissance & Vulnerability Assessment** — Network enumeration, automated vulnerability auditing (Nessus/Nmap), and threat surface prioritization.
4. **External Penetration Testing & Access Acquisition** — Exploiting critical service-level vulnerabilities, achieving remote code execution, and evaluating post-exploitation persistence.
5. **Internal Network Adversary Simulation & Defense** — Intercepting broadcast authentication protocols (LLMNR/NBT-NS), offline cryptanalysis, cleartext traffic forensics, and implementing enterprise Group Policy (GPO) defenses.
6. **Executive Outbrief & Strategic Remediation** — Delivering executive-level briefings for C-suite leadership and building prioritized, NIST CSF-aligned remediation roadmaps balancing security controls against mission-critical operational downtime.

---

## Portfolio Modules & Navigation

| Module | Focus Area | Core Technologies / Protocols | Key Deliverable |
| :--- | :--- | :--- | :--- |
| [**Module 01**](portfolio/01-osint-threat-intelligence) | **OSINT & Threat Intelligence** | SHA256, VirusTotal, Passive DNS, WHOIS, ROE | [Threat Landscape & Artifact Analysis](portfolio/01-osint-threat-intelligence) |
| [**Module 02**](portfolio/02-homelab-network-isolation) | **Homelab Infrastructure & Isolation** | VirtualBox, Host-Only Subnet, TCP/IP TTL, UFW | [Isolated Lab Architecture & Network Baseline](portfolio/02-homelab-network-isolation) |
| [**Module 03**](portfolio/03-reconnaissance-vulnerability-scan) | **Reconnaissance & Vulnerability Assessment** | Nmap, Nessus Professional, SMB (:445), CVE/CVSS | [Vulnerability Audit Report & Attack Surface Map](portfolio/03-reconnaissance-vulnerability-scan) |
| [**Module 04**](portfolio/04-external-penetration-testing) | **External Penetration Testing Results** | Metasploit, Samba RCE, vsftpd Backdoor, RDP | [External Pentest & Exploitation Findings](portfolio/04-external-penetration-testing) |
| [**Module 05**](portfolio/05-internal-attacks-and-defense) | **Internal Network Attacks & Hardening** | Responder, John the Ripper, Wireshark, GPO | [Internal Attack Results & Defensive Playbook](portfolio/05-internal-attacks-and-defense) |
| [**Module 06**](portfolio/06-enterprise-vapt-executive-outbrief) | **Executive Outbrief & Remediation Plan** | NIST CSF, OT Risk Analysis, Downtime Economics | [Executive Briefing & Strategic Remediation Roadmap](portfolio/06-enterprise-vapt-executive-outbrief) |

---

## Simulated Enterprise Architecture
The technical evaluations documented across Modules 03 through 06 were executed against an authorized, simulated enterprise manufacturing environment ([Target Organization]). 

`mermaid
graph TB
    subgraph Attacker_Perimeter [Security Assessment Node]
        Kali[Kali Linux 2026.x<br>IP: 192.168.56.101<br>Auditing Platform]
    end

    subgraph Flat_Corporate_Subnet [Simulated Enterprise Subnet: 192.168.56.0/24 - Host-Only Isolation]
        Target_Win[Windows 11 Enterprise Node<br>IP: 192.168.56.104<br>Endpoint Simulation & GPO Target]
        Target_Linux[Legacy Manufacturing Server<br>IP: 192.168.56.102<br>Mission-Critical CAM Host]
    end

    subgraph Simulated_Enterprise_Scope [Target Organization Context]
        Corp_Scale[900 Windows 11 Endpoints<br>40 Legacy Windows 2003 Servers<br>70 Linux CAM Nodes / Servers]
        Business_Impact[Critical Constraint: Manufacturing Downtime = ,000 / hr per server]
    end

    Kali -->|LLMNR Poisoning & NetNTLMv2 Interception| Target_Win
    Kali -->|RCE Exploitation via Samba / vsftpd| Target_Linux
    Flat_Corporate_Subnet -.-> Corp_Scale
`

---

## Technical Competency Matrix

`
+---------------------------------------------------------------------------------------+
| DOMAIN                         | TOOLS & PROTOCOLS                                    |
+---------------------------------------------------------------------------------------+
| Vulnerability Assessment       | Nessus, Nmap, Searchsploit, Exploit-DB, CVSS v3.1    |
| Exploitation & Post-Exploit    | Metasploit Framework, Meterpreter, msfvenom, Hydra    |
| Identity & Protocol Auditing   | Responder, John the Ripper, CrackStation, NBT-NS     |
| Traffic Analysis & Forensics   | Wireshark, TCPDump, Netstat, Promiscuous Sniffing    |
| Defensive Hardening            | Windows GPO, SMB Signing, UFW, Linux Hardening       |
| Governance & Frameworks        | NIST CSF, MITRE ATT&CK, PTES, Rules of Engagement   |
+---------------------------------------------------------------------------------------+
`

---

## Professional Objectives
- **Target Roles:** Entry-Level Information Security Analyst, Junior SOC Analyst, Penetration Testing Intern, Junior Security Engineer.
- **Key Differentiator:** The ability to not only identify and validate technical vulnerabilities but also articulate business impact, quantify operational risks, and formulate prioritized, framework-compliant remediation roadmaps.

---

## Ethical Disclosure & Compliance Statement
All penetration tests, scans, and adversary simulations documented in this portfolio were conducted in a strictly controlled, host-only laboratory environment under pre-defined Rules of Engagement. All organizational names and network identifiers have been anonymized or represented through generic enterprise placeholders ([Target Organization]) to ensure compliance with professional non-disclosure standards and academic integrity guidelines.
