---
title: ADR-0000 Decision Title
description: Architecture decision record template for monorepo-generator.
status: template
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# ADR-0000: Decision Title

## Status

Proposed

## Date

YYYY-MM-DD

## Decision Owner

Project

## 1. Context

Describe the situation that requires a decision.

Include:

* the product need
* the technical pressure
* the relevant constraints
* the affected users
* the affected parts of the system
* the reason this decision matters now

## 2. Problem Statement

State the decision problem clearly.

Use this form when helpful:

> We need to decide [decision area] so that [desired outcome], while preserving [constraints].

## 3. Decision Drivers

List the criteria that matter for this decision.

Examples:

* generated repository quality
* user control
* implementation simplicity
* long-term maintainability
* polyglot support
* plugin extensibility
* deterministic output
* verification support
* ecosystem compatibility
* open-source compatibility
* avoidance of vendor lock-in
* AI-native but AI-optional workflows

## 4. Options Considered

### Option A: Name

Description.

Pros:

* Pro.

Cons:

* Con.

### Option B: Name

Description.

Pros:

* Pro.

Cons:

* Con.

### Option C: Name

Description.

Pros:

* Pro.

Cons:

* Con.

## 5. Decision

The decision is:

> State the selected decision here.

## 6. Rationale

Explain why this option was selected.

The rationale should connect directly to the decision drivers.

## 7. Consequences

### 7.1 Positive Consequences

* Positive consequence.

### 7.2 Negative Consequences

* Negative consequence.

### 7.3 Neutral Consequences

* Neutral consequence.

## 8. Implementation Notes

Describe how this decision should be reflected in implementation.

Include:

* files likely affected
* modules likely affected
* commands likely affected
* generated repository impact
* validation impact
* documentation impact

## 9. Alternatives Rejected

Document alternatives that were considered but not selected.

This section is important because rejected options often become confusing later.

## 10. Compatibility

Describe compatibility implications.

Consider:

* existing generated repositories
* future generated repositories
* package managers
* operating systems
* external tools
* future adapters
* future plugins

## 11. Security Considerations

Describe any security implications.

Consider:

* file system safety
* generated secrets
* remote code execution
* dependency trust
* template trust
* CI behavior
* user data
* credentials

## 12. Verification

Describe how this decision can be verified.

Examples:

```bash
bun run typecheck
bun run test
bun run verify
```

Decision-specific verification:

* Expected file exists.
* Expected command works.
* Expected validation rule passes.
* Expected generated output is present.
* Expected unsafe behavior is rejected.

## 13. Related Requirements

List related SRS requirements.

Examples:

* FR-001
* FR-004
* FR-016
* NFR-003
* NFR-005

## 14. Related Documents

List related documents.

Examples:

* docs/planning/00-vision/product-inception-brief.md
* docs/planning/01-product/product-charter.md
* docs/planning/02-requirements/software-requirements-specification.md
* docs/planning/03-architecture/architecture-overview.md

## 15. Follow-Up Work

List follow-up tasks.

* [ ] Follow-up task.
* [ ] Follow-up task.
* [ ] Follow-up task.
