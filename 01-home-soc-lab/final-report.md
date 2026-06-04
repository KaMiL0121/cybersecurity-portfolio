Final Threat Hunting Report

Executive Summary

A Home SOC Lab was built using Wazuh, Sysmon, Windows 11, and Kali Linux to simulate and investigate common attacker techniques.

Multiple attack scenarios were executed against the monitored Windows endpoint. Sysmon collected detailed endpoint telemetry while Wazuh generated alerts and mapped activity to the MITRE ATT&CK framework.

The investigation confirmed successful detection of execution, persistence, and file transfer techniques commonly observed during real-world attacks.

Environment

Component Purpose
Wazuh Server SIEM and Alerting
Windows 11 Endpoint Monitored Host
Sysmon Endpoint Telemetry
Wazuh Agent Log Collection
Kali Linux Attack Simulation
VirtualBox Virtualization

Investigation Scenarios

Scenario 1 – PowerShell Activity

Activity

PowerShell was executed and used to spawn cmd.exe.

Detection

Wazuh generated alerts:

-Rule 92004
-PowerShell process spawned Windows command shell instance

MITRE ATT&CK

-T1059.001 – PowerShell
-T1059.003 – Windows Command Shell

Result

Successfully detected.

Scenario 2 – Encoded PowerShell

Activity

A Base64 encoded PowerShell command was executed.

Detection

Wazuh generated alert:

-Rule 92057
-PowerShell executed Base64 encoded command

MITRE ATT&CK

-T1059.001 – PowerShell

Result

Successfully detected.

Scenario 3 – File Drop Detection

Activity

Files were created in locations commonly abused by malware.

Detection

Wazuh generated alert:

-Rule 92213
-Executable file dropped in folder commonly used by malware

MITRE ATT&CK

-T1105 – Ingress Tool Transfer

Result

Successfully detected.

Scenario 4 – Registry Persistence

Activity

A Registry Run Key was created using reg.exe.

Detection

Wazuh generated alerts:

-Rule 92302
-Registry entry modified for execution on next logon

MITRE ATT&CK

-T1547.001 – Registry Run Keys / Startup Folder

Result

Successfully detected.

Scenario 5 – Scheduled Task Persistence

Activity

A scheduled task was created using schtasks.exe.

Detection

Wazuh generated Windows and Sysmon alerts related to task creation and execution.

MITRE ATT&CK

-T1053.005 – Scheduled Tasks

Result

Successfully detected.

Scenario 6 – LOLBins Activity

Activity

Native Windows binaries were used to perform administrative and suspicious actions.

Examples:

-powershell.exe
-cmd.exe
-reg.exe
-schtasks.exe

Detection

Wazuh successfully recorded execution activity and process relationships.

MITRE ATT&CK

-T1218 – Signed Binary Proxy Execution

Result

Execution activity and process relationships were successfully captured and investigated using Sysmon telemetry and Wazuh threat hunting capabilities.

MITRE ATT&CK Techniques Observed

Technique ATT&CK ID
PowerShell T1059.001
Windows Command Shell T1059.003
Registry Run Keys / Startup Folder T1547.001
Scheduled Tasks T1053.005
Ingress Tool Transfer T1105
Application Shimming T1546.011
Windows Services T1543.003

Findings

The Home SOC environment successfully detected multiple attacker behaviors including:

-Command execution
-PowerShell abuse
-Registry persistence
-Scheduled task persistence
-File staging activity
-LOLBins usage

The combination of Sysmon and Wazuh provided strong visibility into endpoint activity and enabled effective threat hunting workflows.

Skills Demonstrated

-Threat Hunting
-Security Monitoring
-SIEM Operations
-Wazuh Administration
-Sysmon Analysis
-Windows Event Log Analysis
-Detection Engineering
-Incident Investigation
-Process Tree Analysis
-MITRE ATT&CK Mapping
-Alert Triage
-Endpoint Visibility

Evidence and Screenshots

The following screenshots were collected during the investigation and are available in the screenshots directory:

-wazuh-dashboard-overview.png
-agent-connected.png
-sysmon-alert-powershell.png
-encoded-powershell-detection.png
-file-drop-detection.png
-registry-persistence-detection.png

These screenshots provide evidence of alert generation, threat hunting activities, and MITRE ATT&CK technique mapping within the Home SOC environment.

Lessons Learned

-Sysmon significantly improves Windows visibility.
-Parent-child process analysis is critical during investigations.
-MITRE ATT&CK mapping helps categorize attacker behavior.
-Wazuh provides effective detection capabilities for a Home SOC environment.
-Threat hunting requires correlation across multiple event sources.

Conclusion

The project successfully demonstrated the deployment and operation of a Home SOC Lab capable of detecting and investigating common attacker techniques.

The environment provides practical experience with SIEM operations, endpoint monitoring, threat hunting, detection engineering, and incident investigation workflows used by SOC analysts.
