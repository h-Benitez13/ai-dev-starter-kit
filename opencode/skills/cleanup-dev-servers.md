# Cleanup Dev Servers

**Domain**: Local dev server lifecycle management, post-implementation cleanup

**When to Use This Skill**:

- After completing testing and validation of an implementation
- When a task's implementation and verification are done
- Before marking a task as complete or creating a PR
- When the user says they're done testing

**Trigger Phrases**: "done testing", "kill servers", "stop dev", "cleanup servers", "finished"

---

## Philosophy

**Core Principle**: Dev servers started during implementation must be killed before a task is marked complete. Orphaned servers waste resources, hold ports, and cause confusing failures on the next task.

---

## Agent Instructions

### Step 1: Find Running Dev Servers

```bash
# Find processes on common dev ports
lsof -i :3000 -i :3001 -i :4173 -i :5173 -i :6173 -i :8080 -t 2>/dev/null

# Find node/dev processes
ps aux | grep -E '(vite|next dev|pnpm.*dev|npm.*dev|webpack.*dev)' | grep -v grep
```

Customize these ports for your project:

| Port   | App                    |
| ------ | ---------------------- |
| `3000` | Default (Next.js, CRA) |
| `5173` | Vite default           |
| `8080` | Webpack dev server     |

### Step 2: Kill Dev Server Processes

```bash
# Kill by port (surgical)
lsof -i :5173 -t 2>/dev/null | xargs -r kill
lsof -i :3000 -t 2>/dev/null | xargs -r kill

# Or kill all common dev ports at once
lsof -i :3000 -i :3001 -i :4173 -i :5173 -i :6173 -i :8080 -t 2>/dev/null | xargs -r kill
```

### Step 3: Kill Tmux Sessions

```bash
# List active tmux sessions
tmux list-sessions 2>/dev/null

# Kill dev server sessions
tmux list-sessions -F '#{session_name}' 2>/dev/null | grep -iE '(dev|server|web)' | xargs -I{} tmux kill-session -t {}
```

### Step 4: Verify Cleanup

```bash
# Verify ports are free
lsof -i :3000 -i :5173 -i :8080 2>/dev/null || echo "All dev ports are free"

# Verify no orphaned processes
ps aux | grep -E '(vite|next dev|pnpm.*dev)' | grep -v grep || echo "No dev server processes running"
```

### Step 5: Report

Note which servers/ports were killed and confirm all ports are free.

---

## Integration

This cleanup should happen as the LAST step before:

1. Marking the final todo as complete
2. Reporting task completion
3. Creating a PR

---

**Skill Version**: 1.0.0
