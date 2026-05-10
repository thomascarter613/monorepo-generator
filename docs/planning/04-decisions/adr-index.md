---
title: Architecture Decision Record Index
description: Index of architecture decision records for monorepo-generator.
status: draft
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# Architecture Decision Record Index

## 1. Purpose

This document is the index for Architecture Decision Records for **monorepo-generator**.

Architecture Decision Records document significant product, technical, architectural, operational, and governance decisions that affect the long-term shape of the project.

The purpose of this index is to make decisions discoverable, ordered, and auditable.

## 2. ADR Standard

Each ADR should record:

- the decision being made
- the context that led to the decision
- the options considered
- the selected decision
- the rationale
- the consequences
- related requirements
- related documents
- follow-up work

ADRs should be concise enough to remain usable, but complete enough to preserve the reasoning behind important decisions.

## 3. ADR Status Values

Allowed ADR statuses are:

| Status | Meaning |
|---|---|
| Proposed | The decision is drafted but not yet accepted. |
| Accepted | The decision is approved and should guide implementation. |
| Superseded | The decision has been replaced by a later ADR. |
| Deprecated | The decision is no longer recommended but remains historically relevant. |
| Rejected | The decision was considered and explicitly rejected. |

## 4. ADR Naming Convention

ADR files should use this naming pattern:

```text
ADR-0001-short-decision-name.md
ADR-0002-short-decision-name.md
ADR-0003-short-decision-name.md
````

Rules:

* Use four-digit numbering.
* Use lowercase kebab-case after the ADR number.
* Do not renumber accepted ADRs.
* If a decision is replaced, create a new ADR and mark the old ADR as superseded.
* Keep ADRs in this directory unless a future ADR changes the documentation layout.

## 5. Initial ADR Queue

The initial ADR queue is:

| ADR      | Title                                       | Status  | Purpose                                                                                  |
| -------- | ------------------------------------------- | ------- | ---------------------------------------------------------------------------------------- |
| ADR-0001 | CLI-First Product Surface                   | Planned | Decide that the first product surface is a local CLI.                                    |
| ADR-0002 | TypeScript and Bun Implementation Baseline  | Planned | Decide the initial implementation language, runtime, package manager, and project setup. |
| ADR-0003 | Repository Planning Before File Writing     | Planned | Decide that generation must produce a plan before writing files.                         |
| ADR-0004 | Default Governed Monorepo Preset            | Planned | Decide the first generated repository preset and its baseline structure.                 |
| ADR-0005 | Metadata Directory and Manifest Location    | Planned | Decide where generated repository metadata should live.                                  |
| ADR-0006 | Non-Destructive File Writing Policy         | Planned | Decide the default write behavior and overwrite conflict model.                          |
| ADR-0007 | Preset and Adapter Architecture             | Planned | Decide how presets and adapters contribute generated files and validation rules.         |
| ADR-0008 | Validation and Repo Contract Model          | Planned | Decide how generated repositories are validated.                                         |
| ADR-0009 | Documentation and ADR Structure             | Planned | Decide the project documentation layout and generated documentation conventions.         |
| ADR-0010 | AI-Native but AI-Optional Architecture      | Planned | Decide how the product supports AI workflows without requiring AI.                       |
| ADR-0011 | Package Versioning and Publishing Strategy  | Planned | Decide initial support for Changesets, Rush, or future package governance adapters.      |
| ADR-0012 | Task Orchestration Adapter Strategy         | Planned | Decide how Turborepo, moonrepo, Nx, Pants, or other orchestrators are introduced.        |
| ADR-0013 | Template Lifecycle and Upgrade Strategy     | Planned | Decide the approach for future Copier-style template updates and repository evolution.   |
| ADR-0014 | Plugin Architecture Boundary                | Planned | Decide what is core versus plugin-provided behavior.                                     |
| ADR-0015 | Testing and Verification Strategy           | Planned | Decide project testing, generated-repo testing, and verification standards.              |
| ADR-0016 | CI Strategy                                 | Planned | Decide the initial CI workflow and quality gates.                                        |
| ADR-0017 | Security and Remote Template Trust Boundary | Planned | Decide constraints for remote templates, generated secrets, and external execution.      |
| ADR-0018 | Project Naming and Package Naming Strategy  | Planned | Decide whether the working name remains monorepo-generator or changes.                   |

## 6. Accepted ADRs

| ADR | Title | Status | File |
|---|---|---|---|
| ADR-0001 | CLI-First Product Surface | Accepted | ADR-0001-cli-first-product-surface.md |

## 7. Superseded ADRs

No ADRs have been superseded yet.

## 8. Rejected ADRs

No ADRs have been rejected yet.

## 9. Decision Principles

Architecture decisions should follow these principles:

1. Prefer generated output that is readable and maintainable.
2. Prefer deterministic behavior over hidden magic.
3. Prefer explicit repository contracts over undocumented conventions.
4. Prefer safe file writing over convenience.
5. Prefer modular seams for presets, adapters, and plugins.
6. Prefer composing best-of-breed tools instead of replacing all tools.
7. Prefer polyglot-ready architecture over JavaScript-only assumptions.
8. Prefer AI-native support without AI lock-in.
9. Prefer verification before expansion.
10. Prefer small accepted decisions over large ambiguous decisions.

## 10. ADR Workflow

The normal ADR workflow is:

```text
1. Identify a significant decision.
2. Create an ADR from the template.
3. Mark status as Proposed.
4. Document context, options, decision, and consequences.
5. Review against product charter, SRS, and architecture overview.
6. Accept, reject, or revise the ADR.
7. Update this index.
8. Implement according to accepted decisions.
```

## 11. Immediate Next ADR

The next ADR should be:

```text
docs/planning/04-decisions/ADR-0001-cli-first-product-surface.md
```

This ADR should decide that **monorepo-generator** begins as a CLI-first product rather than a web app, library-only package, hosted SaaS, editor extension, or framework-specific starter.
