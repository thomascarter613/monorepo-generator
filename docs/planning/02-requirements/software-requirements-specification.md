---
title: Software Requirements Specification
description: Initial software requirements specification for monorepo-generator.
status: draft
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# Software Requirements Specification

## 1. Purpose

This Software Requirements Specification defines the initial requirements for **monorepo-generator**, a free and open-source command-line tool for generating, evolving, and governing production-grade monorepos.

The purpose of this document is to establish a clear requirements baseline before implementation begins.

## 2. Product Summary

**monorepo-generator** is a CLI-first tool that generates complete repository foundations for modern software projects.

The tool should eventually support:

- full monorepo initialization
- polyglot repository layouts
- application, package, library, and service generation
- documentation scaffolding
- ADR scaffolding
- task orchestration configuration
- CI workflow generation
- ownership metadata
- package versioning and publishing workflows
- repository manifests
- verification scripts
- ongoing generators
- repository inspection
- repository validation
- upgrade and evolution workflows
- AI-readable project context

The initial version should focus on creating and verifying a minimal governed monorepo foundation.

## 3. Scope

## 3.1 In Scope for Initial Product

The initial product shall provide:

- a CLI executable
- a repository initialization command
- interactive initialization prompts
- non-interactive initialization inputs
- generated repository files
- generated documentation directories
- generated ADR directories
- generated package metadata
- generated task scripts
- generated CI workflow
- generated repository manifest
- generated verification script
- deterministic output
- safe file writing behavior
- useful terminal output
- meaningful error handling

## 3.2 Out of Scope for Initial Product

The initial product shall not yet provide:

- a hosted SaaS service
- a web UI
- a plugin marketplace
- deep AI model integration
- remote template registry
- full Nx integration
- full Rush integration
- full Pants integration
- full moonrepo integration
- full Copier-compatible update implementation
- production deployment orchestration
- automatic cloud provisioning
- package publishing automation beyond generated configuration
- destructive repository rewrites

These may become future capabilities.

## 4. Definitions

| Term | Meaning |
|---|---|
| CLI | Command-line interface used to run the tool. |
| Generated repository | A repository created or modified by monorepo-generator. |
| Workspace | The top-level generated monorepo. |
| App | A runnable application inside the monorepo. |
| Package | A reusable code package inside the monorepo. |
| Service | A backend, worker, API, or system service inside the monorepo. |
| Library | Shared code used by apps, packages, or services. |
| ADR | Architecture Decision Record. |
| Repo contract | A set of expected files, folders, scripts, metadata, and conventions that define a valid generated repository. |
| Manifest | A machine-readable file describing the generated repository and generator configuration. |
| Preset | A named generation profile that selects default tools, folders, scripts, and conventions. |
| Adapter | A module that emits configuration for an external tool such as Turborepo, Nx, moonrepo, Rush, Changesets, or Pants. |
| Generator | A command or module that creates files or modifies the repository. |
| Evolution command | A command that modifies an existing repository after initial creation. |

## 5. User Classes

## 5.1 Solo Developer

A solo developer uses the tool to quickly create a serious project foundation without repeatedly rebuilding common files and folders.

## 5.2 Startup Founder

A startup founder uses the tool to establish a maintainable product repository that can grow with a future team.

## 5.3 Principal Engineer or Architect

A principal engineer uses the tool to define consistent repository standards, documentation structure, CI conventions, and project boundaries.

## 5.4 Platform Engineer

A platform engineer uses the tool to build or enforce internal golden paths for engineering teams.

## 5.5 Consultant or Agency Developer

A consultant uses the tool to repeatedly bootstrap client projects with predictable structure and quality standards.

## 5.6 Open-Source Maintainer

An open-source maintainer uses the tool to generate project structure, contribution workflows, documentation, and release support.

## 6. Operating Assumptions

The initial assumptions are:

- The product will be implemented as a CLI.
- The implementation language will be TypeScript.
- The development package manager will be Bun.
- Generated repositories should be Git-compatible.
- Generated repositories should be readable and editable.
- Generated repositories should not require AI access.
- Generated repositories should include documentation from the beginning.
- Generated repositories should include verification commands from the beginning.
- The first implementation should prioritize deterministic file generation over advanced orchestration.
- The architecture should preserve room for future adapters and plugins.

## 7. Functional Requirements

## 7.1 CLI Requirements

### FR-001: Provide a CLI Entry Point

