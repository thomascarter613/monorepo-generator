---
title: ADR-0002 TypeScript and Bun Implementation Baseline
description: Decision to use TypeScript and Bun as the initial implementation baseline for monorepo-generator.
status: accepted
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# ADR-0002: TypeScript and Bun Implementation Baseline

## Status

Accepted

## Date

2026-05-10

## Decision Owner

Project

## 1. Context

**monorepo-generator** is a CLI-first tool for generating, evolving, and governing production-grade monorepos.

The project needs an initial implementation baseline before code is added.

The implementation baseline must decide:

- primary implementation language
- package manager
- runtime expectations
- project structure direction
- build and test approach
- generated repository compatibility assumptions
- how strongly the first version should lean into JavaScript and TypeScript tooling

The tool is intended to support polyglot monorepos over time, but the generator itself needs a practical implementation stack that supports fast iteration, strong typing, good ecosystem access, and modern CLI development.

The current working preference is to use **TypeScript** and **Bun**.

## 2. Problem Statement

We need to decide the initial implementation language and package manager/runtime baseline so that **monorepo-generator** can be built quickly, tested reliably, and structured for future adapters, while avoiding premature lock-in to a JavaScript-only generated repository model.

## 3. Decision Drivers

The decision is driven by:

- fast implementation speed
- strong type safety
- CLI ecosystem compatibility
- testability
- maintainability
- developer familiarity
- support for JSON-heavy manifest and config workflows
- compatibility with monorepo tooling
- ability to generate JavaScript/TypeScript repositories well
- ability to preserve future polyglot support
- simple package scripts
- fast local feedback loops
- low initial operational complexity
- future compatibility with npm package distribution

## 4. Options Considered

### Option A: TypeScript with Bun

Use TypeScript as the implementation language and Bun as the package manager/runtime for development scripts, dependency management, tests, and command execution where appropriate.

Pros:

- Strong type safety.
- Excellent fit for CLI configuration, JSON manifests, templates, and adapters.
- Fast package installation and script execution.
- Good fit for monorepo tooling.
- Natural compatibility with generated JavaScript/TypeScript workspaces.
- Easy to publish packages to npm later.
- Good developer ergonomics.
- Supports rapid iteration.
- Aligns with the project’s likely first generated repository targets.
- Can still generate polyglot repositories.

Cons:

- Bun is newer than npm, pnpm, and yarn.
- Some ecosystem packages may still assume Node-specific behavior.
- Cross-platform behavior should be verified carefully.
- The implementation language may bias early design toward JS/TS if not actively controlled.

### Option B: TypeScript with pnpm

Use TypeScript as the implementation language and pnpm as the package manager.

Pros:

- Mature package manager.
- Excellent monorepo workspace support.
- Strong compatibility with the Node ecosystem.
- Widely used in serious TypeScript monorepos.
- Less runtime novelty than Bun.

Cons:

- Slower local workflow compared with Bun in many cases.
- Adds separation between package manager and runtime/test runner.
- Does not align with the current project preference to use Bun.
- Still primarily JS/TS-oriented.

### Option C: Go

Use Go as the implementation language.

Pros:

- Excellent CLI language.
- Produces single binaries.
- Strong standard library.
- Good performance.
- Good cross-platform distribution story.
- Less tied to the JavaScript ecosystem.

Cons:

- Slower to build template-rich JS/TS ecosystem integrations.
- Less direct compatibility with npm package distribution.
- More friction for generating TypeScript-native package metadata and scripts.
- Less aligned with the current development preference.
- Would require a separate approach for JS package ecosystem integration.

### Option D: Rust

Use Rust as the implementation language.

Pros:

- Excellent performance.
- Strong safety guarantees.
- Good CLI tooling.
- Strong binary distribution story.
- Good fit for long-term robust tooling.

Cons:

- Slower initial development.
- Higher implementation complexity.
- More friction for rapid product iteration.
- Less direct integration with JS/TS package ecosystem.
- Not necessary for the first milestone.

### Option E: Python

Use Python as the implementation language.

Pros:

- Strong scripting and templating ecosystem.
- Familiar project generation patterns.
- Good fit for Copier/Cookiecutter-inspired workflows.
- Easy to prototype.

Cons:

- Packaging and distribution can be more complicated for JS/TS users.
- Less natural fit for npm-oriented package distribution.
- Weaker static typing by default.
- Less aligned with generated JS/TS monorepo workflows.
- Runtime environment assumptions can vary across machines.

