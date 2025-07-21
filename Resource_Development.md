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

`event.dataset : "x509"
| where not x509.issuer.common_name in ("DigiCert", "GlobalSign")`

## T1584 – Compromise Infrastructure



