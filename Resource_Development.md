# Resource Development - TA0042

The adversary is setting up infrastructure, capabilities, or access they will use during operations.

## T1583.001 – Acquire Infrastructure: Domains

Use Case: Registering domains for phishing or command-and-control.

`dns.question.name : "*"
| where dns.question.name matches regex ".*\.(tk|ml|cf|gq|ga)$"`

## T1583.002 – Acquire Infrastructure: Server

Renting VPS/cloud for staging or attack infrastructure.

`destination.ip : * 
| where destination.ip in (threat_intel_cloud_ips)`




