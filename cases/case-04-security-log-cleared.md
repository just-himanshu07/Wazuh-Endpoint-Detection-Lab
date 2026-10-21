# Case 04 — Security audit log cleared (Event ID 1102)

## Objective
Verify that a Windows Security audit-log clearing event reaches Wazuh and inspect the actor and event context.

## Precaution
Clearing the Security log removes locally retained audit records. I exported the log to an EVTX file before the lab test. This must not be performed on a production, shared or managed system.

## Evidence observed
The Threat Hunting search for `1102` showed two matching records for `Windows-Laptop-2`. Both were described as `The audit log was cleared`, with Wazuh rule ID `63103` and rule level `5`.

The opened event details displayed `The audit log was cleared` and the account name `Ansh2`. The dashboard showed timestamps around `Oct 22, 2026 @ 04:44:38` and `04:44:42`. The event details also showed the Windows Event Log provider.

## Analysis
A cleared Security audit log can reduce evidence available to an investigation and should be reviewed when unexpected. In this exercise, the action was intentional, so the alert was expected. The screenshots alone do not explain why two matching records were indexed; compare the original Windows records and surrounding data before deciding whether they represent separate actions or duplicate/related ingestion.

## Classification
**Expected lab activity. In a real environment, investigate as potentially suspicious until authorized activity is confirmed.**

## Follow-up
1. Preserve the exported EVTX backup and Wazuh event details.
2. Confirm the actor from the complete event record.
3. Review nearby logon, privilege-use and process-creation events.
4. Check whether the clearing action was authorized.
5. Document any gap between the Windows event and the indexed Wazuh records.
