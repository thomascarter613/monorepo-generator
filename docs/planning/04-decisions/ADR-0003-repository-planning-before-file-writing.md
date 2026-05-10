---
title: ADR-0003 Repository Planning Before File Writing
description: Decision to require generation planning before writing files in monorepo-generator.
status: accepted
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# ADR-0003: Repository Planning Before File Writing

## Status

Accepted

## Date

2026-05-10

## Decision Owner

Project

## 1. Context

**monorepo-generator** is intended to generate, evolve, and govern production-grade monorepos.

The tool will create and modify repository files, including:

- README files
- package metadata
- documentation structures
- ADR files
- CI workflows
- task scripts
- generated app/package/service directories
- repository manifests
- validation scripts
- future adapter configuration
- future upgrade changes

A repository generator can easily damage user work if it writes files directly without first understanding what it intends to do.

The product therefore needs a clear architectural rule for how generation is performed.

The central question is whether commands should write files immediately as they execute, or whether they should first produce a complete generation plan that can be validated, inspected, tested, dry-run, and safely executed.

## 2. Problem Statement

We need to decide whether **monorepo-generator** should write files directly during command execution or first create an explicit generation plan so that repository changes are deterministic, inspectable, testable, conflict-aware, and safe.

## 3. Decision Drivers

The decision is driven by:

- file system safety
- deterministic generation
- user trust
- ability to detect conflicts before writing
- future dry-run support
- future upgrade planning
- future diff previews
- testability
- auditability
- support for non-destructive writes
- support for generated repository validation
- ability to compose presets and adapters
- ability to reason about generated output before applying it
- compatibility with CI and automation
- compatibility with future AI-assisted workflows

## 4. Options Considered

### Option A: Write Files Directly During Command Execution

The command handler would immediately create directories and write files as it processes user input.

Pros:

- Simple to implement initially.
- Fewer internal data structures.
- Faster to prototype.
- Easy for small generators.

Cons:

- Harder to detect all conflicts before modifying the repository.
- Harder to implement dry-run behavior.
- Harder to test generation logic without touching the filesystem.
- Harder to build future upgrade planning.
- Harder to provide useful summaries of planned changes.
- Harder to audit what happened.
- Encourages command handlers to mix parsing, planning, rendering, and writing.
- Increases risk of partial writes leaving the repository in an unclear state.

### Option B: Build an Explicit Generation Plan Before Writing

The command handler would normalize input, resolve a preset, collect adapter contributions, and produce a generation plan before writing files.

The generation plan would describe:

