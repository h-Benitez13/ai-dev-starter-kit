# Worktree Enforcer

**Domain**: Plan execution safety, main branch protection, isolated development

**When to Use This Skill**:

- EVERY time implementation work begins from a plan or task
- Before ANY code changes are made as part of a plan
- This skill is NON-OPTIONAL for plan execution

**Trigger Phrases**: "start work", "execute plan", "implement", "begin work"

---

## Philosophy

**Core Principle**: Main stays clean. ALWAYS. Every task executes in a worktree -- no exceptions. If it has a plan, it gets a worktree.

**Why**:

- Main branch is always deployable
- Parallel tasks can execute simultaneously
- Abandoned work is trivially cleaned up
- No accidental commits to main

---

## Agent Instructions

### Gate Check (MANDATORY before ANY plan execution)

#### 1. Verify You Are NOT on Main

```bash
current_branch=$(git branch --show-current)
if [ "$current_branch" = "main" ] || [ "$current_branch" = "master" ]; then
  echo "BLOCKED: Currently on $current_branch. Must create a worktree first."
else
  echo "OK: On branch $current_branch"
fi
```

**If on main -> STOP. Create a worktree before proceeding.**

#### 2. Determine Worktree Parameters

| Parameter      | Source               | Example                            |
| -------------- | -------------------- | ---------------------------------- |
| **Task name**  | Plan title or ticket | `fix-login-crash`                  |
| **Ticket IDs** | Tickets in the plan  | `ENG-1940`                         |
| **Work type**  | Plan category        | `fix`, `feat`, `refactor`, `chore` |

Derive branch name and worktree path:

```
Branch:  {type}/{descriptive-slug}
Path:    ~/{repo}-{type}-{slug}
```

**Naming conventions:**

| Work Type  | Branch Pattern          | Path Pattern               |
| ---------- | ----------------------- | -------------------------- |
| Bug fix    | `fix/{ticket-or-slug}`  | `~/{repo}-fix-{slug}`      |
| Feature    | `feat/{ticket-or-slug}` | `~/{repo}-feat-{slug}`     |
| Refactor   | `refactor/{slug}`       | `~/{repo}-refactor-{slug}` |
| Chore/deps | `chore/{slug}`          | `~/{repo}-chore-{slug}`    |

#### 3. Create the Worktree

```bash
# Verify path doesn't exist
test -d ~/{repo}-{type}-{slug} && echo "ALREADY EXISTS" || echo "OK"

# Verify branch doesn't exist
git branch -a | grep {branch-name} || echo "OK"

# Create worktree from main
git worktree add -b {branch-name} ~/{repo}-{type}-{slug} main

# Verify
git worktree list | grep {slug}
```

#### 4. Set Up the Worktree

```bash
# Install dependencies (in the worktree directory)
{YOUR_INSTALL_CMD}
# Example: pnpm install / npm install / yarn install

# Verify it builds
{YOUR_BUILD_CMD}
```

#### 5. Proceed with Work

ALL subsequent work happens in the worktree directory.

---

### End-of-Task Protocol

1. **Commit and push** from the worktree
2. **Create PR** (use `linear-pr-tracker` for ticket references)
3. **Kill dev servers** (use `cleanup-dev-servers`)
4. **Report cleanup command** to the user:

```
Worktree cleanup (run after PR is merged):
  git worktree remove ~/{repo}-{type}-{slug}
  git branch -d {branch-name}
```

Do NOT auto-remove -- the user may need it until the PR is merged.

---

### Edge Cases

**Trivial 1-line change:** Still use a worktree. 30 seconds of safety.

**Already in a worktree from same task:** Continue working.

**Different task:** Create a NEW worktree. Never mix task work.

**Path already exists:** Check `git worktree list`. If same task, reuse. If different, append `-v2`.

---

## Quick Reference

```bash
# Create worktree
git worktree add -b fix/eng-1940 ~/myapp-fix-eng-1940 main

# List all worktrees
git worktree list

# Cleanup after PR merge
git worktree remove ~/myapp-fix-eng-1940
git branch -d fix/eng-1940

# Prune stale references
git worktree prune
```

---

**Skill Version**: 1.0.0