The system shall provide a command-line executable that users can run from a terminal.

Priority: Must

Acceptance criteria:

- The CLI can be invoked from the command line.
- The CLI prints helpful output.
- The CLI exits with status code `0` on success.
- The CLI exits with a non-zero status code on failure.

### FR-002: Provide Help Output

The system shall provide help output for the root command and each supported subcommand.

Priority: Must

Acceptance criteria:

- Running the CLI with a help flag displays usage information.
- Help output includes available commands.
- Help output includes important flags.
- Help output is readable in a standard terminal.

### FR-003: Provide Version Output

The system shall provide a way to print the current tool version.

Priority: Should

Acceptance criteria:

- A version command or flag prints the current package version.
- The version value matches project metadata.

## 7.2 Initialization Requirements

### FR-004: Initialize a New Repository Foundation

The system shall provide an initialization command that creates a repository foundation.

Priority: Must

Acceptance criteria:

- The command can generate files into a target directory.
- The command creates a coherent repository structure.
- The command reports what it created.
- The command does not silently fail.

### FR-005: Support Interactive Initialization

The initialization command shall support interactive prompts.

Priority: Must

Acceptance criteria:

- The user can answer prompts for basic project configuration.
- Prompts include project name.
- Prompts include package manager selection when more than one package manager is supported.
- Prompts include preset selection when more than one preset is supported.

### FR-006: Support Non-Interactive Initialization

The initialization command shall support non-interactive usage through flags or configuration.

Priority: Must

Acceptance criteria:

- The user can provide required values without prompts.
- The command can run in CI or scripted environments.
- Missing required values fail with a useful error.

### FR-007: Validate Project Name

The system shall validate the project name before generation.

Priority: Must

Acceptance criteria:

- Empty project names are rejected.
- Project names with invalid package-name characters are rejected or normalized according to documented rules.
- Validation errors explain the problem.

### FR-008: Generate into an Empty Directory by Default

The system shall generate into an empty directory by default.

Priority: Must

Acceptance criteria:

- The tool detects whether the target directory exists.
- The tool detects whether the target directory contains files.
- The default behavior avoids overwriting existing files.

### FR-009: Support Current Directory Generation

The system shall allow generation into the current directory when safe.

Priority: Must

Acceptance criteria:

- The tool can generate into `.`.
- The tool refuses unsafe writes unless an explicit policy allows them.
- Existing files are handled according to documented overwrite rules.

## 7.3 File Generation Requirements

### FR-010: Generate README

The initialization command shall generate a top-level README.

Priority: Must

Acceptance criteria:

- The README contains the project name.
- The README explains the generated repository purpose.
- The README includes basic development commands.

### FR-011: Generate `.gitignore`

The initialization command shall generate a `.gitignore`.

Priority: Must

Acceptance criteria:

- The `.gitignore` excludes dependencies.
- The `.gitignore` excludes build outputs.
- The `.gitignore` excludes environment files.
- The `.gitignore` avoids excluding intended source files.

### FR-012: Generate `.editorconfig`

The initialization command shall generate an `.editorconfig`.

Priority: Must

Acceptance criteria:

- The file defines UTF-8.
- The file defines LF line endings.
- The file defines final newline behavior.
- The file defines default indentation.

### FR-013: Generate Package Metadata

The initialization command shall generate package metadata where applicable.

Priority: Must

Acceptance criteria:

- A generated TypeScript/Bun workspace includes a valid `package.json`.
- The package metadata includes useful scripts.
- The package metadata includes the generated project name.

### FR-014: Generate Documentation Structure

The initialization command shall generate a documentation structure.

Priority: Must

Acceptance criteria:

- The generated repository includes a docs directory.
- The docs structure includes planning or architecture areas.
- The docs structure includes a place for ADRs.
- The docs structure includes a place for verification documents.

### FR-015: Generate ADR Structure

The initialization command shall generate an ADR structure.

Priority: Must

Acceptance criteria:

- The generated repository includes an ADR directory.
- The generated repository includes an ADR index.
- The generated repository includes an ADR template.

### FR-016: Generate Repository Manifest

The initialization command shall generate a machine-readable repository manifest.

Priority: Must

Acceptance criteria:

- The manifest is valid JSON.
- The manifest records the generator name.
- The manifest records the generator version when available.
- The manifest records selected options.
- The manifest records generated preset information.

### FR-017: Generate Verification Script

The initialization command shall generate a verification script.

