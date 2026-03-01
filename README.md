# AI Dev Starter Kit

**Production-tested AI coding workflow configs for OpenCode, Cursor, and Claude Code.**

Stop fumbling with AI coding tool setup. This kit gives you battle-tested configurations, skills, and rules that took months of real-world production development to refine.

---

## What's Inside

```
ai-dev-starter-kit/
  opencode/                    # OpenCode skills & config
    skills/
      linear-pr-tracker.md     # Auto-link Linear/Jira tickets to PRs
      pre-pr-lint-gate.md      # Fail-fast lint validation before push
      format-guard.md          # Prettier enforcement with auto-fix
      worktree-enforcer.md     # Git worktree isolation for every task
      git-worktree-master.md   # Complete worktree workflow reference
      senior-fe-planning.md    # Architecture planning for React apps
      cleanup-dev-servers.md   # Kill orphaned dev servers after tasks
      archive-completed-plan.md # Plan lifecycle management
  cursor/
    rules/
      react-monorepo.md        # Cursor rules for React/TS monorepos
    commands/
      create-test.md           # Auto-generate unit tests
  claude/
    settings.json              # Claude Code settings template
  guides/
    SETUP.md                   # How to install everything
    CUSTOMIZATION.md           # How to adapt for your project
    SELLING.md                 # How to sell your own starter kits
```

## Who Is This For?

- **Frontend engineers** working with React, TypeScript, TanStack Query, Jotai, shadcn/ui
- **Teams** that use monorepos (pnpm/npm/yarn workspaces)
- **Developers** using AI coding tools (OpenCode, Cursor, Claude Code) who want to stop wasting time configuring them
- **Anyone** who wants their AI agent to follow their project's conventions automatically

## What Problems Does This Solve?

| Problem                                            | Solution                                                     |
| -------------------------------------------------- | ------------------------------------------------------------ |
| AI agent commits to main branch                    | `worktree-enforcer` forces isolated branches                 |
| PRs fail CI because of lint/format                 | `pre-pr-lint-gate` + `format-guard` catch issues locally     |
| Linear/Jira tickets aren't linked to PRs           | `linear-pr-tracker` auto-extracts and links them             |
| AI agent leaves dev servers running                | `cleanup-dev-servers` kills orphaned processes               |
| AI agent writes code that doesn't match your style | `react-monorepo` rules enforce your conventions              |
| Planning features takes too long                   | `senior-fe-planning` generates structured architecture plans |
| Completed plans pile up                            | `archive-completed-plan` keeps your workspace clean          |

## Quick Start

### 1. Copy configs to your project

```bash
# Clone the kit
git clone https://github.com/hbenitez13/ai-dev-starter-kit.git

# Copy OpenCode skills to your project
cp -r ai-dev-starter-kit/opencode/skills/ your-project/.opencode/skills/

# Copy Cursor rules
cp ai-dev-starter-kit/cursor/rules/react-monorepo.md your-project/.cursorrules

# Copy Claude settings
cp ai-dev-starter-kit/claude/settings.json your-project/.claude/settings.json
```

### 2. Customize for your project

Open each file and replace the placeholder values:

- `{YOUR_PROJECT}` - your project name
- `{YOUR_TEAM_PREFIX}` - your Linear/Jira prefix (e.g., `ENG`, `FE`)
- `{YOUR_BUILD_CMD}` - your build command
- `{YOUR_LINT_CMD}` - your lint command

See [CUSTOMIZATION.md](guides/CUSTOMIZATION.md) for the full guide.

### 3. Start using

Your AI coding tools will automatically pick up the configs. Try:

- "Create a PR for this work" (triggers `linear-pr-tracker`)
- "Plan the architecture for this feature" (triggers `senior-fe-planning`)
- "Run pre-PR checks" (triggers `format-guard` + `pre-pr-lint-gate`)

## Skills Overview

### `linear-pr-tracker`

Automatically extracts ticket IDs from commits and branch names, then includes them in PR descriptions using Linear/Jira's magic word syntax. PRs auto-transition ticket statuses on merge.

### `pre-pr-lint-gate`

Fail-fast ESLint validation. Runs your shared package lint first, then workspace-wide. Stops at the first error with an actionable fix recommendation.

### `format-guard`

Ensures Prettier formatting passes before push. Auto-fixes formatting issues and stages the changes for your next commit.

### `worktree-enforcer`

Forces all work to happen in git worktrees, never on main. Every task gets an isolated branch and workspace. Prevents accidental commits to main and enables parallel development.

### `git-worktree-master`

Complete reference for git worktree workflows: security patches, feature development, bug fixes, code reviews, and risky refactors. Includes naming conventions, troubleshooting, and cleanup procedures.

### `senior-fe-planning`

Generates structured architecture plans for frontend features. Covers data fetching (TanStack Query), state management (Jotai), UI components (shadcn/Tailwind), and file organization.

### `cleanup-dev-servers`

Kills orphaned Vite/Next.js dev servers and tmux sessions after task completion. Prevents port conflicts and resource waste.

### `archive-completed-plan`

Moves completed plans to an archive directory, keeping your active workspace clean and showing only in-flight work.

## Customization

Every skill is designed to be customized. The configs use generic patterns that work with most React/TypeScript projects. See the [customization guide](guides/CUSTOMIZATION.md) to adapt them for:

- Different package managers (npm, yarn, pnpm, bun)
- Different monorepo tools (Turborepo, Nx, Lerna)
- Different CI/CD systems
- Different project management tools (Linear, Jira, GitHub Issues)
- Different tech stacks (Next.js, Remix, Vite, etc.)

## Pro Version

Want more? The **[AI Dev Starter Kit Pro](https://hbenitez13.gumroad.com/l/ai-dev-starter-kit)** includes everything above plus:

- **Full PR Pipeline** skill - one command runs format, lint, typecheck, build, test, and creates the PR
- **Dependency Updater** skill - safe, isolated dependency updates with full verification
- **Code Review Assistant** skill - automated pre-review checklist before human review
- **Interactive Setup Wizard** - customize all skills for your project in 30 seconds (no manual find-and-replace)
- **Fully configured example** - see exactly what a complete setup looks like for React + Vite + pnpm

## License

MIT - Use it, sell it, modify it, share it. See [LICENSE](LICENSE).

---

Built by [@hbenitez13](https://github.com/hbenitez13) from real production workflows at an AI startup.
