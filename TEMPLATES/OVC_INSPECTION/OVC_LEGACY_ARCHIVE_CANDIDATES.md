---
id: TEMPLATE-OVC-0004
title: OVC Legacy Archive Candidates Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TEMPLATES/OVC_INSPECTION/OVC_LEGACY_ARCHIVE_CANDIDATES.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-OVC-0004.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Provide the reusable template named OVC Legacy Archive Candidates Template.

# Scope

This template is copied and filled during CJ-RETURN-0005 (read-only OVC inspection).

CJ-RETURN-0004 does not create inspection outputs. CJ-RETURN-0005 must declare its own date-stamped output location and then copy this template there.

# Content

## Archive Criteria

List the criteria an OVC artifact must satisfy to be treated as legacy/archive material (report-only; no deletion/migration in CJ-RETURN-0005).

## Candidates

| repo | path | class | notes | hash_or_oid | recommended_retention |
|---|---|---|---|---|---|

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
