# Runbook: Phishing Email Triage

**Alert sources:** email security gateway, user reported phish button, SIEM
correlation on malicious URL or attachment detonation.

## 1. Triage (first 15 minutes)

- [ ] Pull the original message with full headers. Note `Return-Path`,
      `Received` chain, `Reply-To`, and SPF/DKIM/DMARC results.
- [ ] Check sender reputation: domain age, lookalike domain
      (e.g. `micorsoft.com`), display name spoofing.
- [ ] Detonate URLs and attachments in a sandbox, or check hashes against
      VirusTotal / abuse feeds. Never open them on your workstation.
- [ ] Identify the lure: credential harvesting, malicious macro, QR code,
      callback phishing (phone number in the body).

## 2. Scope

- [ ] Search the mail gateway logs: how many users received the same message?
- [ ] Check who opened it, clicked, or submitted credentials (proxy logs,
      IdP sign in logs).
- [ ] If credentials were submitted: treat as account compromise, move to
      the brute force / account takeover runbook.

## 3. Contain

- [ ] Quarantine or purge the message tenant wide from all mailboxes.
- [ ] Block sender domain, URLs and file hashes at the gateway, proxy and
      EDR.
- [ ] Force password reset and revoke sessions for users who submitted
      credentials; require MFA re registration if it was bypassed.

## 4. Evidence to collect

Message headers, gateway verdict, sandbox report, list of recipients and
their click/submit status, proxy logs for the malicious URLs, IdP logs for
affected accounts.

## 5. Escalate when

Any credential submission, any malware execution on an endpoint, or more than
a handful of recipients. Document everything in the ticket before handing to
Tier 2.
