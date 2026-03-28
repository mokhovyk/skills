---
name: comprehensive-docs
description: "Analyze a codebase and generate comprehensive documentation from the source code. Produces a full docs suite: README, architecture overview, API reference, module guides, setup/configuration docs, and contributing guide. Use this skill whenever the user asks to document a project, generate docs, write a README from code, explain the architecture, create API docs, or asks for any form of codebase documentation. Also use when the user says things like 'document this', 'write docs', 'I need documentation', or references a docs/ directory."
---

# Comprehensive Codebase Documentation

Generate a complete documentation suite by analyzing a project's source code, structure, and configuration.

## What You Produce

A `docs/` directory (or single README, depending on project size) containing:

| Document | Purpose | When to include |
|----------|---------|-----------------|
| `README.md` | Project overview, quickstart, badges | Always |
| `ARCHITECTURE.md` | System design, data flow, key decisions | Projects with 3+ modules or non-trivial structure |
| `API.md` | Public API reference with signatures, params, return types | Projects exposing APIs (REST, GraphQL, library exports) |
| `CONFIGURATION.md` | Environment variables, config files, feature flags | Projects with config files or env vars |
| `MODULES.md` | Per-module or per-package breakdown | Monorepos or projects with distinct subsystems |
| `CONTRIBUTING.md` | Dev setup, testing, PR workflow, code style | Open-source or team projects |

Not every project needs every document. A small CLI tool might only need a solid README. A large monorepo might need all six. Use your judgment based on what you find.

## Workflow

### Phase 1: Reconnaissance

Before writing anything, understand the project. Run these in parallel where possible:

1. **Project identity** — Read `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, or equivalent. Extract: name, description, language/runtime, dependencies, scripts/commands.

2. **Directory structure** — Map the top-level layout. Identify:
   - Source directories (src/, lib/, app/, cmd/)
   - Test directories (tests/, __tests__/, spec/)
   - Config files (.env.example, config/, docker-compose.yml)
   - Existing docs (README.md, docs/, wiki/)
   - CI/CD (.github/workflows/, .gitlab-ci.yml)

3. **Entry points** — Find main entry files (main.ts, index.js, main.go, app.py). Trace the boot sequence: what gets initialized, in what order.

4. **Public surface** — Identify exported functions, classes, API routes, CLI commands. These are what users interact with and what needs documenting most.

5. **Existing documentation** — Read any existing README or docs. Note what's already covered, what's outdated, and what's missing. You're building on existing work, not replacing it blindly.

### Phase 2: Planning

Based on reconnaissance, decide which documents to generate. Consider:

- **Project size**: Small projects (< 10 files) usually need just a README. Medium projects need README + one or two supporting docs. Large projects benefit from the full suite.
- **Audience**: Libraries need API docs. Services need architecture and config docs. Tools need usage guides.
- **Existing docs**: If there's already a good ARCHITECTURE.md, don't rewrite it — update it or skip it.

Tell the user your plan before writing: "Based on what I found, I'll generate X, Y, and Z. Does that sound right?"

### Phase 3: Writing

Write each document following the templates below. The key principle: **every claim must come from the code**. Don't guess at behavior — read the implementation. Don't assume config options — check the schema or validation logic.

#### Writing principles

- **Code-sourced**: Every statement should be traceable to something in the codebase. If you can't find it in the code, don't document it.
- **Example-driven**: Show real usage, not abstract descriptions. Pull examples from tests when possible — they're already verified.
- **Layered**: Start with the simplest use case, then progressively add detail. A reader should be able to stop at any heading and have something useful.
- **Honest about gaps**: If something is undocumented or unclear in the code, say so: "Configuration for X is defined in `config.ts` but not validated — check the source for current options."

### Phase 4: Validation

After writing, verify your docs:

1. **Check code references** — Every file path, function name, and config key you mention should actually exist. Grep for them.
2. **Test commands** — If you document shell commands (install, build, test, run), verify they match what's in package.json/Makefile/etc.
3. **Cross-reference** — Make sure links between documents work and terminology is consistent.

## Document Templates

### README.md

```markdown
# Project Name

One-line description of what this project does and who it's for.

## Features

- Feature 1: brief explanation
- Feature 2: brief explanation

## Quick Start

### Prerequisites

- Runtime version (e.g., Node.js >= 18)
- Required system tools

### Installation

[Exact install commands from package manager config]

### Usage

[Minimal working example — ideally copy-pasteable]

## Documentation

- [Architecture](docs/ARCHITECTURE.md) — system design and key decisions
- [API Reference](docs/API.md) — endpoints, functions, and types
- [Configuration](docs/CONFIGURATION.md) — environment variables and settings

## Development

[How to run in dev mode, run tests, lint]

## License

[From LICENSE file or package.json]
```

### ARCHITECTURE.md

```markdown
# Architecture

## Overview

[1-2 paragraph summary of the system: what it does, major components, how they connect]

## System Diagram

[ASCII or Mermaid diagram showing major components and data flow]

## Components

### Component Name
- **Location**: `src/component/`
- **Responsibility**: What it does
- **Key files**: Entry point, types, main logic
- **Dependencies**: What it talks to

[Repeat for each major component]

## Data Flow

[Describe the primary data paths through the system — e.g., request lifecycle, event processing pipeline]

## Key Design Decisions

[Document non-obvious architectural choices and why they were made, if evident from code comments or commit messages]
```

### API.md

For REST/GraphQL APIs:
```markdown
# API Reference

## Authentication

[Auth mechanism found in middleware/auth code]

## Endpoints

### `METHOD /path`

Description of what this endpoint does.

**Parameters:**
| Name | Type | Required | Description |
|------|------|----------|-------------|
| param | string | yes | What it does |

**Response:**
[Shape of response object, from types or actual response construction]

**Example:**
[Request/response pair]
```

For library APIs:
```markdown
# API Reference

## `functionName(params): ReturnType`

Description from docstring or inferred from implementation.

**Parameters:**
- `param` (Type) — description

**Returns:** Type — description

**Example:**
[Usage example, ideally from tests]
```

### CONFIGURATION.md

```markdown
# Configuration

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `VAR_NAME` | yes/no | value | What it controls |

[Source: extracted from .env.example, config validation, or process.env references]

## Config Files

### `documentation.yaml`

[Document the schema with descriptions for each field]
```

## Handling Edge Cases

- **No existing docs**: Start from scratch. More work but cleaner result.
- **Outdated docs**: Read them for context but regenerate from code. Note in the PR/commit that docs were refreshed.
- **Monorepo**: Generate a root README with links, plus per-package docs where warranted.
- **Incomplete code**: Document what exists. Mark TODOs where code is clearly unfinished.
- **Multiple languages**: Document each language's conventions separately within the same structure.

## Output Location

- Place generated docs in `docs/` directory at project root
- README.md goes at project root
- CONTRIBUTING.md goes at project root
- If the user specifies a different location, use that instead
