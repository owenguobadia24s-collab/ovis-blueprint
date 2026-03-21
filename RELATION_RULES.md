---
id: DOC-STRUCTURE-0004
title: OVIS Relation Rules
type: DOC
status: active
authority: canonical
version: '0.1'
layer: blueprint
domain: structure
repo: ovis-blueprint
path: RELATION_RULES.md
owner: Owen Vitae
created: '2026-03-21'
last_updated: '2026-03-21'
registry: ovis-blueprint/REGISTRIES/entries/DOC-STRUCTURE-0004.yaml
module_id: MOD-META-REGISTRY-0001
module_slug: meta_registry
system_id: SYS-METADATA-0001
system_slug: metadata_system
related_module_ids:
  - MOD-META-SCHEMA-POLICY-0001
---

# Purpose

Define how structural relations are classified and recorded.

# Scope

This document governs relation authority classes and recording locations for the OVIS structural model.

# Content

Primary relations are authoritative:

- artifact primary module
- module belongs to system

Secondary relations are overlays:

- artifact related module
- system spans repo
- module depends on module
- system depends on system
- ADR governs target
- CJ implements target

Primary relations must fail closed on inconsistency. Secondary relations may evolve without redefining identity.

# References

- REGISTRIES/relations/module_dependencies.yaml
- REGISTRIES/relations/system_dependencies.yaml
- ADR/ADR-010_SYSTEM_MODULE_REPO_REGISTRY_MODEL.md
