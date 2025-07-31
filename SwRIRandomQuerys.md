Random KQL Querys used by SwRI

mac.bid website visiting

agent.type: "packetbeat" and 
dns.question.name: "www.mac.bid" and 
dns.answers.name: "www.mac.bid"


SO Meeting 7/31/25

Key points:

Elastic Troubleshooting

attempt to reinstal elastic agents on systems, faield due to endpoint permissions not allowing
used cmd promtp to forceibly uninstall, fixed port 8220, bypassed certificate errors
managed to enroll agents on elastic fleet

 MITRE ATT&CK KQLs
 
Built KQL queries in Kibana based on MITRE techniques.
Verified results in Hunt interface to ensure valid threat detection.
Set the foundation for alerts and dashboards

Alert Dashboard

Created a custom Kibana dashboard showing alerts from MITRE and custom KQL rules.
Includes filters, charts, and timelines for quick triage.
Helps spot issues like failed logins or unusual activity fast.

demo kql

user failed logon (more decriptive)

event.code: "4625" and event.action : "logon-failed" and event.outcome : "failure"  and winlog.api : "wineventlog" 

