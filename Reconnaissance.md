# Reconnaissance TA0043
### MITRE ID	|Technique Name	| Purpose
T1595 |	Active Scanning	Actively probe systems to find open ports/services

T1590	| Gather Victim Network Info	Discover IPs, ranges, DNS, etc.

T1592	| Gather Victim Host Info	Discover OS, software, users

T1596 |	Search Open Websites	Recon via forums, job sites, GitHub

T1597	| Search Engines	Use Google, Bing, etc. to find public info

## T1595 – Active Scanning

Attacker scans for open ports or services to find a way in (e.g., Nmap, Masscan).

### detect tools or mass connections

`process.name: ("nmap.exe" or "masscan.exe") or process.command_line: ("*nmap*" or "*masscan*")`

### detect port scan pattern - many connections in short time (Use in Lens or Watcher — this identifies fast scans from one IP to many ports.)

`destination.port: * 
| stats count() by source.ip, destination.port 
| where count_ > 20`

## T1590 – Gather Victim Network Information

Attacker tries to learn about internal IP ranges, DNS servers, or subnets.

### detect ipconfig, nslookup, netstat, route

These commands give the attacker:

-Host IP addresses
-DNS settings
-Routing table
-Local port connections

`process.command_line: ("*ipconfig*" or "*netstat*" or "*nslookup*" or "*route print*")`

## T1596 – Search Open Websites / Forums
