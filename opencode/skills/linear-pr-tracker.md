# Linear / Jira PR Tracker

**Domain**: Ticket tracking, PR creation, automated status transitions

**When to Use This Skill**:

- Creating a PR that addresses one or more tickets
- Committing work that references tickets ({YOUR_TEAM_PREFIX}-XXXX)
- Any PR creation flow where ticket status should auto-transition

**Trigger Phrases**: "create PR", "ticket", "{YOUR_TEAM_PREFIX}-", "close ticket", "link ticket"

---

## Philosophy

**Core Principle**: Every PR that addresses tickets MUST reference them in the PR description using magic word syntax. This enables your project management tool's GitHub integration to automatically transition ticket statuses without manual updates.

---

## How It Works

Your PM tool's GitHub integration reads issue references from **four locations**:

| Location            | Auto-Links | Triggers Status Change | Best For                        |
| ------------------- | ---------- | ---------------------- | ------------------------------- |
| **PR description**  | Yes        | Yes (with magic words) | Multiple tickets, most reliable |
| **PR title**        | Yes        | Yes                    | Single ticket, quick reference  |
| **Branch name**     | Yes        | Partial (links only)   | Auto-linking during development |
| **Commit messages** | Yes        | Yes (if enabled)       | Granular per-commit tracking    |

**PR description is the canonical location.** Always include ticket references there.

---

## Magic Words Reference

### Closing Magic Words (move ticket to Done on merge)

```
Closes {YOUR_TEAM_PREFIX}-1234
Fixes {YOUR_TEAM_PREFIX}-1234
Resolves {YOUR_TEAM_PREFIX}-1234
```

All tenses work: `close`, `closes`, `closed`, `closing`, etc.

### Non-Closing Magic Words (link without closing)

```
Part of {YOUR_TEAM_PREFIX}-1234
Related to {YOUR_TEAM_PREFIX}-1234
Refs {YOUR_TEAM_PREFIX}-1234
```

### Multiple Tickets

```
Closes {YOUR_TEAM_PREFIX}-1234, {YOUR_TEAM_PREFIX}-1235, {YOUR_TEAM_PREFIX}-1236
```

---

## PR Description Template

```markdown
## Summary

<1-3 sentence description of what this PR does>

## Tickets

Closes {YOUR_TEAM_PREFIX}-XXXX
Closes {YOUR_TEAM_PREFIX}-YYYY

## Changes

- <change 1>
- <change 2>

## Testing

- <how it was verified>
```

---

## Agent Instructions

### 1. Ticket Extraction Protocol

Before creating any PR, extract ticket IDs from ALL sources:

```bash
# Get all commits on this branch not on main
git log main..HEAD --oneline

# Extract ticket IDs (customize regex for your prefix)
git log main..HEAD --oneline | grep -oE '[A-Z]{2,5}-[0-9]+' | sort -u

# Check branch name for ticket IDs
git branch --show-current | grep -oE '[A-Za-z]+-[0-9]+' | tr '[:lower:]' '[:upper:]'
```

### 2. Ticket Classification

For each extracted ticket:

1. **PR fully addresses the ticket** -> `Closes {YOUR_TEAM_PREFIX}-XXXX`
2. **PR is a partial contribution** -> `Part of {YOUR_TEAM_PREFIX}-XXXX`
3. **Unsure** -> Default to `Closes` (can be edited in PR)

### 3. PR Body Construction

**MANDATORY**: Every PR description MUST include a `## Tickets` section.

```bash
gh pr create --title "fix(scope): description ({YOUR_TEAM_PREFIX}-1234)" --body "$(cat <<'EOF'
## Summary
<summary>

## Tickets
Closes {YOUR_TEAM_PREFIX}-1234

## Changes
- <changes>
EOF
)"
```

### 4. Commit Message Convention

```
<type>(<scope>): <description> ({YOUR_TEAM_PREFIX}-XXXX)
```

Examples:

```
fix(auth): fix redirect on refresh (ENG-1918, ENG-1919)
feat(dashboard): add export button (FE-1885)
```

### 5. Verification Checklist

- [ ] All ticket IDs from commits are present in the PR description
- [ ] Each ticket has the appropriate magic word
- [ ] Ticket IDs appear in the PR title
- [ ] `## Tickets` section uses one ticket per line with magic word prefix

---

## Quick Reference

```bash
# Extract tickets from commits
git log main..HEAD --oneline | grep -oE '[A-Z]{2,5}-[0-9]+' | sort -u

# Extract ticket from branch name
git branch --show-current | grep -oE '[A-Za-z]+-[0-9]+' | tr '[:lower:]' '[:upper:]'

# Magic words
Closes {PREFIX}-XXXX           # Auto-closes on merge
Part of {PREFIX}-YYYY          # Links but doesn't close
Refs {PREFIX}-ZZZZ             # Weak reference
```

---

**Skill Version**: 1.0.0
