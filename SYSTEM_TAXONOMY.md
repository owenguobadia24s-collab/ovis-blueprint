---
id: DOC-STRUCTURE-0001
title: OVIS System Taxonomy
type: DOC
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: SYSTEM_TAXONOMY.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/DOC-STRUCTURE-0001.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
---

# Purpose

Summarize the initial canonical OVIS system taxonomy in narrative form.

# Scope

This document covers the currently approved OVIS-only system set and its authority patterns. OVC remains intentionally deferred.

# Content

| system_id | slug | title | status | basis | primary participating repos |
| --- | --- | --- | --- | --- | --- |
| `SYS-KERNEL-0001` | `ovis_kernel` | OVIS Cognitive Infrastructure Kernel | active | doctrine-approved, evidenced | blueprint, runtime, knowledge |
| `SYS-METADATA-0001` | `metadata_system` | OVIS Metadata System | emerging | approved direction, proposed | blueprint, runtime, knowledge |
| `SYS-INTEGRATION-0001` | `integration_bridge` | OVIS Integration and Bridge System | emerging | doctrine-approved, evidenced | blueprint, runtime, knowledge |
| `SYS-RETRIEVAL-MEMORY-0001` | `retrieval_memory` | OVIS Retrieval and Memory System | planned | doctrine-approved, proposed | blueprint, runtime, knowledge |

The kernel is the current active system boundary for runtime mediation, work objects, branch continuity, compaction, orchestration, approval, and audit. The other systems provide the bounded expansion surfaces required for metadata governance, integration mediation, and future retrieval/memory continuity.

# References

- REGISTRIES/systems/SYS-KERNEL-0001.yaml
- REGISTRIES/systems/SYS-METADATA-0001.yaml
- ADR/ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
