# Module 02: Isolated Lab Infrastructure & Network Baseline

[![Module](https://img.shields.io/badge/Module-02%20Homelab%20Infrastructure-blue.svg)](#)
[![Virtualization](https://img.shields.io/badge/Hypervisor-VirtualBox%207.x-orange.svg)](#)
[![Isolation](https://img.shields.io/badge/Network-Host--Only%20Strict%20Air--Gap-green.svg)](#)

---

## 1. Executive Summary
This project outlines the engineering architecture, network configuration, and baseline verification for an isolated, multi-node ethical hacking and defensive telemetry laboratory. Built on Oracle VM VirtualBox, the environment models a small enterprise network while maintaining strict Layer 2 broadcast boundaries to ensure that aggressive vulnerability scanning, exploitation payloads, and credential interception remain completely contained without leaking to physical host interfaces or external networks.

---

## 2. Infrastructure Architecture & Network Topology

`mermaid
graph TB
    subgraph Physical_Host [Physical Host Workstation]
        VBox_HostAdapter[VirtualBox Host-Only Ethernet Adapter<br>Subnet: 192.168.56.1 / 24<br>Promiscuous Mode: Allow VMs | Egress Routing: Blocked]
    end

    subgraph Isolated_Lab_VLAN [Isolated Host-Only Broadcast Domain: 192.168.56.0/24]
        Kali[Attacker Node: Kali Linux 2026.x<br>IP: 192.168.56.101<br>Tools: Nmap, Metasploit, Responder, Wireshark]
        Target_Linux[Legacy Target: Metasploitable 2<br>IP: 192.168.56.102<br>Kernel 2.6.24 | Legacy Services: Samba, vsftpd, Apache]
        Target_Win[Endpoint Target: Windows 11 Enterprise<br>IP: 192.168.56.104<br>Role: Domain Endpoint, GPO & RDP Target]
    end

    VBox_HostAdapter --- Kali
    VBox_HostAdapter --- Target_Linux
    VBox_HostAdapter --- Target_Win
`

### Node Specifications & Allocation Matrix
| Node Role | Operating System | IP Address | MAC Address Baseline | vCPU / RAM | Functional Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Auditing & Attack Platform** | Kali Linux (Debian 64-bit) | 192.168.56.101 | Dynamic / VirtualBox | 2 vCPU / 4 GB | Primary security assessment, exploit staging, packet capture. |
| **Legacy Target Server** | Ubuntu Linux 8.04 LTS (Metasploitable 2) | 192.168.56.102 | Dynamic / VirtualBox | 1 vCPU / 1 GB | Simulation of unpatched, mission-critical legacy enterprise servers. |
| **Modern Endpoint Workstation** | Windows 11 Enterprise | 192.168.56.104 | Dynamic / VirtualBox | 2 vCPU / 4 GB | Simulation of modern enterprise client endpoint, GPO testing, and credential defense. |

---

## 3. Network Isolation & Security Boundary Verification
To guarantee complete isolation from the host's physical network:
1. **Dedicated Host-Only Network Adapter:** Configured VirtualBox DHCP server on 192.168.56.100 with address scope 192.168.56.101 – 192.168.56.254 on subnet mask 255.255.255.0.
2. **Elimination of Default Gateway Egress:** Target machines were provisioned without external gateway routing, preventing any unintentional reverse-shell traffic or broadcast artifacts from egressing to the physical LAN or Internet.
3. **Promiscuous Mode Isolation:** The virtual switch interface (boxnet0 / VirtualBox Host-Only Adapter) was restricted to intra-VM communication, blocking packet leakage to physical NICs.

---

## 4. Operating System Fingerprinting & TTL Analysis
Packet behaviors were analyzed using ICMP echo requests and TCP handshakes to establish baseline protocol responses:

| Metric | Target: Linux Node (192.168.56.102) | Target: Windows Node (192.168.56.104) | Technical Rationale |
| :--- | :--- | :--- | :--- |
| **Standard TTL** | 64 | 128 | Default IP packet Time-To-Live defined by Linux kernel vs. Microsoft TCP/IP stack. |
| **ICMP Ping Response** | Immediate Reply (	tl=64) | Dropped when Firewall Active; Reply (	tl=128) when Disabled | Windows Defender Firewall silently drops inbound ICMPv4 Type 8 requests by default. |
| **Closed Port Response** | Returns TCP RST/ACK | Silent Drop (iltered) with Firewall Active; TCP RST with Firewall Disabled | Confirms stateful packet inspection behavior on the Windows endpoint. |

---

## 5. Host Firewall Behavioral Analysis (Windows Defender Firewall)
During active reconnaissance drills using Nmap, quantitative behavioral differences were documented between active and disabled firewall states on the Windows 11 target:

`
[State 1: Windows Defender Firewall ACTIVE]
- Standard Nmap SYN Scan: All 1,000 top ports reported as 'filtered'.
- ICMP Ping: Host appears down unless probed with '-Pn' (treat all hosts as online).
- OS Fingerprinting (-O): Failed to generate definitive OS fingerprint due to absence of TCP RST/ACK and ICMP responses ('Too many fingerprints match this host').

[State 2: Windows Defender Firewall DISABLED]
- Standard Nmap SYN Scan: Accurately identifies listening services (e.g., TCP 135/rpc, 445/smb, 3389/rdp).
- ICMP Ping: Host immediately acknowledges ping sweeps.
- OS Fingerprinting (-O): Accurately detects 'Microsoft Windows 11 / Windows Server 2022' with high confidence based on TCP window size, MSS, and TTL metrics.
`

---

## 6. Operational Maintenance & Reversibility
- **Golden Image Snapshots:** Pre-exploitation snapshots were captured for all three virtual machines immediately following OS installation and baseline configuration.
- **Rapid Reversion Playbook:** Following disruptive exploit executions (e.g., privilege escalation, persistence mechanisms, or service disruptions), targets are reverted to their baseline snapshot within seconds, ensuring repeatable, scientifically rigorous testing conditions.
