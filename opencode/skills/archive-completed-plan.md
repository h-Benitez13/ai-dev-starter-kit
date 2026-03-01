# Archive Completed Plan

**Domain**: Plan lifecycle management, workspace hygiene

**When to Use This Skill**:

- After a plan has been fully executed and all tasks are complete
- After a PR has been created for the work
- When the user says the plan is done

**Trigger Phrases**: "plan done", "finished plan", "archive plan", "plan complete"

---

## Philosophy

**Core Principle**: Active plans live in your plans directory. Completed plans move to a `completed/` subdirectory. A clean plans directory shows only what's in flight.

---

## Agent Instructions

### When to Run

Execute as the FINAL step after:

1. All todo items from the plan are marked complete
2. Build/lint/tests pass
3. Dev servers are cleaned up
4. PR has been created (if applicable)

### Step 1: Identify the Plan File

```bash
# List active plans (customize path for your project)
ls {YOUR_PLANS_DIR}/*.md
# Example: ls .sisyphus/plans/*.md
# Example: ls .plans/*.md
# Example: ls docs/plans/*.md
```

### Step 2: Move to Completed

```bash
# Create completed directory if needed
mkdir -p {YOUR_PLANS_DIR}/completed

# Move the plan
mv {YOUR_PLANS_DIR}/{plan-name}.md {YOUR_PLANS_DIR}/completed/{plan-name}.md
```

### Step 3: Verify

```bash
# Confirm the file moved
ls {YOUR_PLANS_DIR}/completed/{plan-name}.md

# Show remaining active plans
ls {YOUR_PLANS_DIR}/*.md 2>/dev/null || echo "No active plans remaining"
```

---

## Edge Cases

- **Plan doesn't exist**: Skip and note it.
- **`completed/` doesn't exist**: Create it with `mkdir -p`.
- **Plan partially completed**: Do NOT move. Note which tasks remain.
- **Multiple plans completed**: Move all of them. List each one.

---

## Customization

Replace `{YOUR_PLANS_DIR}` with your project's plan storage location:

| Convention | Path               |
| ---------- | ------------------ |
| Sisyphus   | `.sisyphus/plans/` |
| Simple     | `.plans/`          |
| Docs       | `docs/plans/`      |
| Root       | `plans/`           |

---

**Skill Version**: 1.0.0
