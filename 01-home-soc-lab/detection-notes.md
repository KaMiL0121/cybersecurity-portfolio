# Detection Notes

## PowerShell Activity Detection

### Objective

Detect suspicious PowerShell activity and parent-child process relationships.

### Test Activity

```powershell
powershell.exe
cmd.exe
```

### Detection

Wazuh generated alerts identifying:

- PowerShell execution
- PowerShell spawning cmd.exe
- Parent-child process relationships

### MITRE ATT&CK

- T1059.001 – PowerShell
- T1059.003 – Windows Command Shell

---

## Encoded PowerShell Detection

### Objective

Detect Base64-encoded PowerShell execution.

### Test Activity

```powershell
powershell.exe -EncodedCommand SQBIA...
```

### Detection

Wazuh detected:

- Encoded PowerShell execution
- Suspicious command execution chain

### MITRE ATT&CK

- T1059.001 – PowerShell

---

## File Drop Detection

### Objective

Detect files written to suspicious locations.

### Test Activity

```text
C:\Users\Public\soclab.txt
```

### Detection

Wazuh generated alerts for:

- Executable or suspicious files placed in Public directories
- Potential malware staging locations

### MITRE ATT&CK

- T1105 – Ingress Tool Transfer

---

## Registry Persistence Detection

### Objective

Detect Registry Run Key persistence.

### Test Activity

```cmd
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

### Detection

Wazuh detected:

- Registry key modification
- Registry persistence attempt

### MITRE ATT&CK

- T1547.001 – Registry Run Keys / Startup Folder

---

## Scheduled Task Detection

### Objective

Detect scheduled task persistence.

### Test Activity

```cmd
schtasks /create
```

### Detection

Wazuh detected:

- Scheduled task creation
- Persistence activity

### MITRE ATT&CK

- T1053.005 – Scheduled Tasks

---

## LOLBins Detection

### Objective

Detect abuse of native Windows binaries.

### Examples

- powershell.exe
- cmd.exe
- reg.exe
- schtasks.exe

### Detection

Wazuh detected suspicious execution chains involving trusted Windows binaries.

### MITRE ATT&CK

- T1218 – Signed Binary Proxy Execution
