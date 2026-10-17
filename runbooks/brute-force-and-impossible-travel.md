# Runbook: Brute Force and Impossible Travel

**Alert sources:** IdP sign in logs, VPN/firewall auth logs, SIEM rules on
failed logon bursts or geographically impossible sessions.

## 1. Triage (first 15 minutes)

- [ ] Identify the account, source IPs, failure count and time window.
      Password spraying looks like few failures across many accounts from one
      IP; classic brute force is many failures against one account.
- [ ] Check whether any logon *succeeded*. A success after a failure burst is
      the highest priority signal in this runbook.
- [ ] For impossible travel: note the two locations, timestamps and implied
      speed. Rule out VPN egress, mobile carrier NAT and corporate proxies
      before calling it malicious.
- [ ] Check the account type. Service accounts, admins and executives get
      priority.

## 2. Scope

- [ ] Pull 30 days of successful logons for the account: new IPs, new
      user agents, new locations.
- [ ] Check what the session touched: mail, file shares, VPN, cloud consoles.
      Look for mailbox rules, OAuth grants and MFA changes made by the
      session.
- [ ] Search for the source IPs across all auth logs: which other accounts
      did they try?

## 3. Contain

- [ ] Disable or lock the account if compromise is confirmed; revoke all
      sessions and refresh tokens.
- [ ] Force password reset and re register MFA.
- [ ] Block the attacker IPs at the perimeter. Do not block your own VPN
      egress ranges by mistake.

## 4. Evidence to collect

Full auth log extract for the account, source IP list with geolocation,
session activity after the suspect logon, any mailbox/OAuth/MFA changes.

## 5. Escalate when

Confirmed successful logon by the attacker, any admin or executive account,
or signs of persistence (new MFA method, new OAuth app, forwarding rules).
