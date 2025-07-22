# Resource Development - TA0042

Tactic|  	ID	   | Purpose

Resource Development | 	TA0042	| Prepare tools, infra, accounts, identities for later use

Acquire Infrastructure | 	T1583	| Buy/rent infrastructure (domains, servers, certs)

Compromise Infrastructure	| T1584	| Hijack servers or domains instead of buying

Establish Accounts	| T1585	| Create email or social media accounts

Develop Capabilities	| T1586	| Create tools like malware and certs

Develop Capabilities (Sub)	| T1587	| Build obfuscation, exploits, payloads

Obtain Capabilities	| T1588	| Download malware, tools, certs

Gather Victim Identity	| T1589	| Learn usernames and emails of target

## T1583 – Acquire Infrastructure

The adversary is setting up infrastructure, capabilities, or access they will use during operations.

### T1583.001 – Acquire Infrastructure: Domains

Registering domains for phishing or command-and-control.

`dns.question.name : "*"
| where dns.question.name matches regex ".*\.(tk|ml|cf|gq|ga)$"`

### T1583.002 – Acquire Infrastructure: Server

Renting VPS/cloud for staging or attack infrastructure.

`destination.ip : * 
| where destination.ip in (threat_intel_cloud_ips)`

###  T1583.003 – Acquire Infrastructure: Digital Certificates

Acquire or self-sign TLS certs to add legitimacy to phishing/C2 sites.

`event.dataset : "x509" | where not x509.issuer.common_name in ("DigiCert", "GlobalSign")`

## T1584 – Compromise Infrastructure

Instead of buying resources, adversaries compromise existing infrastructure.

### T1584.001 – Compromise Infrastructure: Domains

Hijacked domains are used for phishing or malware delivery.

 `dns.question.name : ("cdn.realcompany.com", "mail.spoofeddomain.org") | where url.path : "*login*" or "*update*"`

### T1584.002 – Compromise Infrastructure: Servers

Use of breached systems to serve payloads or proxy C2.

`destination.ip in (known_compromised_webservers) | where network.protocol : "http" or "https"`

## T1585 – Establish Accounts

Create or obtain accounts (email, social media) to impersonate, communicate, or host.

 ### T1585.001 – Establish Accounts: Social Media

 Fake accounts to impersonate brands or personas.

 `url.full : "*facebook.com*" or "*twitter.com*" or "*linkedin.com*"
| where user_agent.original : "*python*" or "*curl*" or "*bot*"`

###  T1585.002 – Establish Accounts: Email Accounts

 Burner email accounts for registration or phishing.

`url.domain : ("protonmail.com", "tutanota.com", "mail.ru")
| where url.path : "*signup*" or "*register*"`

## T1586 – Develop Capabilities

Develop malware, exploits, or certs internally.

### T1586.001 – Develop Capabilities: Malware

Compile custom malware tools.

`process.name : ("cl.exe", "msbuild.exe", "pyinstaller.exe")
| where process.command_line : ("*.dll", "*.exe", "*.py")`

### T1586.002 – Develop Capabilities: Code Signing Certificates

Sign malware to appear legitimate.

`process.name : "signtool.exe"
| where process.command_line : ("*.exe", "*.dll")`

## T1587 – Develop Capabilities: Payloads, Obfuscation, Exploits

Create or customize malware/tools that will be used during execution.

### T1587.001 – Develop Capabilities: Malware

Create or obfuscate payloads using PowerShell.

`process.command_line : ("*IEX*", "*base64*", "*Invoke-Obfuscation*")
| where process.name : "powershell.exe"`

### T1587.002 – Develop Capabilities: Exploits

Create or test exploit code using frameworks.

`process.name : ("msfconsole", "searchsploit")`

T1587.003 – Develop Capabilities: Payloads

Create or host EXE/DLL payloads.

`process.name : ("curl.exe", "wget.exe", "certutil.exe")
| where process.command_line : ("*.exe", "*.dll", "*.ps1")`

T1587.004 – Develop Capabilities: Drive-by Content

Host malicious web content used in future campaigns.

`url.path : ("*.html", "*.php", "*.js")
| where user_agent.original : "*Windows*" and http.response.status_code == 200`

## T1588 – Obtain Capabilities

Download or purchase tools, malware, or exploits from external sources.

### T1588.001 – Obtain Capabilities: Malware
 
Download prebuilt malware.

`url.full : "*anonfiles.com*" or "*mega.nz*"
| where url.path : ("*.zip", "*.exe", "*.dll")`

### T1588.002 – Obtain Capabilities: Tool

Download public tools like Mimikatz, Rclone, etc.

`url.full : "*github.com*" 
| where url.path : ("/mimikatz", "/rclone", "/cobaltstrike")`

T1588.003 – Obtain Capabilities: Code Signing Certificates

Request legitimate certificates for signing.

process.name : "certreq.exe"

T1588.004 – Obtain Capabilities: Exploits

Retrieve working exploit code.

process.command_line : ("searchsploit", "exploit-db")

## T1589 – Gather Victim Identity Information

Learn about usernames, email addresses, or credentials of the victim environment.

### T1589.001 – Gather: Credentials / Accounts

Identify users for phishing or impersonation.

`process.command_line : ("whoami", "net user", "Get-ADUser")
| where process.name : ("powershell.exe", "cmd.exe")`
