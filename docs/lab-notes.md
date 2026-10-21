# Lab notes

## Environment captured

- Wazuh single-node Docker lab on Kali Linux.
- Windows 11 Home Single Language endpoint.
- Agent name: `Windows-Laptop-2`; agent ID: `003`.
- Endpoint IP shown: `192.168.1.17`.
- Dashboard/manager address shown: `192.168.1.12`.

## Validation performed

- Opened the Wazuh agent page and confirmed the endpoint status was `active`.
- Used Threat Hunting with the Windows endpoint filter applied.
- Searched for `1102`; two matching records appeared.
- Opened event details to inspect the Windows event message, account name, provider and timestamps.
- Reviewed the authentication-failure and authentication-success alert summaries.
- Inspected process-creation records and parent-process fields.

## Evidence handling

Screenshots record the dashboard state during this lab. They are not a complete export of all underlying records. For a formal investigation, export the relevant event documents and record the time range, timezone and filters used.
