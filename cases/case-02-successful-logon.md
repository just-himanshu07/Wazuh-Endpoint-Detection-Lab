# Case 02 — Successful logon

## Objective
Confirm that successful Windows logon records are collected and distinguish them from failed attempts.

## Evidence observed
The captured dashboard showed two events described as `Windows Workstation Logon Success`. The visible Wazuh rule ID was `60118`, with rule level `3`.

## Analysis
A successful logon is normal on a workstation. Its significance depends on the account, logon type, time, source and surrounding activity. When success follows repeated failures, correlate the records before concluding that an account was compromised.

## Classification
**Informational on its own; context-dependent if it follows suspicious failures.**

## Follow-up
- Compare timestamps with authentication-failure events.
- Confirm the account and host.
- Inspect logon type and source fields where available.
- Check subsequent process-creation events for activity unexpected for that account.
