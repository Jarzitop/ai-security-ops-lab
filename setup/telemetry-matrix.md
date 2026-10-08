# Telemetry matrix — AI-Augmented Security Operations Lab

**Status:** PLANNED. **Last review:** 2026-10-08.
No telemetry source is claimed as implemented or validated as of this document.
Only benign, explicitly authorized activities on the Windows host.

## Validation protocol

Each experiment must record all of:

- **Expected event:** event ID, channel, relevant source fields.
- **Generated activity:** exact benign action and any configuration prerequisites.
- **Observed event:** actual result, raw event or Wazuh alert, ID and index pattern.
- **Timestamp:** UTC timestamp (ISO 8601), noting endpoint-to-Wazuh delay.
- **Source:** hostname, operating system, agent ID, channel / provider.
- **Fields:** expected and observed `EventID`, `Computer`, `Image`, `CommandLine`, `User`, `ProcessGuid` (only where relevant).
- **Evidence:** sanitized command output, exported JSON and/or screenshots with audit trail. Never commit secrets, unredacted personal data or complete private logs.

## Matrix

| ID | Source | Expected event | Generated activity (benign) | Relevant fields (expected) | Observed event | Timestamp (UTC) | Evidence | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| T01 | Windows Event Log / Application | Provider and Event ID 100 (custom informational test) | Create a distinct harmless informational Application event; verify Windows-side creation | provider, eventID, computer, message | — | — | — | PLANNED |
| T02 | Windows Event Log / Security | Security Event ID 4624 (successful logon, when auditing is enabled) | Normal interactive sign-in to an authorized test account; avoid repeated failed logins | eventID, computer, targetUserName, logonType | — | — | — | PLANNED |
| T03 | Sysmon / Operational | Sysmon Event ID 1 (process creation, contingent on deployed config) | Open and close Notepad on a controlled endpoint after installing Sysmon | eventID, computer, Image, User, ProcessGuid, CommandLine if recorded | — | — | — | PLANNED |
| T04 | Wazuh agent pipeline | T01-T03 source data forwarded to manager | Repeat a previously confirmed local test and match agent hostname/time | agent.id, agent.name, rule.id if alert exists | — | — | — | PLANNED |
| T05 | Wazuh indexer | Event discoverable in `wazuh-alerts-*` or controlled `wazuh-archives-*` | Query by hostname + eventID + bounded timestamp | timestamp, source channel, eventdata, agent.id | — | — | — | PLANNED |
| T06 | Linux auth telemetry (later phase) | Successful SSH login or sudo audit event in lab Linux endpoint | Authorized Linux test session after Linux source onboarding | hostname, user, program, timestamp | — | — | — | PLANNED |

## Caveats

- Wazuh agent typically monitors Windows Application, Security and System channels by default. Confirm actual local agent configuration, including event severity filters. Informational Application events can be filtered out unless configured correctly.
- Sysmon is **not** installed by deploying Wazuh Agent; the Operational channel must be monitored explicitly.
- `wazuh-alerts-*` represents alerts, not necessarily every source event. For event-level evidence, either demonstrate a relevant rule fired, or intentionally enable `wazuh-archives-*` for a short and cost-controlled validation period.
- Keep the distinction between "event present in Event Viewer", "event received by Wazuh manager" and "event searchable in indexer". Record all three stages.
- If a test fails, record **FAILED** with reason and actual observed data; do not replace evidence with assumptions.
- A test is **VALIDATED** only when local source and Wazuh query evidence match by host, event ID / provider and time window.

## Evidence record template

```text
Test ID:
Executed at (UTC):
Endpoint hostname (sanitized):
Wazuh agent ID:
Expected event/channel:
Activity executed:
Local Event Viewer / command evidence:
Manager ingest evidence:
Indexer name and query:
Observed event timestamp (UTC):
Relevant actual fields:
Result (VALIDATED / FAILED / BLOCKED):
Sanitized evidence path or screenshot reference:
Notes / discrepancies:
```

Documentation:
- https://documentation.wazuh.com/current/user-manual/capabilities/log-data-collection/configuration.html
- https://documentation.wazuh.com/current/user-manual/wazuh-indexer/wazuh-indexer-indices.html
