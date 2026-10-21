# Case 03 — Process creation telemetry

## Objective
Use process and parent-process details to understand what ran on the monitored endpoint.

## Evidence observed
The captured event details included `C:\Windows\System32\ipconfig.exe` as the new process. Parent-process values in the viewed records included `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe` and `C:\Windows\System32\cmd.exe`.

## Analysis
`ipconfig.exe` is a standard Windows utility. Its presence is not suspicious by itself. The parent process, command line, user, timestamp and reason for execution provide context. In this lab, the records were reviewed as examples of process telemetry.

## Classification
**Expected administrative utility; the process name alone does not support a malicious conclusion.**

## Follow-up
1. Inspect the full command line and user context if available.
2. Confirm parent process and timestamp.
3. Correlate with nearby logon events.
4. Investigate further only if the command, parent, user or timing is inconsistent with expected activity.
