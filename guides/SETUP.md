# Setup Guide

How to install the AI Dev Starter Kit in your project.

## Prerequisites

- A React/TypeScript project (monorepo or single app)
- At least one AI coding tool: OpenCode, Cursor, or Claude Code
- Git (version 2.5+ for worktree support)

## Installation

### Option 1: Copy Everything (Recommended)

```bash
# Clone the starter kit
git clone https://github.com/h-Benitez13/ai-dev-starter-kit.git /tmp/ai-kit

# Copy OpenCode skills
mkdir -p .opencode/skills
cp /tmp/ai-kit/opencode/skills/*.md .opencode/skills/

# Copy Cursor rules
cp /tmp/ai-kit/cursor/rules/react-monorepo.md .cursorrules

# Copy Cursor commands
mkdir -p .cursor/commands
cp /tmp/ai-kit/cursor/commands/*.md .cursor/commands/

# Copy Claude settings
mkdir -p .claude
cp /tmp/ai-kit/claude/settings.json .claude/settings.json

# Cleanup
rm -rf /tmp/ai-kit
```

### Option 2: Cherry-Pick Skills

Only want specific skills? Copy just what you need:

```bash
# Just the PR workflow skills
cp /tmp/ai-kit/opencode/skills/linear-pr-tracker.md .opencode/skills/
cp /tmp/ai-kit/opencode/skills/pre-pr-lint-gate.md .opencode/skills/
cp /tmp/ai-kit/opencode/skills/format-guard.md .opencode/skills/

# Just the git worktree skills
cp /tmp/ai-kit/opencode/skills/worktree-enforcer.md .opencode/skills/
cp /tmp/ai-kit/opencode/skills/git-worktree-master.md .opencode/skills/
```

## Post-Installation

After copying the files, you MUST customize them for your project. See [CUSTOMIZATION.md](CUSTOMIZATION.md).

## Verification

To verify the skills are loaded:

### OpenCode

Ask your agent: "What skills do you have available?"
It should list all the skills you copied.

### Cursor

Open a new chat and ask: "What are the cursor rules for this project?"
It should reference the rules from `.cursorrules`.

### Claude Code

The `.claude/settings.json` is automatically picked up on next session start.

## Updating

To update to the latest version:

```bash
git clone https://github.com/h-Benitez13/ai-dev-starter-kit.git /tmp/ai-kit
# Compare and merge new changes with your customized versions
diff -r .opencode/skills /tmp/ai-kit/opencode/skills
# Manually merge desired updates
rm -rf /tmp/ai-kit
```

## Troubleshooting

### Skills not showing up in OpenCode

- Ensure files are in `.opencode/skills/` (not `.opencode/skill/`)
- Restart your OpenCode session

### Cursor rules not applying

- Ensure the file is named `.cursorrules` (not `.cursor-rules`)
- Restart Cursor

### Commands not appearing in Cursor

- Ensure files are in `.cursor/commands/`
- Restart Cursor and check the command palette