```text
target root
project name
selected preset
selected package manager
selected adapters
directories to create
files to create
files that already match
conflicts
manifest content
validation expectations
summary output
````

Pros:

* Supports conflict detection before writes.
* Supports future dry-run mode.
* Supports future diff preview mode.
* Supports future upgrade planning.
* Supports better testing.
* Supports better validation.
* Supports clear command summaries.
* Supports audit logging.
* Supports non-destructive file policies.
* Keeps command handlers thinner.
* Makes presets and adapters easier to compose.
* Makes generated output easier to reason about.

Cons:

* Requires more up-front design.
* Adds internal data structures.
* May feel heavier than direct file writing for the first implementation.
* Requires discipline to keep plan models simple and maintainable.

### Option C: Hybrid Direct Write with Minimal Prechecks

The command would check for obvious conflicts and then write files directly.

Pros:

* Simpler than a full plan model.
* Safer than completely direct writes.
* May be enough for a small initial generator.

Cons:

* Still lacks a complete representation of intended changes.
* Dry-run and upgrade planning become harder later.
* Adapter composition becomes less clean.
* Tests still need more filesystem interaction.
* Conflict behavior can become inconsistent across generators.
* Harder to produce reliable summaries and audit trails.

### Option D: External Template Engine Owns Planning

The product would delegate planning to an external template engine, such as Copier, Cookiecutter, or another template renderer.

Pros:

* Reduces custom implementation.
* Could reuse mature template behavior.
* May provide prompt and rendering support.

Cons:

* Does not fully control monorepo-specific repo contracts.
* Does not naturally model adapters, validation expectations, or future upgrade planning in the desired way.
* May couple the product too tightly to a template engine.
* Still requires product-specific safety and manifest behavior.
* Does not eliminate the need for a first-class internal repo change model.

## 5. Decision

The decision is:

> **monorepo-generator shall build an explicit generation plan before writing repository files.**

Generation commands shall follow this architectural flow:

```text
1. Receive command input.
2. Normalize configuration.
3. Resolve target directory.
4. Resolve preset.
5. Resolve adapters.
6. Build generation plan.
7. Detect conflicts.
8. Report or execute the plan according to write policy.
9. Write files only after the plan is valid.
10. Write or update manifest metadata.
11. Validate or recommend validation.
12. Print a summary.
```

The initial implementation may keep the generation plan simple, but it must preserve the architectural boundary:

> planning happens before writing.

## 6. Rationale

The product’s long-term value depends on trust.

A generator that writes files directly may be fast to build, but it becomes dangerous and difficult to evolve once the tool supports:

* existing repositories
* app generation
* package generation
* service generation
* generated CI workflows
* adapter configuration
* upgrade commands
* repository validation
* template synchronization
* user-reviewed changes

An explicit generation plan gives the product a durable internal model for repository changes.

It allows the system to answer critical questions before touching user files:

* What will be created?
* What already exists?
* What conflicts?
* What will be skipped?
* What metadata will be recorded?
* What validation should pass afterward?
* What adapter contributed each file?
* What preset required each directory?

This design directly supports the future of the product as a repository evolution and governance tool rather than a one-time starter template.

## 7. Consequences

### 7.1 Positive Consequences

* File writes become safer.
* Conflicts can be detected before mutation.
* Dry-run support becomes straightforward.
* Future diff previews become easier.
* Future upgrade planning becomes possible.
* Tests can assert plans without writing files.
* Presets and adapters can contribute to a shared plan.
* Manifest generation can be derived from the plan.
* Validation expectations can be derived from the plan.
* Command summaries can be more accurate.
* Audit logging can later record planned and applied changes.

### 7.2 Negative Consequences

* The initial implementation requires additional types and structure.
* The project must maintain generation plan models.
* Contributors must learn the planning model.
* Very small file generators may feel more verbose.
* Poorly designed plan structures could become cumbersome.

### 7.3 Neutral Consequences

* The first version can start with a minimal plan model.
* The file writer can remain simple at first.
* The plan does not need to support every future feature immediately.
* External template engines may still be used later.
* Adapters may initially be simple contributors rather than full plugins.

## 8. Implementation Notes

The implementation should introduce explicit concepts such as:

```text
GenerationPlan
GeneratedFile
GeneratedDirectory
GenerationConflict
WritePolicy
PlanResult
PlanSummary
```

A conceptual TypeScript model may look like:

```ts
export interface GenerationPlan {
  readonly targetRoot: string;
  readonly projectName: string;
  readonly preset: string;
  readonly packageManager: string;
  readonly adapters: readonly string[];
  readonly directories: readonly GeneratedDirectory[];
  readonly files: readonly GeneratedFile[];
  readonly conflicts: readonly GenerationConflict[];
}

export interface GeneratedFile {
  readonly path: string;
  readonly content: string;
  readonly source: string;
  readonly description?: string;
  readonly executable?: boolean;
}

export interface GeneratedDirectory {
  readonly path: string;
  readonly source: string;
  readonly preserveWhenEmpty: boolean;
  readonly description?: string;
}

