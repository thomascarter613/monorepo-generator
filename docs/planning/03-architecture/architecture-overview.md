---
title: Architecture Overview
description: Initial architecture overview for monorepo-generator.
status: draft
version: 0.1.0
created: 2026-05-10
updated: 2026-05-10
owner: project
---

# Architecture Overview

## 1. Purpose

This document defines the initial architecture for **monorepo-generator**, a CLI-first tool for generating, evolving, and governing production-grade monorepos.

The architecture is intentionally designed to support:

- deterministic repository generation
- safe file writing
- future upgrade planning
- generated repository validation
- preset-based generation
- adapter-based tool integration
- future plugin-based extensibility
- polyglot repository layouts
- documentation-first project foundations
- AI-readable project state without requiring AI

This document does not lock every implementation detail. It defines the first architectural baseline that future ADRs and implementation work should refine.

## 2. Architectural Thesis

The core architectural thesis is:

> monorepo-generator should be a repository foundation engine, not merely a template copier and not merely a wrapper around one monorepo tool.

The product should own the full lifecycle of a generated repository foundation:

1. understand user intent
2. normalize configuration
3. select a preset
4. build a generation plan
5. detect file conflicts
6. write files safely
7. record repository metadata
8. generate verification hooks
9. validate the generated repository
10. support future repository evolution

External tools such as Turborepo, Nx, moonrepo, Rush, Changesets, Pants, Copier, Cookiecutter, Plop, Hygen, and Scaffdog may influence or integrate with the product, but none of them should define the entire architecture alone.

## 3. System Context

At the highest level, **monorepo-generator** is a local developer tool.

