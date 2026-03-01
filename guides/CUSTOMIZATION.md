# Customization Guide

Every file in this kit uses placeholder values. Replace them with your project's actual commands and conventions.

## Placeholder Reference

### Universal Placeholders

| Placeholder              | Description       | Example                      |
| ------------------------ | ----------------- | ---------------------------- |
| `{YOUR_PROJECT}`         | Your project name | `my-app`                     |
| `{YOUR_TEAM_PREFIX}`     | Ticket prefix     | `ENG`, `FE`, `PROJ`          |
| `{YOUR_PACKAGE_MANAGER}` | Package manager   | `pnpm`, `npm`, `yarn`, `bun` |

### Command Placeholders

| Placeholder               | Description       | pnpm Example                       | npm Example                       | yarn Example                   |
| ------------------------- | ----------------- | ---------------------------------- | --------------------------------- | ------------------------------ |
| `{YOUR_BUILD_CMD}`        | Build all         | `pnpm run build`                   | `npm run build`                   | `yarn build`                   |
| `{YOUR_LINT_CMD}`         | Lint all          | `pnpm -r lint`                     | `npm run lint`                    | `yarn lint`                    |
| `{YOUR_LINT_CMD_SHARED}`  | Lint shared first | `pnpm --filter @myorg/shared lint` | `npm run lint --workspace=shared` | `yarn workspace shared lint`   |
| `{YOUR_LINT_CMD_ALL}`     | Lint everything   | `pnpm -r lint`                     | `npm run lint --workspaces`       | `yarn workspaces foreach lint` |
| `{YOUR_FORMAT_CHECK_CMD}` | Check formatting  | `pnpm format:check`                | `npm run format:check`            | `yarn format:check`            |
| `{YOUR_FORMAT_FIX_CMD}`   | Fix formatting    | `pnpm format:fix`                  | `npm run format:fix`              | `yarn format:fix`              |
| `{YOUR_TYPECHECK_CMD}`    | TypeScript check  | `pnpm run typecheck`               | `npm run typecheck`               | `yarn typecheck`               |
| `{YOUR_TEST_CMD}`         | Run tests         | `pnpm run test`                    | `npm test`                        | `yarn test`                    |
| `{YOUR_INSTALL_CMD}`      | Install deps      | `pnpm install`                     | `npm install`                     | `yarn install`                 |
| `{YOUR_FILTER_CMD}`       | Filter workspace  | `pnpm --filter`                    | `npm run --workspace=`            | `yarn workspace`               |

### Path Placeholders

| Placeholder        | Description            | Example                                   |
| ------------------ | ---------------------- | ----------------------------------------- |
| `{YOUR_PLANS_DIR}` | Where plans are stored | `.sisyphus/plans`, `.plans`, `docs/plans` |

## Quick Customization Script

Run this to do a bulk find-and-replace across all skill files:

```bash
# Navigate to your project's skills directory
cd .opencode/skills

# Replace team prefix
sed -i '' 's/{YOUR_TEAM_PREFIX}/ENG/g' *.md

# Replace package manager commands (pnpm example)
sed -i '' 's/{YOUR_BUILD_CMD}/pnpm run build/g' *.md
sed -i '' 's/{YOUR_LINT_CMD_SHARED}/pnpm --filter @myorg\/shared lint/g' *.md
sed -i '' 's/{YOUR_LINT_CMD_ALL}/pnpm -r lint/g' *.md
sed -i '' 's/{YOUR_LINT_CMD}/pnpm -r lint/g' *.md
sed -i '' 's/{YOUR_FORMAT_CHECK_CMD}/pnpm format:check/g' *.md
sed -i '' 's/{YOUR_FORMAT_FIX_CMD}/pnpm format:fix/g' *.md
sed -i '' 's/{YOUR_TYPECHECK_CMD}/pnpm run typecheck/g' *.md
sed -i '' 's/{YOUR_TEST_CMD}/pnpm run test/g' *.md
sed -i '' 's/{YOUR_INSTALL_CMD}/pnpm install/g' *.md
sed -i '' 's/{YOUR_PLANS_DIR}/.plans/g' *.md

# Also update Cursor rules
cd ../../
sed -i '' 's/{YOUR_BUILD_CMD}/pnpm run build/g' .cursorrules
# ... repeat for other placeholders
```

## Customization by Tool

### Linear vs Jira vs GitHub Issues

**Linear**: Works out of the box. The `linear-pr-tracker` skill uses Linear's magic word syntax.

**Jira**: Replace `Closes {PREFIX}-1234` with Jira's syntax:

- Jira uses the same magic words as Linear
- Change the ticket regex from `[A-Z]{2,5}-[0-9]+` if your pattern differs

**GitHub Issues**: Replace ticket references with `Closes #1234` syntax:

- Change regex to `#[0-9]+`
- Use `Closes #1234` instead of `Closes ENG-1234`

### Monorepo vs Single App

**Monorepo** (default): The `pre-pr-lint-gate` skill has a two-step lint process (shared first, then all). Keep this as-is.

**Single app**: Simplify to a single lint command. Remove the shared lint step.

### Tech Stack Variants

**Next.js**: Update the `senior-fe-planning` skill's tech stack table. Add Next.js-specific patterns (App Router, Server Components, etc.).

**Remix**: Similar to Next.js. Update data fetching patterns to use loaders/actions.

**Vite SPA** (default): Works as-is. The skills assume a Vite-based SPA setup.

## Adding Custom Skills

The best skills come from your own workflow pain points. To create a new skill:

1. Copy any existing skill as a template
2. Follow this structure:

```markdown
# Skill Name

**Domain**: What area this skill covers

**When to Use This Skill**:

- Specific trigger conditions

**Trigger Phrases**: "keyword1", "keyword2"

---

## Philosophy

Why this skill exists

## Agent Instructions

Step-by-step protocol

## Quick Reference

Cheat sheet

---

**Skill Version**: 1.0.0
```

3. Place it in `.opencode/skills/`
4. Test it by asking your agent to use it

## Tips

- **Start generic, then specialize**: Use the kit as-is first, then customize as you discover what doesn't fit.
- **Version your skills**: When you update a skill, bump the version at the bottom.
- **Share with your team**: These configs work best when the whole team uses them. Commit them to your repo.
