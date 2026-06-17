# CyberDefenders – Lockdown

## Overview

Lockdown is a Blue Team investigation focused on analyzing a compromised IIS web server using multiple forensic data sources.

The challenge combines:

- Network Forensics (PCAP Analysis)
- Memory Forensics (Volatility 3)
- Malware Analysis
- Threat Intelligence

The objective was to reconstruct the attack chain, identify persistence mechanisms, recover malware artifacts, and determine the adversary's command-and-control infrastructure.

---

# Scenario

An IIS web server was suspected of being compromised after unusual network activity was detected.

Available evidence:

- Packet Capture (PCAP)
- Memory Dump
- Malware Sample

The investigation focused on determining:

- Initial attacker activity
- Exploitation method
- Malware deployment
- Persistence mechanism
- Command and Control infrastructure
- Malware family attribution

---

# Tools Used

| Tool            | Purpose                   |
| --------------- | ------------------------- |
| Wireshark       | PCAP Analysis             |
| Volatility 3    | Memory Forensics          |
| VirusTotal      | Threat Intelligence       |
| FLOSS / Strings | Malware String Analysis   |
| Kali Linux      | Investigation Environment |

---

# Investigation Methodology

The investigation followed a standard incident response workflow:

1. Analyze network traffic for reconnaissance and exploitation activity.
2. Identify attacker infrastructure.
3. Examine SMB activity and uploaded files.
4. Determine reverse shell communications.
5. Analyze memory artifacts.
6. Identify malicious processes and persistence mechanisms.
7. Investigate malware behavior.
8. Attribute the malware family through threat intelligence.

---

# Phase 1 – Network Forensics

## Reconnaissance Activity

Reviewing HTTP traffic revealed a host performing extensive reconnaissance against the IIS server.

Filter used:

```text
ip.src == 10.0.2.4 && http.request
```

Evidence showed:

- Numerous HTTP requests
- Enumeration behavior
- Nmap User-Agent string

### Finding

Attacker IP:

```text
10.0.2.4
```

Tool identified:

```text
Nmap
```

---

## SMB Enumeration

SMB traffic was reviewed to identify accessed network shares.

Filter:

```text
smb2.cmd == 3
```

Observed shares:

```text
\\10.0.2.15\IPC$
```

```text
\\10.0.2.15\Documents
```

These shares exposed sensitive server resources and enabled further attacker interaction.

---

## Web Shell Upload

HTTP requests revealed access to a suspicious ASPX file.

Filter:

```text
http contains "shell.aspx"
```

Observed request:

```text
GET /Documents/shell.aspx
```

### Finding

Malicious file:

```text
shell.aspx
```

The file functioned as a web shell providing remote code execution capability.

---

## Reverse Shell Activity

Following web shell execution, network traffic showed a reverse shell connection.

Filter:

```text
tcp.port == 4443
```

Observed bidirectional communication between:

```text
10.0.2.15
```

and

```text
10.0.2.4
```

### Finding

Reverse shell port:

```text
4443
```

---

# Phase 2 – Memory Forensics

Memory analysis was performed using Volatility 3.

---

## System Identification

Plugin:

```bash
vol -f memdump.mem windows.info
```

### Finding

Kernel Base:

```text
0xf80079213000
```

This confirmed successful profile detection and provided baseline system information.

---

## Process Tree Analysis

Plugin:

```bash
vol -f memdump.mem windows.pstree
```

The process tree revealed an unusual parent-child relationship.

Observed:

```text
w3wp.exe
└── updatenow.exe
```

### Finding

Parent Process:

```text
w3wp.exe
```

PID:

```text
4332
```

This strongly indicated code execution originating from the IIS worker process.

---

## Persistence Discovery

Plugin:

```bash
vol -f memdump.mem windows.cmdline
```

Suspicious command line:

```text
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\updatenow.exe
```

### Finding

Persistence Path:

```text
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\updatenow.exe
```

The malware established persistence using the Windows Startup folder.

MITRE ATT&CK:

```text
T1547.001
Boot or Logon Autostart Execution
```

---

# Phase 3 – Malware Analysis

## Malware Sample

Recovered executable:

```text
updatenow.exe
```

Hash:

```text
91962e35ba555e81c829a6784a73915d392361fc5e9f98807cebdc23bdbce6d3
```

---

## Packer Identification

Static analysis revealed the executable had been packed to hinder analysis.

### Finding

Packer:

```text
UPX
```

MITRE ATT&CK:

```text
T1027
Obfuscated Files or Information
```

---

## Command and Control Infrastructure

Threat intelligence analysis linked the malware to an external domain.

### Finding

C2 Domain:

```text
cp8nl.hyperhost.ua
```

This domain was observed within malware behavioral analysis results.

MITRE ATT&CK:

```text
T1071
Application Layer Protocol
```

---

## Malware Attribution

VirusTotal and threat intelligence correlation associated the sample with a known commodity malware family.

### Finding

Malware Family:

```text
AgentTesla
```

AgentTesla is a credential-stealing Remote Access Trojan (RAT) commonly used to:

- Steal browser credentials
- Capture keystrokes
- Exfiltrate system information
- Communicate with external C2 infrastructure

---

# Indicators of Compromise (IOCs)

## Network

| Type               | Value              |
| ------------------ | ------------------ |
| Attacker IP        | 10.0.2.4           |
| Reverse Shell Port | 4443               |
| C2 Domain          | cp8nl.hyperhost.ua |

---

## Files

| Type      | Value         |
| --------- | ------------- |
| Web Shell | shell.aspx    |
| Malware   | updatenow.exe |

---

## Hashes

| Type   | Value                                                            |
| ------ | ---------------------------------------------------------------- |
| SHA256 | 91962e35ba555e81c829a6784a73915d392361fc5e9f98807cebdc23bdbce6d3 |

---

# MITRE ATT&CK Mapping

| Technique                         | ID        |
| --------------------------------- | --------- |
| Active Scanning                   | T1595     |
| Exploit Public-Facing Application | T1190     |
| Web Shell                         | T1505.003 |
| Command and Scripting Interpreter | T1059     |
| Application Layer Protocol        | T1071     |
| Obfuscated Files or Information   | T1027     |
| Boot or Logon Autostart Execution | T1547.001 |

---

# Evidence

## Network Analysis

- 01-attacker-reconnaissance.png
- 02-smb-share-enumeration.png
- 03-web-shell-access.png
- 04-reverse-shell-port-4443.png

## Memory Forensics

- 05-volatility-system-info.png
- 06-volatility-pstree-updatenow.png
- 07-persistence-path.png

## Threat Intelligence

- 08-c2-domain.png

---

# Conclusion

The investigation revealed a complete attack chain beginning with reconnaissance activity from an external attacker, followed by SMB share enumeration, web shell deployment, reverse shell access, malware execution, persistence establishment, and communication with external command-and-control infrastructure.

Memory analysis confirmed that the IIS worker process (`w3wp.exe`) launched the malicious payload (`updatenow.exe`), which established persistence via the Windows Startup folder.

Threat intelligence correlated the malware sample with the AgentTesla family and identified the associated command-and-control domain.

This investigation demonstrates the value of combining network forensics, memory analysis, malware analysis, and threat intelligence to reconstruct attacker activity and identify persistence mechanisms within a compromised environment.

---

**Platform:** CyberDefenders

**Challenge:** Lockdown

**Category:** Network Forensics / Memory Forensics / Malware Analysis

**Difficulty:** Easy

**Analyst:** Kamil Połubiński