```text
User
  |
  v
monorepo-generator CLI
  |
  v
Generation Engine
  |
  +--> Presets
  +--> Adapters
  +--> Template/Document Generators
  +--> File Planning
  +--> Safe File Writer
  +--> Manifest Writer
  +--> Validator
  |
  v
Generated Repository
````

The tool runs on the user's machine and writes files to a target repository directory.

The initial version should avoid hidden network dependencies, hidden installation steps, and remote code execution.

## 4. Primary Architectural Responsibilities

The system has seven primary responsibilities.

### 4.1 CLI Interaction

The CLI receives user commands, flags, and prompt answers.

It should handle:

* help output
* version output
* command routing
* interactive input
* non-interactive input
* terminal output
* exit codes

The CLI layer should not contain the full generation logic.

### 4.2 Configuration Normalization

The system turns CLI flags, prompt answers, defaults, and future config files into a normalized generation config.

This config becomes the canonical input to the generation engine.

### 4.3 Generation Planning

The system builds a file generation plan before writing files.

The plan should describe:

* files to create
* directories to create
* files to skip
* conflicts
* generated metadata
* selected adapters
* selected preset
* validation expectations

### 4.4 Safe File Writing

The system writes files according to an explicit write policy.

Default behavior should be conservative:

* do not silently overwrite existing files
* report conflicts
* support future dry-run mode
* support future explicit overwrite policy
* use deterministic file contents where practical

### 4.5 Manifest and Metadata Management

The system records how a repository was generated.

The generated manifest should support:

* validation
* future upgrade planning
* future repository inspection
* adapter tracking
* preset tracking
* repo contract versioning

### 4.6 Repository Validation

The system validates generated repositories against a repo contract.

Validation should be usable both by:

* the generator itself immediately after generation
* generated repository verification scripts
* future CI workflows
* future upgrade commands

### 4.7 Evolution and Ongoing Generators

The system must be designed for future commands that modify an existing repository.

Examples:

* add app
* add package
* add service
* add library
* add ADR
* add work packet
* add CI workflow
* add adapter
* upgrade generated structure
* inspect repository state
* validate repo contract

Initialization is the first command, not the entire product.

## 5. Initial Runtime and Implementation Shape

The initial implementation should use:

```text
Language: TypeScript
Runtime/package manager: Bun
CLI surface: command-based CLI
Testing: Bun test or Vitest
Generated documentation: Markdown with YAML frontmatter
Generated metadata: JSON
Generated scripts: POSIX shell initially, with cross-platform design considered
```

The first implementation should remain small and modular.

It should not begin with every adapter fully implemented.

## 6. Proposed Source Layout

The initial repository should eventually grow toward this shape:

```text
monorepo-generator/
├── docs/
│   └── planning/
├── packages/
│   └── cli/
│       ├── src/
│       │   ├── cli/
│       │   ├── commands/
│       │   ├── config/
│       │   ├── engine/
│       │   ├── filesystem/
│       │   ├── manifest/
│       │   ├── presets/
│       │   ├── validation/
│       │   ├── adapters/
│       │   └── templates/
│       ├── test/
│       ├── package.json
│       └── tsconfig.json
├── tools/
│   └── scripts/
├── .github/
│   └── workflows/
├── package.json
├── tsconfig.json
├── bun.lock
└── README.md
```

This structure may be refined through ADRs before implementation.

## 7. Core Modules

## 7.1 CLI Module

The CLI module is responsible for command parsing and user interaction.

Candidate responsibilities:

* parse root command
* parse subcommands
* parse flags
* print help
* print version
* dispatch to command handlers
* map errors to terminal output
* set process exit codes

The CLI module should call application services and avoid direct file-generation logic.

Candidate future files:

```text
packages/cli/src/cli/main.ts
packages/cli/src/cli/output.ts
packages/cli/src/cli/errors.ts
packages/cli/src/commands/init.ts
packages/cli/src/commands/validate.ts
```

## 7.2 Command Module

The command module implements user-facing commands.

Initial commands:

```text
init
validate
```

Future commands:

```text
add app
add package
add service
add doc
adr create
inspect
upgrade plan
upgrade apply
```

Each command should:

1. parse and normalize input
2. call the relevant application service
3. print a useful result
4. return an appropriate exit code

## 7.3 Configuration Module

The configuration module defines normalized input types.

It should handle:

* defaults
* prompt answers
* CLI flags
* future config files
* package manager selection
* preset selection
* adapter selection
* target directory selection
* write policy selection

Candidate concepts:

```text
RawInitOptions
NormalizedInitConfig
PresetConfig
AdapterConfig
WritePolicy
TargetDirectory
```

## 7.4 Generation Engine

The generation engine is the core application service.

It should accept normalized configuration and produce a generation plan.

The engine should not immediately write files.

It should first build a complete plan that can be inspected, validated, rendered, or dry-run.

Candidate responsibilities:

* resolve preset
* resolve adapters
* collect required directories
* collect required files
* render file contents
* detect conflicts
* produce generation plan
* pass plan to file writer when approved

Candidate future files:

```text
packages/cli/src/engine/create-generation-plan.ts
packages/cli/src/engine/execute-generation-plan.ts
packages/cli/src/engine/generation-plan.ts
```

## 7.5 Preset Module

Presets define coherent repository foundations.

A preset may define:

* directory layout
* generated files
* package manager
* scripts
* docs structure
* ADR structure
* manifest expectations
* adapter defaults
* validation rules

Initial preset:

```text
default-governed-monorepo
```

Future presets:

```text
minimal
typescript-monorepo
polyglot-monorepo
governed-saas
package-monorepo
docs-first
```

Presets should be data-driven where practical.

## 7.6 Adapter Module

Adapters generate configuration for external tools.

Potential adapters:

* Turborepo
* moonrepo
* Changesets
* Nx
* Rush
* Pants
* GitHub Actions
* Docker Compose
* OpenAPI tooling
* docs tooling

Adapters should not control the whole generation pipeline.

They should receive normalized configuration and contribute files, scripts, metadata, and validation expectations.

Adapter responsibilities may include:

* declare files to generate
* declare package scripts
* declare dependencies
* declare validation rules
* declare manifest metadata
* declare README guidance

Initial adapter candidates:

```text
github-actions
turborepo
changesets
```

However, the first implementation may generate only minimal configuration before introducing formal adapter interfaces.

## 7.7 Template and Document Module

The template/document module renders generated text files.

Initial generated files may include:

* README
* `.gitignore`
* `.editorconfig`
* `.gitattributes`
* package metadata
* ADR index
* ADR template
* verification script
* manifest JSON
* docs README files

The template layer should support deterministic rendering.

Future template support may include:

* Jinja-style rendering
* Handlebars-style rendering
* embedded TypeScript templates
* external template packs
* Copier-inspired update metadata

The initial implementation should choose the simplest reliable approach.

## 7.8 Filesystem Module

The filesystem module owns safe file and directory operations.

Responsibilities:

* normalize paths
* check whether a path exists
* create directories
* write files
* preserve empty directories
* detect conflicts
* enforce write policy
* support future dry run
* support future atomic writes where practical

This module is critical because repository generators can easily damage user work.

Default file writing policy:

```text
- create missing files
- create missing directories
- refuse conflicting existing files
- do not overwrite without explicit policy
```

## 7.9 Manifest Module

The manifest module owns generated repository metadata.

The manifest should record:

```text
generator name
generator version
repo contract version
generated timestamp
project name
selected preset
selected package manager
selected adapters
generated directories
generated files
```

Initial manifest location is open to ADR.

Candidate locations:

```text
.foundry/manifest.json
.monorepo-generator/manifest.json
.repo-generator/manifest.json
```

Because the product name is provisional, the metadata directory should be selected carefully.

## 7.10 Validation Module

The validation module checks whether a repository matches the expected repo contract.

Validation should support:

* required file checks
* required directory checks
* manifest shape checks
* package script checks
* generated docs checks
* adapter-specific checks
* JSON output in future
* CI usage in future

Validation output should include:

```text
status
errors
warnings
checked files
checked directories
manifest information
```

The validator should be usable by both:

* the CLI validate command
* generated verification scripts

## 8. Generation Pipeline

The initialization pipeline should follow this sequence:

```text
1. Receive init command
2. Collect raw options from flags/prompts
3. Normalize configuration
4. Resolve target directory
5. Resolve preset
6. Resolve selected adapters
7. Build generation plan
8. Detect file conflicts
9. Present or execute plan
10. Create directories
11. Write files
12. Write manifest
13. Run or recommend verification
14. Print summary
```

The important architectural rule is:

> Planning happens before writing.

This allows dry-run, conflict reporting, future upgrade planning, and testability.

## 9. Generation Plan Model

The generation plan should be an explicit data structure.

Conceptual shape:

```text
GenerationPlan
├── targetRoot
├── projectName
├── preset
├── packageManager
├── adapters
├── directories
├── files
├── conflicts
├── manifest
└── validationExpectations
```

A generated file entry should include:

```text
path
content
mode
overwritePolicy
source
description
```

A generated directory entry should include:

```text
path
preserveWhenEmpty
source
description
```

The plan should be serializable in the future for debugging, auditing, or dry-run output.

## 10. File Write Policy

The architecture must protect existing user files.

Initial policies:

```text
create-only
skip-existing
overwrite-explicit
```

Initial default:

```text
create-only
```

Behavior:

* If a file does not exist, create it.
* If a file exists with the same content, treat it as unchanged.
* If a file exists with different content, report a conflict.
* Do not overwrite unless explicitly allowed by a future flag or policy.

The initial version may implement only create-only conflict detection.

## 11. Manifest Model

The manifest is the durable bridge between generation and future evolution.

Initial conceptual manifest:

```json
{
  "schemaVersion": "0.1.0",
  "generator": {
    "name": "monorepo-generator",
    "version": "0.1.0"
  },
  "repository": {
    "name": "example-project",
    "preset": "default-governed-monorepo",
    "packageManager": "bun"
  },
  "adapters": [],
  "contract": {
    "name": "default-governed-monorepo",
    "version": "0.1.0"
  },
  "generated": {
    "timestamp": "2026-05-10T00:00:00.000Z",
    "directories": [],
    "files": []
  }
}
```

The exact schema should be refined during implementation.

Important rule:

> The manifest should be useful for validation and future upgrade planning, not merely informational.

## 12. Repo Contract Model

A repo contract defines what a valid generated repository must contain.

Initial repo contract may require:

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
metadata manifest
```