export interface GenerationConflict {
  readonly path: string;
  readonly kind: "file-exists" | "directory-exists" | "type-mismatch";
  readonly message: string;
}
```

The exact implementation may differ, but the architectural boundary should remain.

Likely future files:

```text
packages/cli/src/engine/generation-plan.ts
packages/cli/src/engine/create-generation-plan.ts
packages/cli/src/engine/execute-generation-plan.ts
packages/cli/src/filesystem/write-policy.ts
packages/cli/src/filesystem/safe-file-writer.ts
packages/cli/src/validation/repo-contract.ts
```

The initial `init` command should not directly write files from the command handler.

Instead, it should:

```text
parse input
normalize config
create plan
check conflicts
execute plan
print summary
```

## 9. Alternatives Rejected

### 9.1 Direct File Writing

Rejected because it is too risky for a tool that intends to evolve existing repositories.

Direct file writing also weakens testability and makes dry-run behavior harder.

### 9.2 Minimal Precheck Then Direct Write

Rejected as the core architecture because it does not provide a complete model of repository changes.

A minimal precheck may be used internally as part of plan validation, but it should not replace the generation plan.

### 9.3 External Template Engine as the Only Planner

Rejected because **monorepo-generator** needs its own repo-aware generation model.

External tools may influence or support rendering, but the product must own repository contracts, manifest metadata, adapter contributions, and validation expectations.

## 10. Compatibility

This decision is compatible with:

* CLI-first architecture
* TypeScript implementation
* Bun-based development workflow
* generated repository manifests
* repo contract validation
* non-destructive file writing
* future dry-run mode
* future upgrade planning
* future diff previews
* future adapter architecture
* future plugin architecture
* future AI-assisted review workflows
* future CI validation

This decision must remain compatible with generated repositories that use:

* Bun
* pnpm
* npm
* yarn
* Go
* Python
* Rust
* Java
* Docker
* infrastructure tooling

The generation plan should describe file changes generically and should not assume a JavaScript-only repository.

## 11. Security Considerations

Planning before writing improves security and safety.

Security benefits include:

* fewer hidden file mutations
* clearer conflict detection
* less risk of overwriting secrets
* less risk of replacing user-owned configuration
* improved future audit logs
* improved ability to preview generated scripts
* improved ability to block unsafe paths

The file planning system must eventually guard against:

* path traversal
* absolute path abuse
* writing outside the target root
* unsafe executable generation
* overwriting `.env` or secret-bearing files
* untrusted remote template output
* adapter-generated unsafe content

The initial implementation should at minimum normalize generated paths and ensure generated writes remain inside the target root.

## 12. Verification

This decision can be verified when implementation includes tests showing that:

* generation creates a plan before writing
* the plan lists expected files
* the plan lists expected directories
* existing conflicting files are detected before writes
* the writer refuses conflicts by default
* dry-run can be added without rewriting command logic
* validation can use plan-derived expectations or repo contracts

Future verification commands:

```bash
bun run typecheck
bun run test
bun run build
bun run verify
```

Decision-specific verification:

* `init` command calls planning before writing.
* Generation logic can be unit tested without writing to the real repository.
* Filesystem writes are isolated in a filesystem module.
* Conflicts are represented before write execution.
* A generated manifest can be derived from planned output.

## 13. Related Requirements

Related SRS requirements:

* FR-004: Initialize a New Repository Foundation
* FR-008: Generate into an Empty Directory by Default
* FR-009: Support Current Directory Generation
* FR-016: Generate Repository Manifest
* FR-029: Validate Generated Repository Structure
* FR-032: Avoid Silent Overwrites
* FR-033: Support Dry Run
* FR-034: Support Idempotent Generation Where Practical
* FR-035: Record Template Metadata
* FR-036: Support Future Template Updates
* FR-037: Support Future Upgrade Planning
* NFR-003: Deterministic Output
* NFR-004: Safe Failure
* NFR-005: Modular Architecture
* NFR-006: Testability
* NFR-008: No Secret Generation by Default
* NFR-009: No Unapproved Remote Execution

## 14. Related Documents

Related documents:

* docs/planning/00-vision/product-inception-brief.md
* docs/planning/01-product/product-charter.md
* docs/planning/02-requirements/software-requirements-specification.md
* docs/planning/03-architecture/architecture-overview.md
* docs/planning/04-decisions/adr-index.md
* docs/planning/04-decisions/ADR-0001-cli-first-product-surface.md
* docs/planning/04-decisions/ADR-0002-typescript-and-bun-implementation-baseline.md

## 15. Follow-Up Work

* [ ] Create ADR-0004 for the default governed monorepo preset.
* [ ] Define the initial `GenerationPlan` type.
* [ ] Define the initial `GeneratedFile` type.
* [ ] Define the initial `GeneratedDirectory` type.
* [ ] Define the initial conflict model.
* [ ] Define the initial write policy model.
* [ ] Implement plan creation for the default preset.
* [ ] Implement conflict detection before writes.
* [ ] Implement safe plan execution.
* [ ] Add tests for plan creation.
* [ ] Add tests for conflict detection.
* [ ] Add tests proving existing files are not silently overwritten.
