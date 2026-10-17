# Windows Security Event IDs

The IDs that matter most during triage. From the Security log unless noted.

## Authentication

| ID | Meaning |
|---|---|
| 4624 | Successful logon (check Logon Type) |
| 4625 | Failed logon |
| 4648 | Explicit credential logon (runas) |
| 4672 | Special privileges assigned (admin logon) |
| 4740 | Account locked out |
| 4768 | Kerberos TGT requested |
| 4769 | Kerberos service ticket requested |
| 4771 | Kerberos pre-auth failed |

Common 4624 logon types: 2 interactive, 3 network, 4 batch, 5 service,
7 unlock, 10 remote interactive (RDP), 11 cached.

## Account management

| ID | Meaning |
|---|---|
| 4720 | User account created |
| 4728 / 4732 / 4756 | Added to privileged group |
| 4738 | User account changed |

## Process and defence

| ID | Meaning |
|---|---|
| 4688 | Process created (needs command line logging enabled) |
| 4697 | Service installed |
| 4698 | Scheduled task created |
| 7045 | Service installed (System log) |
| 4656 / 4658 | Handle to object requested/closed (lsass access) |

## Sysmon (if deployed)

| ID | Meaning |
|---|---|
| 1 | Process creation (with full command line and hashes) |
| 3 | Network connection |
| 7 | Image loaded (DLL) |
| 11 | File created |
| 13 | Registry value set |

## PowerShell logging (enable all three)

Module logging (Event ID 4103), script block logging (4104), transcription.
Without 4104 you are triaging blind.
