# SOC Analyst Toolkit

A blue team field reference I am building as I prepare for SOC analyst work:
triage runbooks for the alerts a Tier 1 analyst sees most, Sigma detection
rules, SIEM query examples, and the cheatsheets I actually reach for.

Everything here is written to be usable, not just readable. Adapt the
thresholds, log source names and escalation paths to your own environment.

## Contents

### Runbooks

Step by step triage for common alerts, mapped to NIST SP 800-61 phases.

* [Phishing email triage](runbooks/phishing-triage.md)
* [Endpoint malware alert](runbooks/endpoint-malware-alert.md)
* [Brute force and impossible travel](runbooks/brute-force-and-impossible-travel.md)
* [Suspicious PowerShell](runbooks/suspicious-powershell.md)

### Detections

Sigma rules ready to convert with `sigmac` for your SIEM.

* [Mimikatz indicators](detections/sigma/windows_credential_access_mimikatz.yml)
* [Suspicious scheduled task creation](detections/sigma/windows_persistence_schtasks.yml)
* [Obfuscated PowerShell](detections/sigma/windows_defence_evasion_powershell_obfuscation.yml)
* [Authentication brute force](detections/sigma/auth_brute_force_failures.yml)

### Queries

* [Splunk SPL examples](queries/splunk-examples.md)
* [KQL (Microsoft Sentinel) examples](queries/kql-examples.md)

### Cheatsheets

* [Wireshark display filters](cheatsheets/wireshark-display-filters.md)
* [Windows security event IDs](cheatsheets/windows-event-ids.md)
* [Linux authentication log locations](cheatsheets/linux-auth-logs.md)

### Incident response

* [NIST IR phase checklist](ir/nist-ir-checklist.md)

## How to use this repo

1. Start with a runbook when an alert fires; each one tells you what to check,
   what evidence to collect, and when to escalate.
2. Tune a Sigma rule to your log sources before deploying it anywhere real.
3. Treat the query examples as starting points: replace index, table and field
   names with your own.
