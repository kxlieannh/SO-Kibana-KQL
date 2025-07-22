# Elastic Agent Installation Troubleshooting

Resources and documentation to help troubleshoot Elastic Agent installation issues, especially for standalone setups (such as in Security Onion or a custom deployment).

## Official Elastic Resources

### Elastic Agent Installation Docs

Use for: Step-by-step installation instructions (standalone, Fleet-managed, or service-based), supported OS versions, permissions.

 https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html

### Troubleshooting Elastic Agent

Use for: Common errors, agent not showing up in Fleet, logs not shipping, service not starting, etc.

https://www.elastic.co/guide/en/fleet/current/troubleshooting-agent.html

### Elastic Agent Logs Location

C:\Program Files\Elastic\Agent\data\elastic-agent-*\logs\elastic-agent.log

Use for: Investigating startup or connection issues.

### Fleet Server Connection and Enrollment Troubleshooting

Use for: Checking certs, connectivity, enrollment tokens, Fleet Server availability.

https://www.elastic.co/guide/en/fleet/current/fleet-troubleshooting.html

### GitHub Issues - Elastic Agent

Use for: Asking questions, searching similar install problems, especially version-specific ones.
If install is corrupted:

https://discuss.elastic.co/

## Powershell Troubleshooting Methods

### Uninstall or Force Remove Agent/Endpoint

Use for: Clean removal to reinstall, or start fresh if install is broken.

`.\elastic-agent.exe uninstall --force`

### Check agent status

`.\elastic-agent.exe status`

Clear and Reinstall

*If install is corrupted:*

Uninstall agent

Delete data folder manually (if uninstall fails):

`Remove-Item -Recurse -Force "C:\Program Files\Elastic\Agent"`

Then Reinstall MSI file from Elastic Agent downloads page

### Force Stop Elastic Agent Service

Sometimes the agent locks itself and prevents uninstall. Force stop it:

`Stop-Service -Name "Elastic Agent" -Force`

Verify it's stopped:

`Get-Service -Name "Elastic Agent"`

### Change Folder Permissions (if uninstall is blocked due to file locks)

Grant yourself full control of the Elastic Agent folder:

`$path = "C:\Program Files\Elastic\Agent"`

`takeown /F "$path" /R /D Y`

`icacls "$path" /grant administrators:F /T`

Now try uninstalling again.

### Use Agent Uninstall CLI with --force

`cd "C:\Program Files\Elastic\Agent"`

`.\elastic-agent.exe uninstall --force`

 ### Check for Permission Issues

Test if you can create/delete files in the install path:

`New-Item "C:\Program Files\Elastic\Agent\test.txt" -Force`

`Remove-Item "C:\Program Files\Elastic\Agent\test.txt"`

If denied, confirm you're part of Administrators group:

`whoami /groups`













