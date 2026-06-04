Home SOC Lab – Wazuh & Sysmon Threat Detection
Overview

This project demonstrates the design, deployment, and operation of a Home Security Operations Center (SOC) lab built using Wazuh and Sysmon.

The objective was to simulate attacker behavior on a Windows endpoint, collect telemetry through Sysmon, forward logs to Wazuh, and perform threat hunting activities mapped to the MITRE ATT&CK framework.

Lab Architecture

Components
-Wazuh Server
-Windows 11 Endpoint
-Wazuh Agent
-Sysmon
-Kali Linux
-VirtualBox

Log Flow

Windows Endpoint → Sysmon → Wazuh Agent → Wazuh Server

Detection Scenarios

The following attack simulations were performed and investigated:

PowerShell Activity Detection

-PowerShell process execution
-Parent-child process analysis
-PowerShell spawning cmd.exe

Encoded PowerShell Detection

-Base64 encoded PowerShell commands
-Suspicious PowerShell execution chains

File Drop Detection

-Files written to suspicious locations
-Public folder abuse
-Temporary directory execution

Registry Persistence Detection

-Registry Run Key creation
-Registry value modification
-Persistence validation

Scheduled Task Detection

-Task creation using schtasks.exe
-Scheduled task persistence
-Task execution monitoring

LOLBins Detection

-Abuse of native Windows binaries
-Command execution through trusted processes
-Living Off The Land techniques

Threat Hunting Investigation

-Alert triage
-Event correlation
-Process lineage analysis
-MITRE ATT&CK mapping

MITRE ATT&CK Techniques Observed

Technique ATT&CK ID
PowerShell T1059.001
Windows Command Shell T1059.003
Registry Run Keys / Startup Folder T1547.001
Scheduled Tasks T1053.005
Ingress Tool Transfer T1105
Application Shimming T1546.011
Windows Services T1543.003

Skills Demonstrated

-Threat Hunting
-SIEM Operations
-Wazuh Administration
-Sysmon Analysis
-Windows Event Analysis
-Detection Engineering
-Incident Investigation
-MITRE ATT&CK Mapping
-Security Monitoring
-Endpoint Visibility

Screenshots

Screenshots of detections, alerts, investigations, and dashboards are available in the screenshots directory.

Tools Used

-Wazuh
-Sysmon
-Windows 11
-Kali Linux
-PowerShell
-VirtualBox

Outcome

Successfully deployed a functional Home SOC environment capable of detecting and investigating multiple attacker techniques using Sysmon telemetry and Wazuh threat detection capabilities.
