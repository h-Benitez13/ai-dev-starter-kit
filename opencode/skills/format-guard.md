# Format Guard

**Domain**: Code formatting enforcement, Prettier validation, PR readiness

**When to Use This Skill**:

- Proactively after ANY file edits during development
- Before push or PR creation
- When the user mentions format check, Prettier, or failed format validation in CI

**Trigger Phrases**: "format check", "prettier", "format fix", "PR readiness"

---

## Philosophy

**Core Principle**: PR validation runs format checks. This skill ensures formatting passes locally so PRs don't fail on the first attempt.

---

## Agent Instructions

### Required Command Order

1. Run format check from repo root:

```bash
{YOUR_FORMAT_CHECK_CMD}
# Example: pnpm format:check
# Example: npx prettier --check "**/*.{js,jsx,ts,tsx,json,md}"
```

2. If step 1 fails, run format fix:

```bash
{YOUR_FORMAT_FIX_CMD}
# Example: pnpm format:fix
# Example: npx prettier --write "**/*.{js,jsx,ts,tsx,json,md}"
```

3. Re-run format check to verify:

```bash
{YOUR_FORMAT_CHECK_CMD}
```

4. If files were reformatted, stage them:

```bash
git add <paths reported by Prettier>
```

### Enforcement Rules

- If step 1 passes: report PASS; no further action.
- If step 1 fails: run step 2, then step 3. Only mark PASS when step 3 exits 0.
- If step 3 still fails after fix: report FAIL and surface Prettier output.
- Run from the repository root so the same globs as CI are used.

### Reporting Format

**PASS (no changes)**:

- `Status`: PASS
- `Result`: All files use correct code style. Safe to push/PR.

**PASS (after fix)**:

- `Status`: PASS
- `Action`: Ran format fix; re-ran format check.
- `Files reformatted`: list of file paths that were rewritten.
- `Result`: Formatting fixed and staged. Include in your next commit.

**FAIL**:

- `Status`: FAIL
- `Actionable Error`: relevant Prettier output
- `Next Step`: Fix syntax errors first, then re-run formatting.

### Error Handling

- If Prettier fails on a file (syntax error), report the file so the agent can fix syntax first.
- Never skip formatting -- PR validation will fail without it.

---

## Commands Reference

```bash
# Format all files
{YOUR_FORMAT_FIX_CMD}

# Check formatting (validation only)
{YOUR_FORMAT_CHECK_CMD}

# Format specific files
npx prettier --write "path/to/file.ts"

# Check specific files
npx prettier --check "path/to/file.ts"
```

---

## Integration

This skill complements `pre-pr-lint-gate`. For full pre-PR validation:

1. **Format guard** (this skill) -- Prettier
2. **Lint gate** -- ESLint
3. **Build validation** -- your build command

---

**Skill Version**: 1.0.0
