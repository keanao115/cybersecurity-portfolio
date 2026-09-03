# Module 04: External Penetration Testing & Exploitation Assessment

[![Module](https://img.shields.io/badge/Module-04%20External%20Exploitation%20Report-blue.svg)](#)
[![Tone](https://img.shields.io/badge/Tone-Professional%20Pentest%20Report-red.svg)](#)
[![Standards](https://img.shields.io/badge/Framework-PTES%20%7C%20NIST%20CSF-orange.svg)](#)

---

## 1. Executive Summary
During the external penetration testing phase conducted against **[Target Organization]**, the security team simulated an external adversary positioned on the network perimeter. The assessment focused on evaluating whether exposed network services could be weaponized to achieve unauthorized access, execute arbitrary code, and establish persistent footholds without triggering enterprise alarms.

The assessment concluded with **Critical** findings: unauthenticated remote code execution (RCE) was successfully achieved on core production servers, and administrative persistence was established on enterprise Windows endpoints.

---

## 2. Scope & Target Inventory
The authorized scope encompassed targeted evaluation of perimeter endpoints and core production nodes within the simulated enterprise boundary:
- **Target Host Alpha (Production Linux Server):** 192.168.56.102 (Hosting legacy manufacturing and management services)
- **Target Host Beta (Corporate Windows Endpoint):** 192.168.56.104 (Hosting enterprise administrative access and client services)

---

## 3. Technical Findings & Verification of Impact

`
+-------------------------------------------------------------------------------------------------------+
| ID      | FINDING TITLE                                        | SEVERITY | CVSS v3.1 | ACCESS GAINED |
+-------------------------------------------------------------------------------------------------------+
| EXT-01  | Samba 3.0.20 'usermap_script' Remote Code Execution  | CRITICAL | 9.8       | Full Root RCE |
| EXT-02  | vsftpd 2.3.4 Backdoor Execution                      | CRITICAL | 9.8       | Root Shell    |
| EXT-03  | Weak Credential & Remote Desktop Persistence         | HIGH     | 8.8       | Admin Takeover|
+-------------------------------------------------------------------------------------------------------+
`

---

### Finding EXT-01: Samba 3.0.20 'usermap_script' Remote Code Execution (CVE-2007-2447)
- **Vulnerability Identified:** The target production server running Samba 3.0.20 was found vulnerable to input validation failure in the username map script configuration directive. When parsing MS-RPC authentication requests containing shell metacharacters, the daemon passes unsanitized user input directly to the system shell.
- **Verified Impact & Access Level Achieved:**
  - **Unauthenticated Root Access:** Successful exploitation was achieved remotely without requiring valid domain or local user credentials.
  - **Interactive Session & Elevation:** The assessment team established an interactive reverse shell operating under uid=0(root) gid=0(root).
  - **Telemetry Evasion & Meterpreter Session:** The command shell was subsequently upgraded to an encrypted Meterpreter session, verifying that an attacker could stage post-exploitation toolkits, inspect active TCP sockets, and manipulate filesystem artifacts.
- **Business Risk:** **Critical.** Complete loss of confidentiality, integrity, and availability on the target host. Because this server supports operational manufacturing workflows, unauthorized code execution could halt operations, incurring estimated losses of **,000 per hour per server**.
- **Strategic Remediation:**
  1. *Immediate:* Remove the username map script directive from /etc/samba/smb.conf and restart the daemon.
  2. *Defensive:* Restrict TCP ports 139 and 445 at host and perimeter firewall boundaries to authorized management subnets only.
  3. *Long-Term:* Upgrade Samba to an actively maintained, patched enterprise release.

---

### Finding EXT-02: vsftpd 2.3.4 Malicious Backdoor Command Execution (CVE-2011-2523)
- **Vulnerability Identified:** An unpatched FTP daemon (sftpd 2.3.4) running on TCP Port 21 was identified. This specific release contains a malicious backdoor introduced into the upstream source archive, designed to spawn a listening root shell on TCP Port 6200 upon receiving a username ending with a smiley face sequence (:)).
- **Verified Impact & Access Level Achieved:**
  - **Instant Shell Acquisition:** Triggering the authentication condition reliably opened the secondary port (6200/tcp), granting immediate root-level command execution.
  - **Comparative Daemon Analysis:** Analysis between sftpd and alternative FTP modules (such as ProFTPD mod_copy) demonstrated that while vsftpd provides immediate privilege, its predictable port listener (:6200) generates detectable telemetry compared to stealthier payload injection vectors.
- **Business Risk:** **Critical.** Legacy software distributions containing known historical trojans present an open gateway for total server compromise with zero exploitation complexity.
- **Strategic Remediation:**
  1. *Immediate:* Immediately terminate and decommission the sftpd 2.3.4 service.
  2. *Modernization:* Transition all file transfer requirements to secure, encrypted protocols such as SFTP (SSH File Transfer Protocol) with key-based authentication.
  3. *Supply Chain Verification:* Implement cryptographic hash validation for all software binaries deployed within production environments.

---

### Finding EXT-03: Windows Endpoint Compromise, Defensive Blindness & Persistence
- **Vulnerability Identified:** The Windows 11 enterprise endpoint was susceptible to offline credential recovery due to insufficient password entropy. Furthermore, Remote Desktop Protocol (RDP / TCP 3389) was exposed without Network Level Authentication (NLA) enforcement or multi-factor authentication (MFA).
- **Verified Impact & Access Level Achieved:**
  - **Interactive Graphical Takeover:** Exploiting recovered credentials, the assessment team successfully connected to the host via 
desktop, establishing an active graphical desktop session.
  - **Circumvention of Defensive Controls:** Operating under administrative privilege, the team demonstrated how an adversary can blind defenders by using PowerShell commands and local control panels to disable Windows Defender Real-Time Protection, disable Windows Defender Firewall, and suppress automatic system updates.
  - **Administrative Persistence:** Created unauthorized local backdoor accounts (
et user [backdoor_user] [password] /add) and elevated them to the local Administrators security group (
et localgroup administrators [backdoor_user] /add). Verified full token privileges via whoami /priv.
- **Business Risk:** **High.** An adversary gaining endpoint administrative access can blind endpoint security visibility, stage ransomware or lateral movement toolsets, and maintain permanent persistent access even after transient connections are terminated.
- **Strategic Remediation:**
  1. *Access Control:* Enforce mandatory Multi-Factor Authentication (MFA) across all Remote Desktop sessions and restrict RDP access exclusively to dedicated management jump boxes.
  2. *Endpoint Protection (EDR):* Deploy enterprise-grade Endpoint Detection and Response (EDR) with tamper-protection enabled, preventing local administrators from disabling antivirus services or security logging.
  3. *Privilege Governance:* Enforce least-privilege access, removing local administrative rights from standard user accounts via centralized Active Directory Group Policy.
