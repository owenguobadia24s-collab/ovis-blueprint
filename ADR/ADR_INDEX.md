# ADR Index

| ADR number | title | status | depends_on | brief summary |
| --- | --- | --- | --- | --- |
| ADR-001 | Canonical State Model and Lineage Contract | Accepted | none | Defines canonical OVIS object types, IDs, lineage relations, transitions, append-only audit rules, storage-agnostic state, and explicit legacy mapping for the Notion-era pipeline. |
| ADR-002 | Function Calling as the Canonical OVIS Capability Bus | Accepted | ADR-001 | Defines function calling as the canonical executable capability bus with schema discipline, idempotency, side-effect boundaries, policy enforcement expectations, and clear separation from skills, MCP, and legacy direct scripts. |
| ADR-003 | Responses API as the Canonical OVIS Runtime | Accepted | ADR-001, ADR-002 | Defines the Responses API as the canonical runtime substrate, the runtime wrapper as the only canonical model invocation path, prohibits scattered direct model calls, and classifies OpenAI-managed durable state as optional or derived. |
| ADR-004 | ZDR-First Session Mode and Continuation Policy | Accepted | ADR-001, ADR-002, ADR-003 | Defines ZDR-first as the default governed session posture, explicit response chaining as the canonical continuation pattern, and branch continuity as OVIS-owned rather than runtime-owned. |
| ADR-005 | Review Gate and Policy Profiles | Accepted | ADR-001, ADR-002, ADR-003, ADR-004 | Defines the canonical review gate between `Plan Job` and `Execute Job`, action-centric risk classes `R0–R4`, baseline policy profiles, and Approval-based execution enforcement. |
| ADR-006 | Branch Compaction and Continuity | Accepted | ADR-001, ADR-002, ADR-003, ADR-004, ADR-005 | Defines the canonical Branch object as the continuity container, compaction trigger classes, preserved references, compacted state usage, and canonical compaction event emission. |
