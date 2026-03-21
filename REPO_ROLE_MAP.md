---
id: DOC-STRUCTURE-0003
title: OVIS Repo Role Map
type: DOC
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: REPO_ROLE_MAP.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/DOC-STRUCTURE-0003.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
---

# Purpose

Describe the canonical roles of the core OVIS repos in the structural model.

# Scope

This map covers the currently governed core repos only: `ovis-blueprint`, `ovis-runtime`, and `ovis-knowledge`.

# Content

| repo_id | repo_name | role | authority scope |
| --- | --- | --- | --- |
| `REPO-BLUEPRINT-0001` | `ovis-blueprint` | design_authority | canonical doctrine, ADRs, policies, registries, taxonomy |
| `REPO-RUNTIME-0001` | `ovis-runtime` | implementation_authority | executable runtime, metadata tooling, operator surfaces |
| `REPO-KNOWLEDGE-0001` | `ovis-knowledge` | derived_knowledge_authority | overlays, summaries, derived evidence and future memory surfaces |

`ovis-notion-bootstrap` remains outside the current governed repo map and is treated as transitional evidence input rather than a canonical structural authority.

# References

- REGISTRIES/repos/REPO-BLUEPRINT-0001.yaml
- REGISTRIES/repos/REPO-RUNTIME-0001.yaml
- ADR/ADR-012_OVIS_OVC_ECOSYSTEM_BOUNDARY.md
