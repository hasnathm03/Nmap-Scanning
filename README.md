# Task 1 · Basic Network Scanning with Nmap

## Objective
Perform a network scan on a local virtual machine to identify open ports, running services, and the operating system, then analyze the security implications of the findings.

## What is Nmap?
Nmap (Network Mapper) is a free, open-source tool used to discover hosts and services on a network. It works by sending crafted packets to a target and analyzing the responses to determine which ports are open, what services are running on them, and in some cases what operating system the target is using.

## Why Network Scanning Matters
Network scanning is one of the first steps in both offensive security (penetration testing) and defensive security (vulnerability assessment). It helps identify:
- Unnecessary open ports that increase the attack surface
- Outdated or misconfigured services that could be exploited
- Missing patches, revealed through service version banners
- The overall security posture of a system before deeper testing

Understanding your own network's exposure is the foundation of proactive defense — you can't secure what you don't know is open.

## Ethical Use Guidelines
- Only scan systems you own or have explicit written permission to scan.
- All scans in this task were performed against a local virtual machine (`[VM name/OS here]`) that I own and control.
- No external, production, or third-party systems were scanned.
- Scanning networks without authorization is illegal in most jurisdictions (e.g., under the Computer Fraud and Abuse Act in the US, or equivalent local cybercrime laws) and violates responsible disclosure and professional ethics standards.

## Environment
- **Host/Scanning machine OS:** Ubuntu Linux (kernel 7.0.0-27-generic)
- **Target:** The local host machine itself, scanned over its active Wi-Fi interface
- **Network mode:** Local Wi-Fi network (wlp0s20f3 interface)
- **Target IP:** 192.168.1.11

**Note on target selection:** The task originally called for scanning a
separate VirtualBox VM on the Host-Only network (192.168.56.101), but
that host was not reachable at scan time (VM not powered on / not
attached to the Host-Only adapter). Per the task's allowance to scan
"a local machine or virtual machine," the scan was performed against
the local host machine's own active network interface instead, which
I own and control.

## Installation Steps
```bash
sudo apt update
sudo apt install nmap -y
nmap --version
```
[Insert installation screenshot here / note that Nmap was pre-installed on Kali]

## Scans Performed
| Scan Type | Command | Purpose |
|---|---|---|
| Basic scan | `nmap [target IP]` | Identify open ports on top 1000 common ports |
| Service version scan | `nmap -sV [target IP]` | Identify service names and version numbers |
| OS detection scan | `sudo nmap -O [target IP]` | Fingerprint the target's operating system |

Full raw output is in [`nmap_scan_results.txt`](./nmap_scan_results.txt).

## Summary of Findings
The scan identified 4 open ports on the target: SSH (22, OpenSSH
10.2p1), HTTP (80, Apache 2.4.66), and two remote desktop services -
Microsoft RDP (3389) and xrdp (3390). The two remote desktop ports
are the highest-risk finding, as RDP-class services are frequently
targeted for brute-force attacks and have a history of critical
CVEs. The OS detection scan did not return an exact fingerprint
match but confirmed Linux-based TCP/IP behavior consistent with the
actual host OS. Full technical details and per-port risk analysis
are in `nmap_scan_results.txt`.

## Repository Structure
```
task1-nmap-scan/
├── README.md
├── nmap_scan_results.txt
└── screenshots/
    ├── basic_scan.png
    ├── service_scan.png
    └── os_scan.png
```

## Disclaimer
This project was completed strictly for educational purposes as part of the OASIS InfoByte Cyber Security Internship, using isolated virtual machines under my own control.
