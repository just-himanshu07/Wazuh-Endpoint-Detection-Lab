# Wazuh Endpoint Detection Lab

A small home SOC lab built to practice endpoint monitoring and first-line investigation with Wazuh. The manager, indexer and dashboard run on Kali Linux; a Windows 11 endpoint sends security events through the Wazuh agent. I used Windows Security events and process telemetry to investigate authentication activity, process creation and audit-log clearing.

This repository documents the setup I tested, what appeared in the dashboard, and the limits of the evidence. An alert is a starting point for investigation, not proof of compromise.

## Lab layout

```text
Kali Linux
├── Wazuh Manager
├── Wazuh Indexer
└── Wazuh Dashboard
       ▲
       │ Wazuh agent connection over the local network
       ▼
Windows 11 endpoint
├── Wazuh Agent
├── Windows Security log
└── Sysmon (where configured)
```

## Environment captured during testing

| Component | Observed value |
|---|---|
| Wazuh agent | `Windows-Laptop-2` |
| Agent ID | `003` |
| Agent status | Active in the captured dashboard |
| Windows version | Windows 11 Home Single Language, `10.0.26200.9711` |
| Agent version shown | Wazuh `v4.14.9` |
| Windows endpoint IP shown | `192.168.1.17` |
| Dashboard/manager address used | `192.168.1.12` |
| Search time range | Last 24 hours in the captured searches |

These values come from the screenshots. If this repository is public, consider replacing local IPs, usernames and host identifiers with placeholders.

## Cases covered

1. **Authentication failures** — Wazuh showed `Logon Failure - Unknown user or bad password` (rule `60122`, level `5` in the captured view).
2. **Successful logons** — Wazuh showed `Windows Workstation Logon Success` (rule `60118`, level `3` in the captured view).
3. **Process creation** — event details included `C:\Windows\System32\ipconfig.exe` and parent-process values including PowerShell and Command Prompt.
4. **Security audit log cleared** — Wazuh showed `The audit log was cleared` (rule `63103`, level `5` in the captured view).
5. **Endpoint validation** — the Windows endpoint was shown as active and its events were searchable in Threat Hunting.

## Findings from the screenshots

- The dashboard showed agent `003` as active.
- The authentication-failure filter showed **3 results** in the captured time range.
- The authentication-success filter showed **2 results** in the captured time range.
- Searching for `1102` returned **2 records** described as `The audit log was cleared`.
- The opened log-clearing event contained the message `The audit log was cleared` and account name `Ansh2`.
- Process details showed `ipconfig.exe`; the parent process shown in the inspected records included `powershell.exe` or `cmd.exe`.

These counts are snapshots, not fixed totals. They change with the selected time range and new events.

## Investigation method

For each result, I checked the timestamp, endpoint, rule description, rule ID, rule level and raw event details. Where available, I looked at account, process and parent-process fields, then considered nearby events and whether the activity was part of my own lab test.

A few failed logons do not prove brute force. A successful logon is not automatically suspicious. A log-clearing event deserves attention when unexpected, but this lab included an intentional test. Conclusions should reflect that context.

## Repository map

```text
Wazuh-Endpoint-Detection-Lab/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── lab-notes.md
│   └── safety-and-limitations.md
├── cases/
│   ├── case-01-authentication-failures.md
│   ├── case-02-successful-logon.md
│   ├── case-03-process-creation.md
│   └── case-04-security-log-cleared.md
├── queries/
│   └── dashboard-search-notes.md
├── screenshots/
└── evidence/
    └── evidence-index.md
```

## Reproduce the checks

1. Confirm the Wazuh containers are healthy on Kali.
2. Confirm the Windows agent is active in Wazuh.
3. In Threat Hunting, select the Windows endpoint and a suitable time range.
4. Search for `1102`, or use the dashboard's authentication-failure/success filters.
5. Open individual events and inspect the raw event details; don't rely only on a chart or count.
6. Capture screenshots with the time range visible and remove sensitive details before publishing.

## What I learned

This lab gave me practice following Windows endpoint events into a SIEM, reading Wazuh rule metadata and separating an alert from a confirmed incident. It also reinforced the need to preserve evidence before destructive tests and to check the underlying event instead of relying on the dashboard summary alone.

## Next improvements

- Document the Sysmon configuration and verify which event channels are enabled.
- Repeat authentication tests with a recorded test plan.
- Compare Wazuh's parsed fields with the original Windows event.
- Add a consistent timeline and incident-report template.
- Review alert tuning only after validating coverage and false positives.

## Safety note

Run tests only on systems you own or are authorized to administer. Clearing the Security log is destructive; export it first and never do this on a production, shared or managed machine. Do not publish credentials, tokens or sensitive incident data.