Priority: Must

Acceptance criteria:

- The generated repository includes a verification command.
- The verification command checks for required files.
- The verification command exits non-zero on failure.
- The verification command prints useful output.

### FR-018: Generate CI Workflow

The initialization command shall generate an initial CI workflow.

Priority: Should

Acceptance criteria:

- The generated repository includes a GitHub Actions workflow when GitHub Actions is selected.
- The workflow installs dependencies.
- The workflow runs verification.
- The workflow runs lint, typecheck, or test commands when present.

## 7.4 Monorepo Structure Requirements

### FR-019: Generate Standard Top-Level Directories

The initialization command shall generate standard monorepo directories.

Priority: Must

Acceptance criteria:

- The generated repository includes directories for apps.
- The generated repository includes directories for packages or libraries.
- The generated repository includes directories for services.
- The generated repository includes directories for tools.
- The generated repository includes directories for docs.

### FR-020: Support Empty Directory Preservation

The system shall preserve intentionally empty generated directories.

Priority: Must

Acceptance criteria:

- Empty generated directories include `.gitkeep` or equivalent placeholder files.
- Placeholder files are documented or conventional.
- Verification recognizes required empty directories.

### FR-021: Support Preset-Specific Layouts

The system shall support different generated layouts through presets.

Priority: Should

Acceptance criteria:

- A preset can define required directories.
- A preset can define required files.
- A preset can define package scripts.
- A preset can define verification expectations.

## 7.5 Task Orchestration Requirements

### FR-022: Generate Basic Task Scripts

The initialization command shall generate basic task scripts.

Priority: Must

Acceptance criteria:

- The generated repository includes a verification script.
- The generated repository includes a formatting or linting script when tooling is present.
- The generated repository includes a typecheck script when TypeScript is present.
- Scripts are documented in the README.

### FR-023: Support Turborepo Configuration

The system should support generating Turborepo configuration.

Priority: Should

Acceptance criteria:

- A selected preset can generate `turbo.json`.
- Generated package scripts are compatible with Turborepo.
- The generated README explains task execution.

### FR-024: Support Future Orchestration Adapters

The system architecture shall allow additional orchestration adapters.

Priority: Should

Acceptance criteria:

- The architecture can support future Nx, moonrepo, Pants, or Rush adapters.
- Adapter-specific output is isolated from core generation logic.
- Adapter selection can be represented in the repository manifest.

## 7.6 Ongoing Generator Requirements

### FR-025: Support Future App Generation

The system shall be designed to support adding apps after initialization.

Priority: Should

Acceptance criteria:

- The architecture distinguishes initial generation from later generation.
- The repository manifest can record generated apps.
- Future app generation can reuse shared file-writing policy.

### FR-026: Support Future Package Generation

The system shall be designed to support adding packages after initialization.

Priority: Should

Acceptance criteria:

- The architecture distinguishes package generation from initial generation.
- The repository manifest can record generated packages.
- Future package generation can reuse shared validation.

### FR-027: Support Future Service Generation

The system shall be designed to support adding services after initialization.

Priority: Should

Acceptance criteria:

- The architecture distinguishes service generation from initial generation.
- The repository manifest can record generated services.
- Future service generation can reuse shared validation.

### FR-028: Support Future Document Generation

The system shall be designed to support adding docs, ADRs, and work packets after initialization.

Priority: Should

Acceptance criteria:

- The architecture can support document generators.
- The repository manifest can record document conventions.
- Generated documents can include YAML frontmatter.

## 7.7 Repository Validation Requirements

### FR-029: Validate Generated Repository Structure

The system shall provide or generate validation for repository structure.

Priority: Must

Acceptance criteria:

- Validation checks required files.
- Validation checks required directories.
- Validation reports missing required items.
- Validation exits non-zero when the repo contract is violated.

### FR-030: Validate Manifest Format

The system shall validate repository manifest format.

Priority: Must

Acceptance criteria:

- Invalid JSON fails validation.
- Missing required manifest fields fail validation.
- Validation error messages identify the failed field.

### FR-031: Support Machine-Readable Validation Output

The system should support machine-readable validation output.

Priority: Should

Acceptance criteria:

- Validation can output JSON.
- JSON output includes pass/fail status.
- JSON output includes errors.
- JSON output includes warnings when applicable.

## 7.8 Safety Requirements

### FR-032: Avoid Silent Overwrites

