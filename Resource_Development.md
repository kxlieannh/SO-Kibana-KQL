# Resource Development - TA0042

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
