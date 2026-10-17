# KQL Examples (Microsoft Sentinel)

## Failed sign ins by user

```kql
SigninLogs
| where ResultType != 0
| summarize failures=count(), ips=make_set(IPAddress) by UserPrincipalName
| where failures > 10
| order by failures desc
```

## Successful sign in after failures (possible compromise)

```kql
SigninLogs
| summarize failures=countif(ResultType != 0), successes=countif(ResultType == 0) by UserPrincipalName, IPAddress
| where failures > 5 and successes > 0
```

## Impossible travel (simplified)

```kql
SigninLogs
| where ResultType == 0
| summarize locations=make_set(Location), times=make_set(TimeGenerated) by UserPrincipalName
| where array_length(locations) > 1
// Refine with real distance/time math before alerting; VPN egress causes noise
```

## Process creation with suspicious parents

```kql
DeviceProcessEvents
| where InitiatingProcessFileName in ("winword.exe", "excel.exe", "mshta.exe")
| where FileName in ("powershell.exe", "cmd.exe", "wscript.exe", "cscript.exe")
| project Timestamp, DeviceName, InitiatingProcessFileName, FileName, ProcessCommandLine
```

## Defender malware detections

```kql
DeviceEvents
| where ActionType == "AntivirusDetection"
| summarize count() by DeviceName, FileName=extractjson("$.FileName", AdditionalFields)
| order by count_ desc
```

## New local user creation

```kql
DeviceEvents
| where ActionType == "UserAccountCreated"
| project Timestamp, DeviceName, AccountName=extractjson("$.TargetAccount", AdditionalFields)
```
