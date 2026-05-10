---
title: ADR-0001 CLI-First Product Surface
description: Decision to make monorepo-generator a CLI-first product before adding other product surfaces.
status: accepted
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# ADR-0001: CLI-First Product Surface

## Status

Accepted

## Date

2026-05-10

## Decision Owner

Project

## 1. Context

**monorepo-generator** is intended to generate, evolve, and govern production-grade monorepos.

The product needs an initial user-facing surface that can:

- run locally
- create files and directories
- inspect repository state
- validate repository structure
- support interactive and non-interactive workflows
- work in developer terminals
- work in CI environments
- avoid requiring a hosted service
- avoid requiring a paid subscription
- remain useful with or without AI tools

Several product surfaces are possible:

- command-line interface
- library-only package
- hosted SaaS
- local web UI
- desktop app
- editor extension
- framework-specific starter
- GitHub app

The first product surface should support the core product thesis with the least unnecessary platform complexity.

## 2. Problem Statement

We need to decide the initial product surface for **monorepo-generator** so that the product can generate and validate repositories immediately, while preserving local-first usage, inspectability, scriptability, open-source distribution, and future extensibility.

## 3. Decision Drivers

The decision is driven by:

- ability to create and modify local repository files
- ability to run in developer terminals
- ability to run in CI
- ability to support interactive prompts
- ability to support non-interactive flags and config files
- open-source friendliness
- low operational overhead
- no hosted-service dependency
- no AI-provider dependency
- easy installation and execution
- future compatibility with plugins, presets, and adapters
- future compatibility with SaaS or UI surfaces
- implementation simplicity for the first milestone

## 4. Options Considered

### Option A: CLI-First Product Surface

Build the first product surface as a command-line interface.

The CLI would provide commands such as:

