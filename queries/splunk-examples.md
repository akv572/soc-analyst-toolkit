# Splunk SPL Examples

Replace index and field names with your own. Each query is a starting point
for a hunt, not a finished detection.

## Failed logons by account

```spl
index=wineventlog EventCode=4625
| stats count as failures, dc(TargetUserName) as accounts, values(src_ip) as src_ips by TargetUserName
| where failures > 10
| sort - failures
```

## Successful logon after a failure burst

```spl
index=wineventlog (EventCode=4625 OR EventCode=4624)
| stats sum(eval(EventCode=4625)) as failures, sum(eval(EventCode=4624)) as successes by TargetUserName, src_ip
| where failures > 5 AND successes > 0
```

## PowerShell with encoded commands

```spl
index=wineventlog EventCode=4688 OR index=sysmon EventCode=1
(process_name="*powershell.exe" OR process_name="*pwsh.exe")
(CommandLine="*-enc*" OR CommandLine="*FromBase64String*" OR CommandLine="*IEX*")
| table _time, host, user, parent_process, CommandLine
```

## New scheduled tasks

```spl
index=wineventlog EventCode=4698
| table _time, host, user, TaskName
```

## Rare outbound destinations per host

```spl
index=proxy
| stats dc(host) as host_count, values(host) as hosts by dest_domain
| where host_count < 3
| sort host_count
```

## USB device insertions

```spl
index=wineventlog EventCode=6416
| table _time, host, user, DeviceDescription
```