### Option F: Shell-First Implementation

Use shell scripts as the initial implementation.

Pros:

- Very simple to start.
- Good for local automation.
- Easy to inspect.
- Minimal dependencies.

Cons:

- Poor long-term maintainability.
- Difficult cross-platform support.
- Weak type safety.
- Harder to test properly.
- Poor fit for complex generation plans, manifests, adapters, and validation.
- Not appropriate for a serious generator product.

## 5. Decision

The decision is:

> **monorepo-generator shall use TypeScript and Bun as the initial implementation baseline.**

The project will use:

```text
Implementation language: TypeScript
Initial package manager: Bun
Initial script runner: Bun
Initial test runner: Bun test or Vitest, to be finalized later
Initial package distribution target: npm-compatible package distribution
Initial generated repository focus: governed monorepo foundation with JS/TS support first, but polyglot-ready architecture
````

This decision does not mean generated repositories must always use Bun.

Generated repositories may eventually support:

* Bun
* pnpm
* npm
* yarn
* Go modules
* Python package managers
* Rust Cargo workspaces
* Maven or Gradle
* other language ecosystems

Bun is the implementation baseline for this tool, not a permanent restriction on generated repository targets.

## 6. Rationale

TypeScript is the best initial implementation language because **monorepo-generator** will heavily use structured configuration, manifests, file plans, validation rules, presets, adapters, and generated text.

Strong static typing will help keep these models explicit.

Bun is the best initial package manager and local workflow baseline because it provides fast installation, fast script execution, and a compact development experience.

This choice supports the near-term goal:

> Build a minimal TypeScript CLI that can generate and validate a governed monorepo foundation.

The TypeScript and Bun baseline also aligns with likely early integrations:

* Turborepo
* Changesets
* GitHub Actions for JS/TS workflows
* package.json scripts
* JSON manifests
* TypeScript-based validation
* npm-compatible CLI distribution

At the same time, this ADR explicitly prevents a dangerous misunderstanding:

> Using TypeScript and Bun for the generator does not mean the generated monorepos are JavaScript-only.

The architecture must remain polyglot-ready through presets, adapters, and generated repo contracts.

## 7. Consequences

### 7.1 Positive Consequences

* The project can move quickly.
* The codebase benefits from type safety.
* Manifest and config models can be strongly typed.
* CLI implementation can use the modern JS/TS ecosystem.
* Generated JS/TS monorepo support will be straightforward.
* Bun provides fast install and script workflows.
* The project can later publish an npm-compatible CLI package.
* Tests can run quickly.
* The implementation is accessible to many web/tooling developers.

### 7.2 Negative Consequences

* Bun compatibility must be watched carefully.
* Some packages may assume Node behavior.
* The project may need Node compatibility checks before broad distribution.
* Early design may accidentally overfit to JS/TS unless polyglot boundaries are enforced.
* Users who do not use Bun may need generated-repository package-manager options later.

### 7.3 Neutral Consequences

* The CLI can still generate repos that use other package managers.
* The project may later add packaged binaries.
* The project may later add Go or Rust helper binaries if needed.
* The project may later support Node-based execution if required for distribution.
* The implementation can still use POSIX shell scripts for verification where appropriate.

## 8. Implementation Notes

The initial repository should move toward this structure:

```text
monorepo-generator/
├── packages/
│   └── cli/
│       ├── src/
│       ├── test/
│       ├── package.json
│       └── tsconfig.json
├── tools/
│   └── scripts/
├── package.json
├── tsconfig.json
├── bun.lock
└── docs/
```

Initial root scripts should eventually include:

```json
{
  "scripts": {
    "format": "biome format --write .",
    "format:check": "biome format .",
    "lint": "biome lint .",
    "typecheck": "tsc -p tsconfig.json --noEmit",
    "test": "bun test",
    "build": "bun run --filter '*' build",
    "verify": "bash tools/scripts/verify.sh"
  }
}
```

The exact scripts may be refined during implementation.

The first implementation should include:

* root `package.json`
* root `tsconfig.json`
* CLI package
* TypeScript source files
* TypeScript build settings
* test setup
* verification script
* CI workflow

The implementation should avoid assuming that every generated repository must use Bun.

Generated repository package manager selection should be represented in normalized config and manifest data.

## 9. Alternatives Rejected

### 9.1 Go First

Go is a strong CLI implementation option, but it is rejected for the first implementation because TypeScript better fits the expected early ecosystem integrations, JSON-heavy configuration, and npm-compatible distribution path.

Go may be reconsidered later for performance-critical helper binaries or packaged distribution.

### 9.2 Rust First

Rust is rejected for the first implementation because it would slow early product iteration.

Rust may be reconsidered later for performance-sensitive or safety-sensitive components.

### 9.3 Python First

Python is rejected for the first implementation because the product is likely to interact heavily with JS/TS package metadata and npm-compatible distribution early.

Python remains an important generated-repository target and may influence future template functionality.

### 9.4 Shell First

Shell is rejected as the main implementation language because the product requires typed models, safe planning, validation, adapter boundaries, tests, and maintainable command behavior.

Shell scripts may still be generated or used for verification where appropriate.

### 9.5 TypeScript with pnpm First

TypeScript with pnpm is a strong alternative, but Bun is selected because it aligns with the current preferred workflow and provides faster local iteration.

pnpm should remain a future generated-repository option.

## 10. Compatibility

This decision is compatible with:

* CLI-first architecture
* npm-compatible package distribution
* generated TypeScript workspaces
* generated JavaScript workspaces
* Turborepo integration
* Changesets integration
* GitHub Actions integration
* JSON manifest files
* Markdown documentation generation
* future adapters
* future plugins
* future polyglot generated repositories

This decision must remain compatible with future support for:

* pnpm-generated repositories
* npm-generated repositories
* yarn-generated repositories
* Go services
* Python packages
* Rust crates
* Java projects
* Docker and infrastructure code

The implementation must avoid hard-coding Bun assumptions into all generated repository contracts.

## 11. Security Considerations

This decision has the following security implications:

* Dependencies must be reviewed before adoption.
* Package scripts should be explicit and inspectable.
* The CLI must not execute remote scripts by default.
* Generated repositories must not include real secrets.
* Bun lifecycle script behavior should be understood before adding dependencies.
* CI should use deterministic install behavior where possible.
* Future template execution must have explicit trust boundaries.

Using Bun and TypeScript does not remove the need for safe file-writing policy, manifest validation, and non-destructive generation.

## 12. Verification

This decision can be verified when the repository includes:

* root `package.json`
* Bun lockfile after dependency installation
* TypeScript configuration
* CLI package TypeScript source
* typecheck command
* test command
* build command
* verification command

Future verification commands:

```bash
bun install
bun run typecheck
bun run test
bun run build
bun run verify
```

Decision-specific verification:

* `bun install` succeeds.
* `bun run typecheck` succeeds.
* TypeScript source compiles.
* CLI package exists.
* Generated repository package manager is configurable rather than hard-coded everywhere.
* Documentation states that Bun is the implementation baseline, not the only generated-repo target.

## 13. Related Requirements

Related SRS requirements:

* FR-001: Provide a CLI Entry Point
* FR-002: Provide Help Output
* FR-003: Provide Version Output
* FR-004: Initialize a New Repository Foundation
* FR-006: Support Non-Interactive Initialization
* FR-013: Generate Package Metadata
* FR-022: Generate Basic Task Scripts
* FR-024: Support Future Orchestration Adapters
* NFR-003: Deterministic Output
* NFR-005: Modular Architecture
* NFR-006: Testability
* NFR-007: Cross-Platform Intent

## 14. Related Documents

Related documents:

* docs/planning/00-vision/product-inception-brief.md
* docs/planning/01-product/product-charter.md
* docs/planning/02-requirements/software-requirements-specification.md
* docs/planning/03-architecture/architecture-overview.md
* docs/planning/04-decisions/adr-index.md
* docs/planning/04-decisions/ADR-0001-cli-first-product-surface.md

## 15. Follow-Up Work

* [ ] Create ADR-0003 for repository planning before file writing.
* [ ] Decide CLI framework or parser.
* [ ] Create root `package.json`.
* [ ] Create root `tsconfig.json`.
* [ ] Add Bun lockfile.
* [ ] Create `packages/cli`.
* [ ] Add TypeScript source layout.
* [ ] Add typecheck script.
* [ ] Add test script.
* [ ] Add build script.
* [ ] Add verification script.
* [ ] Add CI workflow for Bun and TypeScript.
