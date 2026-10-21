# Wazuh dashboard search notes

These notes reflect searches used during the lab. Dashboard version, filters and selected time range affect the results.

## Audit log cleared
Entered in Threat Hunting:

```text
1102
```

Observed: two matching records described as `The audit log was cleared`; rule ID `63103`, level `5`.

## Authentication failures
The dashboard's Authentication failure filter showed three results described as `Logon Failure - Unknown user or bad password`; rule ID `60122`, level `5`.

## Successful logons
The Authentication success filter showed two results described as `Windows Workstation Logon Success`; rule ID `60118`, level `3`.

## Process creation
Individual event details included fields such as:

- `data.win.eventdata.newProcessName`
- `data.win.eventdata.parentProcessName`
- `data.win.eventdata.subjectUserName`

Field availability depends on the event and configuration. Inspect the raw event before assuming a field is populated.
