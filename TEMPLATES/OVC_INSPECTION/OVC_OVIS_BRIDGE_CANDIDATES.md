---
id: TEMPLATE-OVC-0005
title: OVC/OVIS Bridge Candidates Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TEMPLATES/OVC_INSPECTION/OVC_OVIS_BRIDGE_CANDIDATES.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-OVC-0005.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Provide the reusable template named OVC/OVIS Bridge Candidates Template.

# Scope

This template is copied and filled during CJ-RETURN-0005 (read-only OVC inspection).

CJ-RETURN-0004 does not create inspection outputs. CJ-RETURN-0005 must declare its own date-stamped output location and then copy this template there.

# Content

## Bridge Constraints

The bridge must not contain trading interpretation. Only the following are allowed:

- artifact metadata (envelope only)
- file registration and repo/path mapping
- validation status
- promotion status
- checksum/hash
- version
- authority classification
- event log reference

## Candidates

| repo | path | class | notes | hash_or_oid | bridge_fields_present |
|---|---|---|---|---|---|

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