```text
monorepo-generator --help
monorepo-generator --version
monorepo-generator init
monorepo-generator validate
````

Future commands may include:

```text
monorepo-generator inspect
monorepo-generator add app
monorepo-generator add package
monorepo-generator add service
monorepo-generator add doc
monorepo-generator adr create
monorepo-generator upgrade plan
monorepo-generator upgrade apply
```

Pros:

* Works directly with local repositories.
* Fits developer workflows.
* Can run in CI.
* Supports scripted and automated usage.
* Supports interactive prompts.
* Supports non-interactive flags.
* Can remain fully open-source.
* Does not require a hosted service.
* Does not require user accounts.
* Does not require paid AI access.
* Can later be wrapped by other product surfaces.
* Encourages explicit command behavior and exit codes.

Cons:

* Requires terminal comfort from users.
* Less visually approachable than a web UI.
* Rich previews and guided workflows require more work.
* Cross-platform shell behavior must be handled carefully.

### Option B: Library-Only Package

Build the first product surface as a library that other tools can import.

Pros:

* Encourages clean core architecture.
* Makes embedding possible.
* Can be reused by future CLI, web, or desktop surfaces.
* Easier to test core logic independently.

Cons:

* Does not directly serve end users.
* Requires another surface before the product is usable.
* Slows down feedback on actual user workflows.
* Makes installation and usage less obvious.
* Does not satisfy the immediate need for repository generation commands.

### Option C: Hosted SaaS First

Build the first product surface as a hosted web application.

Pros:

* Easier onboarding for non-terminal users.
* Could support accounts, templates, dashboards, teams, and monetization.
* Could later manage template registries and remote project policies.

Cons:

* Adds hosting, authentication, billing, database, deployment, and operational burden too early.
* Creates unnecessary dependency on a hosted service.
* Does not naturally write to local repositories without extra integration.
* Increases security and trust complexity.
* Conflicts with the open-source local-first goal for the initial version.
* Slows down the first useful milestone.

### Option D: Local Web UI First

Build a local web interface that runs on the user's machine.

Pros:

* Could provide a nicer guided setup experience.
* Can remain local-first.
* Could later visualize project graphs and upgrade plans.

Cons:

* Requires a server process or local app runtime.
* Adds frontend complexity before core generation is proven.
* Still needs filesystem access and command execution boundaries.
* Harder to use in CI.
* More complex than a CLI for the first milestone.

### Option E: Editor Extension First

Build the first surface as an extension for VS Code or another editor.

Pros:

* Integrates with developer environment.
* Could provide guided workflows and file previews.
* Could later expose repo inspection and upgrade plans visually.

Cons:

* Locks the initial experience to a specific editor.
* Harder to run in CI.
* Adds editor API complexity.
* Does not naturally serve terminal-first users.
* Should depend on a stable core CLI/library rather than define the core product.

### Option F: Framework-Specific Starter First

Build the first product as a specific app starter, such as a Next.js, Hono, SolidJS, Rails, or T3-style starter.

Pros:

* Easier to make immediately concrete.
* Faster to demo.
* Clearer first generated application output.

Cons:

* Conflicts with the broader product goal.
* Risks becoming another framework starter.
* Does not solve the full governed monorepo foundation problem.
* Narrows the product too early.
* Weakens polyglot and adapter-based architecture.

## 5. Decision

The decision is:

> **monorepo-generator shall be CLI-first.**

The first product surface shall be a local command-line interface capable of initializing, validating, and later evolving repositories.

The CLI-first decision does not prevent future surfaces such as:

* library API
* local web UI
* hosted SaaS
* editor extension
* GitHub app
* template registry
* visual project graph explorer

However, those surfaces shall come after the CLI and core generation engine are established.

## 6. Rationale

A CLI is the best first product surface because repository generation is fundamentally a local developer workflow.

The product must create files, inspect directories, validate project structure, and eventually run verification commands. These responsibilities align naturally with terminal-based tooling.

A CLI also supports both primary usage modes:

1. **Interactive local usage**
   A user can run `monorepo-generator init` and answer prompts.

2. **Automated usage**
   A user or CI workflow can run commands with flags or config files.

A CLI-first approach also protects the project from premature platform complexity. Starting with SaaS, a web UI, or an editor extension would force the project to solve hosting, authentication, UI state, deployment, or editor integration before the core generator is proven.

The CLI-first approach keeps the initial milestone focused:

> Generate and verify a minimal governed monorepo foundation.

## 7. Consequences

### 7.1 Positive Consequences

* The product can become useful quickly.
* The project can remain local-first.
* The tool can work without accounts or hosted services.
* The tool can work without AI access.
* The command model can support both humans and automation.
* The CLI can run in CI.
* The CLI can be tested through command-level smoke tests.
* Future surfaces can wrap the same core engine.
* The project avoids premature SaaS and UI complexity.

### 7.2 Negative Consequences

* The first experience is terminal-oriented.
* Non-technical users may find the first version less approachable.
* Rich visual workflows are deferred.
* Cross-platform command behavior must be handled carefully.
* Installation and packaging must be designed well.

### 7.3 Neutral Consequences

* The core engine should still be separated from CLI parsing.
* A future library API remains desirable.
* A future hosted product remains possible.
* A future editor extension remains possible.
* A future visual UI remains possible.

## 8. Implementation Notes

The implementation should reflect this decision by creating a CLI-oriented project structure.

Expected future areas:

```text
packages/cli/
packages/cli/src/cli/
packages/cli/src/commands/
packages/cli/src/engine/
packages/cli/src/presets/
packages/cli/src/filesystem/
packages/cli/src/manifest/
packages/cli/src/validation/
```

The first CLI commands should be:

```text
monorepo-generator --help
monorepo-generator --version
monorepo-generator init
monorepo-generator validate
```

The CLI layer should not contain all business logic.

The implementation should separate:

```text
CLI parsing
command handlers
configuration normalization
generation planning
file writing
manifest writing
validation
terminal output
```

This separation ensures that a future library API, web UI, or editor extension can reuse the same core engine.

## 9. Alternatives Rejected

### 9.1 Hosted SaaS First

Rejected because it introduces too much operational and product complexity before the generator core exists.

The product may later have SaaS features, but SaaS should not be required for the initial open-source tool.

### 9.2 Web UI First

Rejected because the product needs reliable local repository generation before it needs visual interaction.

A web UI may later wrap the core engine or CLI workflows.

### 9.3 Editor Extension First

Rejected because it would create editor lock-in and reduce CI/scriptability.

An editor extension may later integrate with the CLI.

### 9.4 Library-Only First

Rejected as the primary surface because it does not directly serve users.

However, the implementation should still be modular enough to expose library-like internals later.

### 9.5 Framework Starter First

Rejected because **monorepo-generator** is intended to be a governed monorepo foundation tool, not a starter for one framework.

Framework-specific outputs should eventually be implemented as presets or adapters.

## 10. Compatibility

This decision is compatible with:

* local development
* CI usage
* Git-based workflows
* package manager workflows
* generated repository validation
* future plugin architecture
* future adapter architecture
* future SaaS extension
* future editor extension
* future local web UI
* future AI-assisted workflows

The decision intentionally avoids requiring:

* a hosted backend
* user accounts
* OAuth
* billing
* paid AI subscription
* remote code execution
* editor-specific APIs

## 11. Security Considerations

A CLI that writes files must be conservative.

Security implications:

* The CLI must not silently overwrite user files.
* The CLI must not execute remote code by default.
* The CLI must not generate real secrets.
* The CLI must not hide file writes from the user.
* The CLI should clearly report generated files.
* The CLI should support future dry-run workflows.
* The CLI should treat remote template support as a separate trust-boundary decision.

This ADR supports a local-first security posture.

## 12. Verification

This decision can be verified when the project includes:

* a CLI package
* a root command
* help output
* version output
* an `init` command
* a `validate` command
* command tests or smoke tests
* documentation describing CLI usage

Future verification commands may include:

```bash
bun run typecheck
bun run test
bun run build
bun run verify
```

Decision-specific verification:

* Running the CLI with `--help` prints usage.
* Running the CLI with `--version` prints a version.
* Running the CLI with `init` generates a repository foundation.
* Running the CLI with `validate` checks a generated repository.
* The core generation engine is not tightly coupled to terminal output.

## 13. Related Requirements

Related SRS requirements:

* FR-001: Provide a CLI Entry Point
* FR-002: Provide Help Output
* FR-003: Provide Version Output
* FR-004: Initialize a New Repository Foundation
* FR-005: Support Interactive Initialization
* FR-006: Support Non-Interactive Initialization
* FR-029: Validate Generated Repository Structure
* FR-031: Support Machine-Readable Validation Output
* NFR-001: Clear Command Output
* NFR-002: Useful Errors
* NFR-005: Modular Architecture
* NFR-006: Testability

## 14. Related Documents

Related documents:

* docs/planning/00-vision/product-inception-brief.md
* docs/planning/01-product/product-charter.md
* docs/planning/02-requirements/software-requirements-specification.md
* docs/planning/03-architecture/architecture-overview.md
* docs/planning/04-decisions/adr-index.md
* docs/planning/04-decisions/adr-template.md

## 15. Follow-Up Work

* [ ] Create ADR-0002 for the TypeScript and Bun implementation baseline.
* [ ] Decide the CLI framework or parser.
* [ ] Create the initial root `package.json`.
* [ ] Create the initial CLI package structure.
* [ ] Add help output.
* [ ] Add version output.
* [ ] Add the first `init` command.
* [ ] Add the first `validate` command.
* [ ] Add CLI smoke tests.
* [ ] Add verification scripts.
