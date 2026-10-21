# Lab architecture

## Components

- **Kali Linux:** Wazuh manager, indexer and dashboard running as a single-node Docker lab.
- **Windows 11:** monitored endpoint with the Wazuh agent.
- **Windows Security log:** source for authentication, account-management and audit-log events.
- **Sysmon:** additional process and network telemetry where its configuration enables those events.
- **Wazuh Threat Hunting:** used to filter events, inspect rule metadata and open raw event details.

## Event path

1. Windows writes an event to a local event channel.
2. The Wazuh agent collects configured event channels.
3. The manager processes events through its decoders and rules.
4. Alerts are indexed and become searchable in the dashboard.
5. The analyst reviews the alert and the underlying event, then documents a conclusion.

A dashboard result confirms that a matching record is indexed. It does not, on its own, prove malicious intent.
