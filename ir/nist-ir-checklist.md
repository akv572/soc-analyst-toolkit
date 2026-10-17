# Incident Response Checklist (NIST SP 800-61)

## 1. Preparation

- [ ] Contact list current: IR team, IT, legal, HR, PR, executives.
- [ ] Tooling ready: EDR console access, forensic images, write blockers,
      clean admin credentials, out of band comms channel.
- [ ] Playbooks reviewed in the last quarter.

## 2. Detection and Analysis

- [ ] Alert validated: true positive, not a misconfiguration or test.
- [ ] Initial severity assigned (use your matrix, document the reasoning).
- [ ] Timeline started: every action timestamped from here on.
- [ ] Scope bounded: which hosts, accounts, data are affected?
- [ ] Ticket opened with all evidence attached.

## 3. Containment

- [ ] Short term: isolate hosts, block IOCs, disable accounts. Preserve
      evidence first: memory capture before reimaging.
- [ ] Confirm containment worked: no new beacons, no new logons.
- [ ] Long term plan: what stays blocked until eradication is done?

## 4. Eradication

- [ ] Root cause identified: how did they get in?
- [ ] Malware and persistence removed from all affected systems.
- [ ] Attacker created accounts, tasks and services removed.
- [ ] Vulnerabilities or misconfigurations that enabled access are fixed.

## 5. Recovery

- [ ] Systems restored from known good backups or rebuilt.
- [ ] Enhanced monitoring on affected assets for at least 30 days.
- [ ] Business owner signs off on return to production.

## 6. Post Incident

- [ ] Lessons learned meeting within 5 business days.
- [ ] Timeline finalised and report written.
- [ ] Detections added or tuned so this class of activity fires earlier.
- [ ] Action items tracked to completion, not just assigned.
