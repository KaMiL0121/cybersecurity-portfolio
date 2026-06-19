# Attack Scenarios

## Scenario 1 – PowerShell Activity Detection

### Objective

Detect PowerShell execution and parent-child process relationships.

### Commands Executed

```powershell
powershell.exe
cmd.exe
```

### Expected Detection

- PowerShell execution
- PowerShell spawning cmd.exe
- Parent-child process visibility

### Wazuh Alert

**Rule ID:** 92004

**Description:**
PowerShell process spawned Windows command shell instance.

### MITRE ATT&CK

- T1059.001 – PowerShell
- T1059.003 – Windows Command Shell

### Result

Successfully detected and investigated using Sysmon process creation events.

---

## Scenario 2 – Encoded PowerShell Detection

### Objective

Detect Base64-encoded PowerShell execution.

### Commands Executed

```powershell
powershell.exe -EncodedCommand SQBFAFgA...
```

### Expected Detection

- Encoded PowerShell execution
- Suspicious execution chain

### Wazuh Alert

**Rule ID:** 92057

**Description:**
PowerShell.exe spawned a PowerShell process which executed a Base64 encoded command.

### MITRE ATT&CK

- T1059.001 – PowerShell

### Result

Successfully detected.

---

## Scenario 3 – File Drop Detection

### Objective

Detect files written to locations commonly abused by malware.

### Commands Executed

```powershell
echo test > C:\Users\Public\soclab.txt
```

### Expected Detection

- File creation in suspicious location
- Malware staging simulation

### Wazuh Alert

**Rule ID:** 92213

**Description:**
Executable file dropped in folder commonly used by malware.

### MITRE ATT&CK

- T1105 – Ingress Tool Transfer

### Result

Successfully detected.

---

## Scenario 4 – Registry Persistence Detection

### Objective

Detect Registry Run Key persistence.

### Commands Executed

```cmd
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run /v SOCLab /t REG_SZ /d C:\Users\Public\soclab.txt /f
```

### Expected Detection

- Registry modification
- Persistence mechanism creation

### Wazuh Alert

**Rule ID:** 92302

**Description:**
Registry entry to be executed on next logon was modified using command line application reg.exe.

### MITRE ATT&CK

- T1547.001 – Registry Run Keys / Startup Folder

### Result

Successfully detected.

---

## Scenario 5 – Scheduled Task Persistence

### Objective

Detect persistence using Windows Scheduled Tasks.

### Commands Executed

```cmd
schtasks.exe /create /tn SOCLabTask /tr C:\Windows\System32\cmd.exe /sc onlogon /f
```

### Expected Detection

- Scheduled task creation
- Persistence activity

### Wazuh Alert

**Rule ID:** 92004

**Description:**
PowerShell process spawned Windows command shell instance.

### MITRE ATT&CK

- T1053.005 – Scheduled Tasks

### Result

Successfully detected and correlated through Sysmon telemetry.

---

## Scenario 6 – LOLBins Detection

### Objective

Detect abuse of trusted Windows binaries.

### Binaries Used

- powershell.exe
- cmd.exe
- reg.exe
- schtasks.exe

### Expected Detection

- Suspicious execution chains
- Trusted binary abuse
- Parent-child process visibility

### Wazuh Detection

Execution activity was recorded and correlated using Sysmon process creation events.

### MITRE ATT&CK

- T1218 – Signed Binary Proxy Execution

### Result

Successfully investigated through threat hunting activities.

---

## Scenario 7 – Threat Hunting Investigation

### Objective

Correlate alerts generated during the attack simulation.

### Activities Performed

- Alert triage
- Event correlation
- Parent-child process analysis
- MITRE ATT&CK mapping

### Findings

The Home SOC Lab successfully detected:

- PowerShell execution
- Encoded PowerShell commands
- File drop activity
- Registry persistence
- Scheduled task persistence
- LOLBins usage

### Outcome

All attack scenarios generated telemetry visible in Sysmon and alerts visible in Wazuh, enabling effective threat hunting and investigation workflows.
