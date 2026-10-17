# Runbook: Suspicious PowerShell

**Alert sources:** EDR behavioural detection, Sysmon Event ID 1 with
powershell.exe, AMSI trigger, SIEM Sigma rule on obfuscation.

## 1. Triage (first 15 minutes)

- [ ] Get the full command line (Event ID 4688 with command line logging, or
      Sysmon Event ID 1). Truncated command lines hide the payload.
- [ ] Look for obfuscation markers: `-EncodedCommand` / `-enc`, heavy use of
      backticks, string concatenation (`+`), `[Convert]::FromBase64String`,
      `IEX` / `Invoke-Expression`, `DownloadString`, `Net.WebClient`.
- [ ] Identify the parent process. PowerShell spawned by `winword.exe`,
      `excel.exe` or `mshta.exe` is malicious until proven otherwise.
      PowerShell spawned by an admin's terminal needs context, not panic.
- [ ] Decode any base64 payload in a sandbox and read what it actually does
      before deciding.

## 2. Scope

- [ ] Search the fleet for the same command line pattern, decoded URLs and
      file hashes from the last 14 days.
- [ ] Check for follow on behaviour on the host: credential access
      (Event ID 4656 on lsass), new scheduled tasks, outbound connections to
      the decoded infrastructure.
- [ ] Review the user's recent email and web activity if the parent suggests
      a phishing origin.

## 3. Contain

- [ ] Kill the process tree and isolate the host if the decoded payload is
      malicious (downloader, credential dumper, C2 stager).
- [ ] Block decoded domains/IPs and hashes at proxy, firewall and EDR.
- [ ] If AMSI was bypassed or the script disabled defences, treat the host
      as fully compromised until reimaged.

## 4. Evidence to collect

Full command line, decoded payload, parent/child process tree, AMSI/Sysmon
events, network connections during execution, persistence artefacts.

## 5. Escalate when

Decoded payload shows C2, credential theft, or lateral movement tooling; or
the same pattern appears on multiple hosts.
