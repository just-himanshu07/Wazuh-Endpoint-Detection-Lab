# Case 01 — Authentication failures

## Objective
Verify that failed Windows logon activity is visible in Wazuh and identify the fields needed for triage.

## Evidence observed
The captured Threat Hunting view for `Windows-Laptop-2` showed three results described as `Logon Failure - Unknown user or bad password`. The visible Wazuh rule ID was `60122`, with rule level `5`.

## Analysis
The description indicates that Windows rejected an authentication attempt because the supplied account name or password was not accepted. Inspect the target account, logon type, source/workstation details, status/substatus codes and timestamp in the raw event.

Three failures alone do not establish brute force. Possible explanations include a lab test, mistyped credentials or a stale saved password. Frequency, time window, account spread, source context and any later successful logon are needed to assess the pattern.

## Classification
**Suspicious activity requiring context; not a confirmed incident from the count alone.**

## Follow-up
1. Open an individual alert and inspect the raw Windows event.
2. Record account, source/workstation, logon type, status and substatus if present.
3. Check for other failures in the same time window.
4. Look for a later successful logon and compare account and source.
5. Note whether the activity matches the controlled lab test.