The system shall not silently overwrite existing files.

Priority: Must

Acceptance criteria:

- Existing file conflicts are detected.
- Default behavior refuses overwrites.
- An explicit overwrite policy is required to replace files.
- Conflict output identifies affected paths.

### FR-033: Support Dry Run

The system should support dry-run generation.

Priority: Should

Acceptance criteria:

- Dry run prints planned changes.
- Dry run does not write files.
- Dry run returns success when the plan is valid.
- Dry run reports conflicts.

### FR-034: Support Idempotent Generation Where Practical

The system should make generation repeatable where practical.

Priority: Should

Acceptance criteria:

- Re-running generation with the same inputs does not corrupt output.
- Existing generated files are handled according to policy.
- The tool can distinguish unchanged files from conflicts where practical.

## 7.9 Template and Evolution Requirements

### FR-035: Record Template Metadata

The system shall record template or preset metadata in the repository manifest.

Priority: Must

Acceptance criteria:

- The manifest identifies the selected preset.
- The manifest identifies generated tool adapters.
- The manifest can later support upgrade planning.

### FR-036: Support Future Template Updates

The system architecture shall allow future update workflows inspired by full-repo template systems.

Priority: Should

Acceptance criteria:

- Generated repositories include enough metadata to know how they were generated.
- The architecture does not prevent future diff-aware upgrades.
- Template/preset identity is represented in machine-readable form.

### FR-037: Support Future Upgrade Planning

The system should eventually inspect an existing repository and propose changes.

Priority: Could

Acceptance criteria:

- The future upgrade system can read the repository manifest.
- The future upgrade system can compare expected files against actual files.
- The future upgrade system can produce a non-destructive plan.

## 8. Non-Functional Requirements

## 8.1 Usability

### NFR-001: Clear Command Output

The system shall provide clear command output.

Acceptance criteria:

- Successful commands explain what happened.
- Failed commands explain what failed.
- Output avoids unnecessary noise.

### NFR-002: Useful Errors

The system shall provide useful error messages.

Acceptance criteria:

- Errors include the cause.
- Errors include the affected file, field, or command when applicable.
- Errors do not expose sensitive environment information.

## 8.2 Reliability

### NFR-003: Deterministic Output

The system shall produce deterministic output for the same inputs and version.

Acceptance criteria:

- File paths are predictable.
- File contents are predictable.
- Generated metadata is stable except for intentional timestamps or version fields.

### NFR-004: Safe Failure

The system shall fail safely.

Acceptance criteria:

- Failed generation does not silently leave the repository in an unknown state.
- Errors are surfaced to the user.
- Partial writes are minimized or documented.

## 8.3 Maintainability

### NFR-005: Modular Architecture

The implementation shall be modular.

Acceptance criteria:

- CLI parsing is separated from generation logic.
- File writing is separated from planning.
- Presets are separated from core engine behavior.
- Future adapters can be added without rewriting the whole CLI.

### NFR-006: Testability

The implementation shall be testable.

Acceptance criteria:

- Core generation logic can be tested without invoking an external shell.
- File generation can be tested in temporary directories.
- Validation logic can be tested independently.

## 8.4 Portability

### NFR-007: Cross-Platform Intent

The system should be designed with cross-platform usage in mind.

Acceptance criteria:

- Core CLI logic should not require Linux-only behavior.
- Generated shell scripts may initially target POSIX environments, but this limitation must be documented.
- Path handling should use platform-safe APIs in implementation code.

## 8.5 Security

### NFR-008: No Secret Generation by Default

The system shall not generate real secrets.

Acceptance criteria:

- Generated `.env.example` files use placeholders only.
- Generated files do not include tokens, passwords, private keys, or credentials.

### NFR-009: No Unapproved Remote Execution

The system shall not execute remote code during generation without explicit design and user consent.

Acceptance criteria:

- Initial generation does not fetch arbitrary remote scripts.
- External tool installation is not hidden.
- Future remote template behavior must be explicit.

## 8.6 Performance

### NFR-010: Fast Initialization

The initialization command should complete quickly for the default preset.

Acceptance criteria:

- Default generation should be mostly file I/O.
- The initial implementation should avoid unnecessary dependency installation during generation.
- The user should be able to inspect generated files before installing dependencies.

## 9. External Interface Requirements

## 9.1 Command-Line Interface

The product shall expose commands through a CLI.

Initial candidate commands:

