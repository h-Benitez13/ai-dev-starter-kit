# Git Worktree Master

**Domain**: Git workflow optimization, isolated development environments

**When to Use This Skill**:

- Security patches that need isolation
- Feature development without disrupting main workspace
- Bug fixes requiring clean environment
- Dependency updates (especially risky major version bumps)
- Parallel work on multiple features/branches
- Code reviews requiring local testing

**Trigger Phrases**: "use worktree", "isolated workspace", "parallel branches"

---

## Philosophy

**Core Principle**: Your main repository should remain stable and clean. Use worktrees to create isolated, purpose-specific workspaces that can be destroyed without affecting your primary environment.

**Benefits**:

- No branch switching = no losing uncommitted work
- Multiple branches checked out simultaneously = parallel workflows
- Clean environment per task = reduced dependency conflicts
- Easy cleanup = just delete the worktree directory

---

## Standard Worktree Patterns

### Pattern 1: Security / Dependency Fixes

```bash
git worktree add -b fix/security-{issue} ~/{repo}-security-fixes main
```

### Pattern 2: Feature Development

```bash
git worktree add -b feat/{feature-name} ~/{repo}-feat-{feature} main
```

### Pattern 3: Bug Fixes

```bash
git worktree add -b fix/{bug-name} ~/{repo}-fix-{bug} main
```

### Pattern 4: Risky Refactors

```bash
git worktree add -b refactor/{area} ~/{repo}-refactor-{area} main
```

### Pattern 5: Code Review Testing

```bash
git worktree add ~/{repo}-review-{pr-number}
cd ~/{repo}-review-{pr-number}
gh pr checkout {pr-number}
```

---

## Worktree Operations Reference

### Creating

```bash
# From main branch
git worktree add -b {new-branch} {path} main

# From current branch
git worktree add -b {new-branch} {path} HEAD

# Checkout existing branch
git worktree add {path} {existing-branch}
```

### Managing

```bash
# List all worktrees
git worktree list

# Detailed info
git worktree list --porcelain

# Lock (prevent accidental removal)
git worktree lock {path}
```

### Cleanup

```bash
# Remove worktree (must be clean)
git worktree remove {path}

# Force remove
git worktree remove --force {path}

# Delete branch after removal
git branch -d {branch-name}

# Prune stale references
git worktree prune
```

---

## Agent Instructions

### Worktree Creation Checklist

Before creating a worktree, ALWAYS verify:

- [ ] Path doesn't already exist: `ls {path}` returns "No such file"
- [ ] Branch doesn't already exist: `git branch -a | grep {branch}` returns nothing
- [ ] Main repo is clean (or user is okay leaving it)

### Post-Creation Steps

```bash
# 1. Install dependencies in worktree
{YOUR_INSTALL_CMD}

# 2. Verify it builds
{YOUR_BUILD_CMD}

# 3. Start working
```

### Best Practices

**DO**:

- Create worktrees in home directory (`~/`) for consistency
- Use descriptive names: `{repo}-{type}-{slug}`
- Clean up immediately after PR merge
- Run `git worktree list` before creating new ones
- Install dependencies in each worktree (they need their own node_modules)

**DON'T**:

- Nest worktrees inside the main repo
- Forget to push before creating PR
- Remove worktrees with uncommitted work
- Let worktrees accumulate

### Troubleshooting

| Error                                 | Solution                                             |
| ------------------------------------- | ---------------------------------------------------- |
| `'path' already exists`               | Remove existing directory or choose different path   |
| `invalid reference: 'branch'`         | Source branch doesn't exist. Use `main`              |
| `'branch' is already checked out`     | Can't checkout same branch in multiple worktrees     |
| Worktree in `list` but directory gone | `git worktree prune`                                 |
| `install` fails in worktree           | Check environment variables are set in current shell |

---

## Quick Reference

```bash
# CREATE
git worktree add -b {branch} {path} main

# LIST
git worktree list

# REMOVE
git worktree remove {path}
git branch -d {branch}

# CLEANUP STALE
git worktree prune

# STANDARD PATHS
~/{repo}-security-fixes     # Security updates
~/{repo}-feat-{name}        # Features
~/{repo}-fix-{name}         # Bug fixes
~/{repo}-refactor-{area}    # Refactors
~/{repo}-review-{pr-num}    # PR reviews
```

---

**Skill Version**: 1.0.0
