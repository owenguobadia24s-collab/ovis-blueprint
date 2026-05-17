---
id: CJ-RETURN-0004
title: OVC/OVIS Separation Boundary
type: CJ
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TASKS/CJ-RETURN-0004_OVC_OVIS_SEPARATION_BOUNDARY.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/CJ-RETURN-0004.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Formally adopt and lock the OVC/OVIS separation boundary as canonical doctrine before any OVC repo inspection work begins.

# Scope

This CJ defines and adopts boundary doctrine only. It does not recover, normalize, or rewrite any OVC repos.

Constraints:

- no OVC repo mutation during initial inspection work
- no OVIS architecture changes during initial inspection work
- no metadata normalization during OVC inspection unless a separate approved CJ is opened

# Content

## Boundary Authority

The canonical boundary authority is:

- ADR-012: `ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md`

All OVC inspection work must comply with ADR-012.

## CJ-RETURN-0005 Output Location Rule

**CJ-RETURN-0005 must declare its own date-stamped output location when it opens.**

Preferred output location characteristics:

- outside active repos (`ovis-blueprint`, `ovis-runtime`, `ovis-knowledge`)
- outside OVC repos (`ovc-infra`, `ovc-infra-fresh`)

Recommended example path format (Windows; example only; not created by CJ-RETURN-0004):

`C:\\Users\\Owner\\OVIS_RETURN_REPORTS\\ovc\\<YYYY-MM-DD>\\`

## Deliverables

CJ-RETURN-0004 delivers:

- expanded + accepted ADR-012 boundary doctrine
- ADR index updated to reflect ADR-012 acceptance
- five inspection templates under `TEMPLATES/OVC_INSPECTION/` for CJ-RETURN-0005 to copy into its declared output location

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
- ADR/ADR_INDEX.md
