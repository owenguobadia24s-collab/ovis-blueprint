---
id: ADR-ARC-0012
title: OVIS OVC Ecosystem Boundary
type: ADR
status: proposed
authority: canonical
version: '0.1'
layer: blueprint
domain: arc
repo: ovis-blueprint
path: ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/ADR-ARC-0012.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
---

# Purpose

Fix the current ecosystem boundary so structural taxonomy work proceeds on OVIS without accidental OVC expansion.

# Scope

This ADR governs the current relationship between OVIS and OVC for structural registry and taxonomy purposes.

# Content

## Decision

OVIS is the active governed substrate and structural authority in the current workspace.

OVC is deferred. It is carried only as an ecosystem-domain placeholder and receives no system registry, module registry, or artifact mapping work until a dedicated OVC direction step is completed and accepted.

## Consequences

- OVIS structural registries may be canonicalized now.
- OVC records remain out of scope for this implementation wave.
- Future OVC structural work must begin with a dedicated direction ADR and an explicit boundary update.

# References

- ADR-008_WORKSPACE_METADATA_ENVELOPE_AND_REGISTRY_GOVERNANCE.md
- ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
- REPO_ROLE_MAP.md