```text
monorepo-generator --help
monorepo-generator --version
monorepo-generator init
monorepo-generator validate
````

Future candidate commands:

```text
monorepo-generator add app
monorepo-generator add package
monorepo-generator add service
monorepo-generator add doc
monorepo-generator adr create
monorepo-generator inspect
monorepo-generator upgrade plan
monorepo-generator upgrade apply
```

## 9.2 File System Interface

The product shall read and write repository files.

Requirements:

* File writes must be planned before execution.
* File writes must respect overwrite policy.
* Generated paths must be normalized.
* Generated files must use LF line endings.

## 9.3 Git Interface

The product should be Git-aware but should not require destructive Git operations.

Requirements:

* The tool may detect whether the target directory is inside a Git repository.
* The tool may recommend commits.
* The tool shall not force commits in the initial version.
* The tool shall not force pushes.

## 10. Data Requirements

## 10.1 Repository Manifest

The generated repository manifest should include:

```text
generator name
generator version
generated timestamp
project name
selected preset
selected package manager
selected adapters
generated directories
generated files
repo contract version
```

## 10.2 Future Audit Events

The architecture should allow future audit logging.

Potential event fields:

```text
event id
timestamp
command
actor
target path
action
status
metadata
```

Audit logging is not required for the first minimal implementation, but the architecture should not prevent it.

## 11. Initial Repo Contract

The initial generated repository should include, at minimum:

```text
README.md
.gitignore
.editorconfig
.gitattributes
package.json
docs/
docs/planning/
docs/planning/00-vision/
docs/planning/01-product/
docs/planning/02-requirements/
docs/planning/03-architecture/
docs/planning/04-decisions/
apps/
packages/
services/
tools/
.foundry/
.foundry/manifest.json
```

The exact hidden metadata directory name may be changed by ADR.

## 12. MVP Acceptance Criteria

The MVP is acceptable when:

1. A user can run an initialization command.
2. The tool can generate a repository into a safe target directory.
3. The generated repository contains required foundation files.
4. The generated repository contains a documentation structure.
5. The generated repository contains an ADR structure.
6. The generated repository contains a machine-readable manifest.
7. The generated repository contains a verification command.
8. The verification command passes immediately after generation.
9. Existing files are not silently overwritten.
10. The implementation has tests for core generation behavior.
11. The project has CI that runs verification.
12. The project has ADRs for major technical decisions.

## 13. Requirements Traceability

| Requirement Area         | Related Product Charter Section  | Initial Implementation Area |
| ------------------------ | -------------------------------- | --------------------------- |
| CLI entry point          | Product Direction                | CLI package                 |
| Initialization           | Product Scope                    | `init` command              |
| Documentation generation | Product Principles               | docs generator              |
| ADR generation           | Project Governance               | ADR generator               |
| Verification             | Non-Negotiable Quality Standards | validation engine           |
| Manifest                 | Template Drift Risk              | manifest writer             |
| Safe writes              | Quality Standards                | file writer                 |
| Presets                  | Scope Explosion Risk             | preset registry             |
| Future adapters          | Compose Best-of-Breed Tools      | adapter interface           |
| Future upgrades          | Support Evolution                | upgrade planner             |

## 14. Open Questions

The following questions require ADRs or later planning:

1. What should the final product name be?
2. Should the CLI package name remain `monorepo-generator`?
3. Should the tool use oclif, Clipanion, Commander, or a custom CLI parser?
4. Should generated metadata live in `.foundry/`, `.monorepo-generator/`, or another directory?
5. Should the first default generated monorepo use Turborepo immediately?
6. Should moonrepo be included in the first default preset or added later?
7. Should Changesets be included in the first default preset?
8. What should the first generated app/package/service examples be?
9. What is the minimum useful plugin interface?
10. What level of Copier-style update compatibility should be supported?

## 15. Initial Implementation Recommendation

The first implementation slice should be:

```text
Build a TypeScript CLI that can generate and validate a minimal governed monorepo foundation.
```

The slice should include:

* CLI entry point
* `init` command
* file planning
* safe file writing
* default preset
* repository manifest
* verification script generation
* basic validation command
* tests for generated output

## 16. Immediate Next Step

After this SRS is accepted, the next document should be:

```text
docs/planning/03-architecture/architecture-overview.md
```

That architecture overview should define the initial system shape, modules, command boundaries, generation pipeline, file-writing model, manifest model, validation model, and future adapter/plugin seams.