The repo contract should eventually include:

* required files
* required directories
* required scripts
* required manifest fields
* required documentation files
* adapter-specific requirements

Repo contracts allow generated repositories to be verified consistently.

## 13. Adapter Architecture

Adapters should be small, explicit contributors to a generation plan.

An adapter may contribute:

```text
files
directories
package scripts
dependencies
dev dependencies
README sections
manifest entries
validation rules
```

Example adapter categories:

```text
Task orchestration adapter:
  - Turborepo
  - moonrepo
  - Nx
  - Pants

Package governance adapter:
  - Changesets
  - Rush

CI adapter:
  - GitHub Actions
  - GitLab CI
  - CircleCI

Documentation adapter:
  - Markdown only
  - Starlight
  - Docusaurus
  - Fumadocs

Language adapter:
  - TypeScript
  - Go
  - Python
  - Rust
  - Java
```

The first implementation should not overbuild this system.

Instead, it should keep clean seams so adapters can emerge naturally.

## 14. Preset Architecture

Presets are product-level recipes.

A preset should answer:

```text
What kind of repository is being generated?
What folders should exist?
What tools should be configured?
What scripts should be available?
What docs should exist?
What validation should pass?
```

Initial preset:

```text
default-governed-monorepo
```

This preset should create a serious but minimal foundation.

