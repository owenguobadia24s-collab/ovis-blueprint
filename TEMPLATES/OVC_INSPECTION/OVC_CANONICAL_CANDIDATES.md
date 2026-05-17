---
id: TEMPLATE-OVC-0003
title: OVC Canonical Candidates Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TEMPLATES/OVC_INSPECTION/OVC_CANONICAL_CANDIDATES.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-OVC-0003.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Provide the reusable template named OVC Canonical Candidates Template.

# Scope

This template is copied and filled during CJ-RETURN-0005 (read-only OVC inspection).

CJ-RETURN-0004 does not create inspection outputs. CJ-RETURN-0005 must declare its own date-stamped output location and then copy this template there.

# Content

## Inclusion Criteria

List the criteria an OVC artifact must satisfy to be considered a canonical candidate (report-only; no promotion in CJ-RETURN-0005).

## Exclusion Rationale

List common exclusion reasons (e.g., private data, duplicates, ambiguous provenance, non-reproducible artifacts).

## Candidates

| repo | path | class | notes | hash_or_oid | proposed_next_step |
|---|---|---|---|---|---|

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
