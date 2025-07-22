# Initial Access – TA0001

How adversaries gain entry into a network or system. These are the techniques used to breach the perimeter.

### Technique	ID	|   Name

T1078	Valid Accounts

T1133	External Remote Services	

T1190	Exploit Public-Facing Application

T1200	Hardware Additions	

T1566	Phishing (w/ sub-techniques)	

T1195	Supply Chain Compromise	

T1091	Replication Through Removable Media	

T1189	Drive-by Compromise	

T1199	Trusted Relationship	

T1559	Inter-Process Communication (used in privilege escalation too)

## T1078 – Valid Accounts

Adversary uses stolen or default credentials to access systems.

Looks for successful logons (interactive or RDP) that aren't the local admin—often a sign of lateral movement or access gained with stolen credentials.

`event.code: "4624" and winlog.event_data.LogonType: ("2", "10")
| where winlog.event_data.TargetUserName != "Administrator"`

## T1133 – External Remote Services

 Gaining access via RDP, VPN, Citrix, etc.

 Detects RDP logons. Useful for spotting first external logins if you monitor border hosts.

`event.code: "4624" and winlog.event_data.LogonType == "10"`

## T1190 – Exploit Public-Facing Application

Web server exploit, SQLi, RCE on internet-facing services.

Watch for errors during known exploit paths (webshells, login brute force).

`event.dataset: "http"
| where url.path : ("/admin", "/login", "/phpmyadmin", "/wp-login.php") 
  and http.response.status_code == 500`

##T1200 – Hardware Additions

Using rogue USBs, wireless adapters, or rogue devices.

Detects USB device insertion. Especially useful with Sysmon or WinLogBeat.

device.product : "*USB*" or device.name : "*Removable*"
| where event.action : "connected"

## T1566 – Phishing

Most common initial access method. Sub-techniques focus on method used.







