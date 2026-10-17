# Linux Authentication Log Locations

## Where to look

| Distro | Auth log |
|---|---|
| Debian / Ubuntu | `/var/log/auth.log` |
| RHEL / CentOS / Fedora | `/var/log/secure` |
| Arch | journald: `journalctl _COMM=sshd` |

systemd systems also keep everything in the journal:
`journalctl -u sshd --since "1 hour ago"`.

## Quick triage commands

```bash
# Failed SSH logons by source IP
grep "Failed password" /var/log/auth.log | grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -c | sort -nr | head

# Successful logons in the last day
grep "Accepted" /var/log/auth.log | tail -20

# New user accounts
grep "new user" /var/log/auth.log

# sudo usage
grep "sudo:" /var/log/auth.log | tail -20

# Logons outside business hours (rough cut)
grep "Accepted" /var/log/auth.log | awk '$3 < "08:00:00" || $3 > "18:00:00"'
```

## What is suspicious

* `Accepted password` for root from an external IP.
* Bursts of `Failed password` from one IP (brute force) or one user across
  many hosts (spraying).
* `Accepted publickey` from an unfamiliar key: check
  `~/.ssh/authorized_keys` for additions.
* `session opened for user` by an unexpected parent process.
