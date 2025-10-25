# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

shadcn/ui is a component library system that provides accessible, customizable React components built with Radix UI and Tailwind CSS. The project consists of:
- A documentation/registry website (Next.js)
- A CLI tool (`shadcn`) for installing components into user projects
- A component registry system that serves as the source of truth

## Monorepo Structure

This is a pnpm workspace managed by Turborepo:

- `apps/v4/` - **Primary site** (ui.shadcn.com) using React 19 and Tailwind v4
- `apps/www/` - **Legacy site** (deprecated, all changes should be made in v4)
- `packages/shadcn/` - CLI package published to npm as `shadcn`
- `packages/tests/` - Shared test utilities and fixtures

## Development Commands

### Initial Setup
```bash
pnpm install
```

### Running Applications
```bash
# Primary development site (v4)
pnpm v4:dev                    # http://localhost:4000

# Legacy site (www) - deprecated
pnpm www:dev                   # http://localhost:3333

# CLI development
pnpm shadcn:dev                # Build CLI in watch mode
pnpm shadcn                    # Run CLI locally (requires registry running)
```

### CLI Testing Workflow
When developing the CLI, use this workflow:
1. Start registry: `pnpm v4:dev`
2. Build CLI in watch mode: `pnpm shadcn:dev`
3. Test CLI: `pnpm shadcn <command>` or `pnpm shadcn <init|add|...> -c ~/path/to/test-app`
4. Run CLI tests: `pnpm --filter=shadcn test` or `pnpm shadcn:test`

### Building
```bash
pnpm build                     # Build all workspaces
pnpm build:cli                 # Build CLI only
pnpm shadcn:build             # Build shadcn package
pnpm v4:build                 # Build v4 site
```

### Registry Operations
The registry must be rebuilt after component changes:
```bash
pnpm build:registry           # Build registry for both www and v4
pnpm registry:build           # Build v4 registry only
pnpm --filter=v4 validate:registries  # Validate registry JSON
```

### Testing
```bash
pnpm test                     # Run all tests (starts v4:dev server first)
pnpm test:dev                 # Run tests without starting server
pnpm --filter=shadcn test    # Run CLI tests only
```

### Code Quality
```bash
pnpm lint                     # Lint all workspaces
pnpm lint:fix                 # Auto-fix linting issues
pnpm typecheck               # Type check (excludes www)
pnpm format:write            # Format with Prettier
pnpm format:check            # Check formatting
pnpm check                   # Run lint + typecheck + format:check
```

## Architecture

### Registry System
The registry is the core of shadcn/ui:
- Component source code lives in `apps/v4/registry/new-york-v4/`
- Registry metadata is defined in `apps/v4/registry/registry-*.ts` files
- The `build:registry` script processes components into JSON format
- CLI fetches components from the registry at runtime

When modifying components:
1. Update source in `apps/v4/registry/new-york-v4/`
2. Update registry metadata if needed (registry-ui.ts, registry-examples.ts, etc.)
3. Run `pnpm build:registry` to regenerate JSON
4. Lint/format will auto-run as part of build:registry

### Component Organization
- `registry-ui.ts` - UI component definitions
- `registry-examples.ts` - Example/demo components
- `registry-blocks.ts` - Block/template definitions
- `registry-hooks.ts` - React hooks
- `registry-lib.ts` - Utility functions
- `registry-themes.ts` - Theme configurations
- `registry-charts.ts` - Chart components
- `registry-colors.ts` - Color system definitions

### CLI Architecture (`packages/shadcn/`)
The CLI is distributed as an npm package with these key features:
- Uses `commander` for CLI framework
- Fetches components from registry at runtime (configurable via `REGISTRY_URL`)
- Transforms and installs components into user projects
- Supports MCP (Model Context Protocol) interface via `shadcn mcp`

Key CLI files:
- `src/index.ts` - CLI entry point
- `src/commands/` - Command implementations (init, add, etc.)
- `src/utils/` - Shared utilities for transformations and file operations

### Documentation System (v4)
- Uses Fumadocs for MDX documentation
- Content lives in `apps/v4/content/`
- Fumadocs configuration in `source.config.ts`
- Documentation builds with: `pnpm --filter=v4 build`

## Commit Convention

Follow conventional commits: `category(scope): message`

Categories:
- `feat` - New features
- `fix` - Bug fixes
- `refactor` - Code refactoring
- `docs` - Documentation changes
- `build` - Build system or dependency changes
- `test` - Test additions or modifications
- `ci` - CI/CD configuration
- `chore` - Other changes

Example: `feat(components): add new variant to button component`

## Component Development Guidelines

When adding or modifying components in v4:
1. Make changes in `apps/v4/registry/new-york-v4/`
2. Update relevant registry files (`registry-ui.ts`, `registry-examples.ts`, etc.)
3. Update documentation in `apps/v4/content/docs/`
4. Run `pnpm build:registry` to regenerate registry JSON
5. Ensure `pnpm lint:fix` and `pnpm format:write` pass
6. Add tests if applicable

## Key Configuration Files

- `turbo.json` - Turborepo pipeline configuration
- `pnpm-workspace.yaml` - pnpm workspace definition
- `package.json` (root) - Workspace scripts and shared dependencies
- `vitest.config.ts` - Vitest configuration (excludes fixtures, templates, packages/tests)
- `tsconfig.json` - Shared TypeScript configuration

## Publishing

The shadcn CLI is published to npm:
```bash
cd packages/shadcn
pnpm pub:beta      # Publish beta tag
pnpm pub:release   # Publish latest tag
```

Changesets are used for version management:
```bash
pnpm release       # Run changeset version
```
