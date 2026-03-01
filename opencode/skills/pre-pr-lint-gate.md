# Pre-PR Lint Gate

**Domain**: ESLint validation, fail-fast lint enforcement, PR readiness

**When to Use This Skill**:

- Agent review before push or PR creation
- When the user asks to validate before push
- When the user mentions lint gate or lint failures

**Trigger Phrases**: "lint gate", "pre-PR", "lint check", "before push", "PR readiness"

---

## Philosophy

**Core Principle**: Catch lint failures locally before code is pushed. PR validation runs the same lint commands -- this skill ensures they pass first.

---

## Agent Instructions

### Required Command Order (Fail-Fast)

1. Run shared/core package lint first:

```bash
{YOUR_LINT_CMD_SHARED}
# Example: pnpm --filter @myorg/shared lint
# Example: npx turbo run lint --filter=shared
```

2. Only if step 1 succeeds, run full workspace lint:

```bash
{YOUR_LINT_CMD_ALL}
# Example: pnpm -r lint
# Example: npx turbo run lint
# Example: npm run lint
```

### Enforcement Rules

- If step 1 fails: **Stop immediately.** Do not run step 2.
- If step 2 fails: **Stop push/PR preparation.**
- Only mark lint validation as passed when **both** commands exit with code 0.

### Reporting Format

**PASS**:

- `Status`: PASS
- `Checks`: `{YOUR_LINT_CMD_SHARED}`, `{YOUR_LINT_CMD_ALL}`
- `Result`: Both lint commands passed. Safe to continue with push/PR.

**FAIL**:

- `Status`: FAIL
- `Failed Command`: exact command that failed
- `Failing Scope`: package/workspace that reported the error
- `Actionable Error`: first relevant lint error line
- `Next Step`: one concise fix recommendation, then rerun from step 1

### Retry Behavior

After fixes, rerun the full sequence from step 1 to confirm no regressions.

---

## Integration

For complete pre-PR validation (matching CI), run in this order:

1. **Format guard** (Prettier): `{YOUR_FORMAT_CHECK_CMD}`
2. **Lint gate** (this skill): shared lint then workspace lint
3. **Build**: `{YOUR_BUILD_CMD}`

---

## Customization

Replace these placeholders with your project's commands:

| Placeholder               | Example                            |
| ------------------------- | ---------------------------------- |
| `{YOUR_LINT_CMD_SHARED}`  | `pnpm --filter @myorg/shared lint` |
| `{YOUR_LINT_CMD_ALL}`     | `pnpm -r lint`                     |
| `{YOUR_FORMAT_CHECK_CMD}` | `pnpm format:check`                |
| `{YOUR_BUILD_CMD}`        | `pnpm run build`                   |

---

**Skill Version**: 1.0.0
