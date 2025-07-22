# Execution – TA0002

How adversaries run malicious code on a local or remote system after gaining access. It’s a critical step before persistence, privilege escalation, or impact.


Technique	|  Use Case  |	Key KQL Indicator

T1059 |	Script interpreters  |	PowerShell or cmd usage with suspicious flags

T1203	| Exploits	|  Parent-child mismatch like chrome → cmd
 
T1053	| Scheduled tasks	|  schtasks.exe /create

T1047	| WMI exec  | wmic spawning commands

T1106	| API abuse	|  rundll32.exe loading suspicious DLL

T1569	| Malicious services	|  sc.exe create/start

T1021	| Remote execution	|  Admin share or psexec

T1129	| Shared modules	| DLLs running from temp/user folders

T1072	| IT tools used for evil	|  PDQ/SCCM deployment scripts

T1204	| Trick the user	|  LNK/VBS files from Explorer/Outlook

T1559	| Inter-process triggers	|  COM hijack via registry


## T1059 – Command and Scripting Interpreter

Execution using shells, scripting languages (PowerShell, Bash, CMD, Python).

Sub-techniques include:
T1059.001 – PowerShell
T1059.003 – Windows Command Shell
T1059.005 – Visual Basic
T1059.006 – Python

`process.name : ("powershell.exe", "cmd.exe", "pwsh.exe", "cscript.exe", "wscript.exe", "python.exe")`

With Suspicious Argument

`process.name: "powershell.exe"
| where process.command_line : ("*Invoke-WebRequest*", "*IEX*", "*Base64*", "*DownloadString*")`

Purpose: Detect script interpreters being used for lateral movement or code execution.

## T1203 – Exploitation for Client Execution

Adversary exploits a vulnerability in a user-facing app (e.g., browser, PDF reader).

`event.category: "malware" and file.name : ("acrobat.exe", "chrome.exe", "firefox.exe")`

OR: Detect Exploit Loads via Sysmon:

`process.parent.name : ("chrome.exe", "outlook.exe") and process.name : ("cmd.exe", "powershell.exe", "rundll32.exe")`

## T1053 – Scheduled Task/Job

Creating a scheduled task or cron job to run malicious payloads.

`process.name: "schtasks.exe"
| where process.command_line : ("*/create*", "* /sc *")`

Detects creation of Windows scheduled tasks.

## T1047 – Windows Management Instrumentation (WMI)

Execution via WMI using wmic.exe or WmiPrvSE.exe.

`process.name : ("wmic.exe", "WmiPrvSE.exe")
| where process.command_line : ("*process call create*", "*select * from win32*")`

## T1106 – Native API

Execution using direct calls to Windows APIs (via C++, DLL injection, etc.).

`process.name: "rundll32.exe"
| where process.command_line : ("*.dll*", "*#*", "*.dll*,*")`

## T1569 – System Services

Use of services.exe or sc.exe to execute code by starting a malicious service.

`process.name : "sc.exe"
| where process.command_line : ("*create*", "*start*", "*binPath*")`

### T1021 – Remote Services (Used to Execute Code on Remote Hosts)

### T1021.001 – Remote Desktop Protocol

### T1021.002 – SMB/Windows Admin Shares

### T1021.004 – SSH


`process.name: "psexec.exe" or process.command_line : "*\\\\*\\admin$*"`

## T1129 – Shared Modules

Loading code via DLLs already present in shared folders or memory.

`process.name : ("rundll32.exe", "dllhost.exe") | where file.path : ("C:\\Users\\*", "C:\\ProgramData\\*", "C:\\Temp\\*")`

## T1072 – Software Deployment Tools

Using SCCM, PDQ Deploy, or other tools to push and execute software.

`process.name : ("PDQDeploy.exe", "PDQInventory.exe") | where process.command_line : ("*install*", "*deploy*")`

## T1204 – User Execution

User is tricked into executing malicious code (e.g., clicking .lnk, .vbs, .exe in phishing email).

### T1204.002 – Malicious File Execution

`file.extension : ("exe", "lnk", "vbs", "js", "scr") | where process.parent.name : ("explorer.exe", "outlook.exe")`

## T1559 – Inter-Process Communication

Adversaries use COM, named pipes, or Windows Messaging to execute code between processes.

`registry.path : "*\\CLSID\\*" | where registry.data.strings : ("*mshta.exe*", "*cmd.exe*", "*powershell*")`


