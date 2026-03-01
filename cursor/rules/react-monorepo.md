# Cursor Rules for React/TypeScript Monorepo

## Project Overview

This is a monorepo web application built with React, TypeScript, TanStack Query, and uses workspace-based package management. The project includes multiple apps and shared packages.

## Code Quality & Build Requirements

### Automatic Build Validation

- After ALL code updates, run build scripts for modified apps
- Build scripts should run in the background to validate PR readiness
- Commands:
  - Root: `{YOUR_BUILD_CMD}`
  - Specific app: `{YOUR_FILTER_CMD} {app} build`

### TypeScript Validation

- After every set of changes, run `{YOUR_TYPECHECK_CMD}` in the background
- If TypeScript errors are found, fix them using established patterns
- If zero errors, stop running typecheck and continue conversation
- **NEVER use `any` type when creating or adding new types**

### Code Formatting

- Run Prettier on all files before creating PRs
- Use the project's prettier configuration
- Format command: `{YOUR_FORMAT_CMD}`

### Console Log Management

- Remove ALL added console.log statements before pushing branches for PRs
- Clean up debug logging before code review

## Technology Stack & Patterns

### State Management

- **Primary**: Use your chosen state library (Jotai / Zustand / Context)
- Leverage persistence to localStorage where appropriate
- Follow established patterns in the codebase

### API & Cache Management

- **Primary**: Use TanStack Query (`@tanstack/react-query`) for all API operations
- Use TanStack Query for cache management on all queries and mutations
- Avoid legacy data fetching patterns for new implementations

### Component Libraries

- Use your design system components (workspace package)
- Use shadcn-ui components when appropriate
- Follow established component patterns in the shared package

## Development Workflow

### PR Preparation Checklist

1. Run build validation for all apps
2. Run TypeScript checks
3. Remove console.log statements
4. Run prettier formatting
5. Ensure all tests pass

### Agent Review Pre-Push Gate

- During agent review before push/PR, run this lint validation sequence:
  1. `{YOUR_LINT_CMD_SHARED}`
  2. `{YOUR_LINT_CMD_ALL}`
- Use fail-fast behavior:
  - If step 1 fails, stop immediately
  - Only run step 2 if step 1 passes

### Background Tasks

- Build validation should run automatically in background
- TypeScript checking should run after changes
- Exit background tasks when no errors are found

## Code Style Preferences

### General Guidelines

- Prefer concise solutions when proposing code fixes
- Follow existing patterns established in the codebase
- Use TypeScript strictly - leverage type safety
- Maintain consistency with existing component structure

### File Organization

- Follow the established monorepo structure
- Use appropriate workspace packages for shared code
- Maintain clear separation between apps and packages

## Package Management

- Use {YOUR_PACKAGE_MANAGER} for package management
- Respect workspace dependencies
- Use workspace protocols for internal packages

## Tools & Scripts

- **Linting**: `{YOUR_LINT_CMD}`
- **Formatting**: `{YOUR_FORMAT_CMD}`
- **Type Checking**: `{YOUR_TYPECHECK_CMD}`
- **Building**: `{YOUR_BUILD_CMD}`
- **Testing**: `{YOUR_TEST_CMD}`