It should not yet include every possible external tool.

Recommended initial generated shape:

```text
apps/
packages/
services/
tools/
docs/
docs/planning/
docs/planning/00-vision/
docs/planning/01-product/
docs/planning/02-requirements/
docs/planning/03-architecture/
docs/planning/04-decisions/
.foundry/ or equivalent metadata directory
```

The metadata directory name must be decided by ADR.

## 15. Validation Architecture

Validation should be treated as a first-class product feature.

Validation command flow:

```text
1. Resolve repository root
2. Read manifest if present
3. Load expected repo contract
4. Check required files
5. Check required directories
6. Check manifest schema
7. Check package scripts
8. Report errors and warnings
9. Exit with appropriate code
```

Exit behavior:

```text
0 = valid
1 = validation failed
2 = command/configuration error
```

Future JSON output should support automation:

```json
{
  "status": "failed",
  "errors": [
    {
      "code": "missing-required-file",
      "path": "README.md",
      "message": "Required file is missing."
    }
  ],
  "warnings": []
}
```

## 16. Error Handling Architecture

Errors should be explicit and typed.

Candidate error categories:

```text
InvalidInputError
TargetDirectoryError
FileConflictError
PresetNotFoundError
AdapterError
ManifestError
ValidationError
UnexpectedInternalError
```

Errors should include:

* stable error code
* human-readable message
* relevant path or field
* optional remediation guidance

The CLI should convert typed errors into readable terminal output.

## 17. Testing Architecture

The project should be testable from the beginning.

Initial test categories:

### 17.1 Unit Tests

Unit tests should cover:

* project name validation
* config normalization
* generation plan creation
* manifest generation
* path normalization
* validation logic

### 17.2 Filesystem Integration Tests

Filesystem tests should cover:

* generating into a temporary directory
* detecting conflicts
* preserving empty directories
* validating generated output
* refusing unsafe writes

### 17.3 CLI Smoke Tests

CLI tests should cover:

* help output
* version output
* init command
* validate command
* expected exit codes

Testing should avoid relying on global machine state.

## 18. CI Architecture

Initial CI should run:

```text
bun install
bun run format:check
bun run lint
bun run typecheck
bun run test
bun run build
bun run verify
```

Not every script must exist on day one, but the architecture should move toward this pattern.

CI should validate both:

* the generator project itself
* generated repository output, once generator tests exist

## 19. Documentation Architecture

Documentation should remain in the repository.

Initial planning documentation:

```text
docs/planning/00-vision/
docs/planning/01-product/
docs/planning/02-requirements/
docs/planning/03-architecture/
docs/planning/04-decisions/
```

Future documentation may include:

```text
docs/product/
docs/architecture/
docs/adr/
docs/domain/
docs/delivery/
docs/verification/
docs/operations/
docs/ai-context/
```

The current `docs/planning/` structure is acceptable because the project is in early planning.

A future ADR can decide whether to keep or evolve this layout.

## 20. AI-Readable Context Architecture

The product should support AI-assisted development without requiring AI.

Generated repositories should eventually include:

```text
docs/ai-context/
docs/ai-context/project-brief.md
docs/ai-context/current-state.md
docs/ai-context/assistant-guidelines.md
```

The purpose is to make repository context durable and portable.

