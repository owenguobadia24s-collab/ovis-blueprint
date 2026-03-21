---
id: DOC-STRUCTURE-0002
title: OVIS Module Taxonomy
type: DOC
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: MODULE_TAXONOMY.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/DOC-STRUCTURE-0002.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
---

# Purpose

Summarize the initial canonical OVIS module taxonomy.

# Scope

This document covers the first approved module set for the current OVIS systems and the metadata governance package that records them.

# Content

The kernel system currently owns the active runtime-facing modules: `runtime_gateway`, `work_object_registry`, `branch_continuity`, `branch_compaction`, `event_log`, `planning_execution_loop`, `review_gate`, and `operator_surface`.

The integration system owns `tool_gateway` and the planned `bridge_dispatch` module.

The retrieval and memory system owns the planned `retrieval_layer` and `system_memory` modules.

The metadata system owns the governance modules `meta_header`, `meta_schema_policy`, `meta_registry`, `meta_reconcile`, and `meta_scaffold`.

Detailed owned paths, lifecycle, basis, and repo participation are canonical in `REGISTRIES/modules/*.yaml`.

# References

- REGISTRIES/modules/MOD-META-REGISTRY-0001.yaml
- REGISTRIES/modules/MOD-RUNTIME-GATEWAY-0001.yaml
- ADR/ADR-011_MODULE_INTRODUCTION_AND_TAXONOMY_GOVERNANCE.md
