---
id: TEMPLATE-OVC-0002
title: OVC Artifact Classification Ledger Template
type: TEMPLATE
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: return
repo: ovis-blueprint
path: TEMPLATES/OVC_INSPECTION/OVC_ARTIFACT_CLASSIFICATION_LEDGER.md
owner: Owen Vitae
created: '2026-05-17'
last_updated: '2026-05-17'
registry: ovis-blueprint/REGISTRIES/entries/TEMPLATE-OVC-0002.yaml
module_id: MOD-BRIDGE-DISPATCH-0001
module_slug: bridge_dispatch
system_id: SYS-INTEGRATION-0001
system_slug: integration_bridge
---

# Purpose

Provide the reusable template named OVC Artifact Classification Ledger Template.

# Scope

This template is copied and filled during CJ-RETURN-0005 (read-only OVC inspection).

CJ-RETURN-0004 does not create inspection outputs. CJ-RETURN-0005 must declare its own date-stamped output location and then copy this template there.

# Content

## Classification Classes

Each discovered artifact must be classified as one of:

1. OVC Doctrine
2. OVC Implementation
3. OVC Archive
4. OVC Data
5. OVC Examples/Screenshots
6. OVC Journals
7. Cross-System Bridge
8. Non-Canonical / Unresolved

## Ledger

| repo | path | artifact_class | notes | hash_or_oid | bridge_fields_present |
|---|---|---|---|---|---|

# References

- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
