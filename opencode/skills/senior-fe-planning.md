# Senior FE Planning

**Domain**: Frontend architecture planning, React, TanStack Query, state management, component design

**When to Use This Skill**:

- When the user asks for a plan or architecture for frontend work
- When designing data fetching, state management, or UI component architecture
- When the user asks for implementation steps for a frontend feature

**Trigger Phrases**: "plan", "approach", "architecture", "frontend plan", "implementation plan"

---

## Philosophy

**Core Principle**: Produce concise, production-ready plans that follow existing code patterns first, use the project's established tech stack, and prioritize performance and accessibility.

---

## Agent Instructions

### 1. Understand the Goal and Constraints

- Summarize the target behavior and scope.
- Document assumptions explicitly (do not ask for confirmation).

### 2. Check Existing Patterns

- Identify relevant files or patterns to mirror (routes, hooks, components, utils).
- Prefer existing components and design system primitives.

### 3. Choose Architecture and Data Strategy

- **Data Fetching**: Define query keys, caching, invalidation, error handling.
- **State Management**: Identify global state vs local state boundaries.
- **Validation**: Include only if validation is non-trivial or reused.
- **Animation**: Specify when/why animation is needed and performance implications.

### 4. Propose a File/Folder Layout

- Keep modules siloed by concern (components, hooks, utils, types).
- Prefer feature-based folders if that matches the repo.

### 5. List Implementation Steps

- Ordered, minimal steps that map to files.
- Include edge cases and error states.

### 6. Define Validation and Testing

- UI states: loading, error, empty, populated.
- Cache invalidation behavior.
- Performance and accessibility checks.

---

## Output Template

Use this exact structure for all plans:

```
Overview
- [1-2 sentences on goal and scope]

Assumptions
- [explicit assumptions, no questions]

Architecture
- Data: query keys, cache, invalidation strategy
- State: global atoms vs local state
- Validation: schema validation if needed
- UI: component library usage
- Motion: animation usage and performance

File Plan
- [path]: purpose
- [path]: purpose

Implementation Steps
1. [step tied to files]
2. [step tied to files]

Risks / Edge Cases
- [risk or edge case + mitigation]

Validation / Test Plan
- [manual checks and automated tests]
```

---

## Tech Stack Reference

Customize this table for your project:

| Layer            | Technology                | Usage                                  |
| ---------------- | ------------------------- | -------------------------------------- |
| Data fetching    | TanStack Query            | API operations, caching, invalidation  |
| State management | Jotai / Zustand / Context | Global state, localStorage persistence |
| UI components    | shadcn/ui + Tailwind      | Styling and design system              |
| Animation        | Framer Motion             | Transitions and interactions           |
| Validation       | Zod                       | Schema validation where needed         |

### Key Rules

- Prefer concise plans with clear trade-offs.
- Keep terminology consistent.
- Avoid adding new libraries unless necessary.
- Follow existing patterns in the codebase.

---

**Skill Version**: 1.0.0