The tool itself must not require:

* OpenAI
* Anthropic
* local model runtime
* MCP server
* paid subscription
* hosted AI service

AI support should be adapter-based or configuration-based in the future.

## 21. Security Architecture

The initial security posture should be conservative.

Rules:

* do not generate real secrets
* do not fetch arbitrary remote scripts
* do not execute unknown code during generation
* do not hide file writes
* do not silently overwrite files
* do not require credentials for local generation

Generated `.env.example` files, when added, must contain placeholders only.

Future remote template support must include explicit trust boundaries.

## 22. Polyglot Architecture

The product must be designed for polyglot repositories.

Initial implementation may focus on TypeScript because the CLI itself is TypeScript.

However, the architecture should avoid assumptions that every generated project is only JavaScript or TypeScript.

Polyglot support should eventually include:

```text
TypeScript
JavaScript
Go
Python
Rust
Java
Shell
Docker
Terraform/OpenTofu
Markdown
OpenAPI
```

This should be implemented through adapters and presets, not hard-coded into one monolithic init command.

## 23. Upgrade and Evolution Architecture

Future upgrade support should be designed around non-destructive planning.

Potential upgrade pipeline:

```text
1. Read repository manifest
2. Inspect current repository
3. Load target preset/contract version
4. Compare actual state to expected state
5. Generate upgrade plan
6. Report additions, changes, conflicts, and risks
7. Allow user review
8. Apply approved changes
9. Update manifest
10. Re-run validation
```

The initial version does not need full upgrade support.

However, the manifest and generation plan should not block this future.

## 24. Command Architecture

Initial commands:

```text
monorepo-generator init
monorepo-generator validate
```

Future commands:

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

Command implementation should avoid large files that mix parsing, planning, writing, and validation.

## 25. Architectural Constraints

The architecture must observe the following constraints:

1. The CLI must remain inspectable and maintainable.
2. Generation must be deterministic where practical.
3. Planning must happen before writing.
4. Default file writing must be non-destructive.
5. The system must not require a paid AI provider.
6. The system must not hard-lock into one monorepo engine.
7. The implementation must support future presets.
8. The implementation must support future adapters.
9. The generated repository must include documentation.
10. The generated repository must include validation hooks.
11. Significant decisions must be captured as ADRs.

## 26. Key Architectural Decisions Needed

The next step is to create ADR infrastructure and then decide the major architectural choices.

Initial ADRs should include:

```text
ADR-0001: CLI-first product surface
ADR-0002: TypeScript and Bun implementation baseline
ADR-0003: Repository planning before file writing
ADR-0004: Default governed monorepo preset
ADR-0005: Metadata directory and manifest location
ADR-0006: Non-destructive file writing policy
ADR-0007: Preset and adapter architecture
ADR-0008: Validation and repo contract model
ADR-0009: Documentation and ADR structure
ADR-0010: AI-native but AI-optional architecture
```

These ADRs should be created before heavy implementation.

## 27. Initial Implementation Slice

The first implementation slice should be:

```text
Build a minimal TypeScript CLI that can generate and validate a governed monorepo foundation.
```

The slice should include:

* root package setup
* CLI package
* TypeScript config
* Bun scripts
* init command
* validate command
* default preset
* generation plan model
* safe file writer
* manifest writer
* repo contract validator
* basic tests
* verification script

The first implementation should not attempt to fully integrate Nx, Turborepo, moonrepo, Rush, Pants, or Changesets.

Those should be introduced through ADRs and adapters once the generator core exists.

## 28. Architecture Summary

The architecture should be understood as five layers:

```text
CLI Layer
  Receives commands and prints results.

Application Layer
  Normalizes input and coordinates use cases.

Generation Layer
  Builds generation plans from presets and adapters.

Filesystem/Manifest Layer
  Writes files safely and records generated metadata.

Validation Layer
  Checks generated repositories against repo contracts.
```

This structure keeps the system testable, extensible, and safe.

## 29. Immediate Next Step

After this architecture overview is accepted, the next documents should be:

```text
docs/planning/04-decisions/adr-index.md
docs/planning/04-decisions/adr-template.md
```

After the ADR index and template exist, begin recording the initial architecture decisions.
