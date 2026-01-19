# Claude Code Commands Reference

Complete reference for Claude Code slash commands and keyboard shortcuts (January 2026).

## Essential Commands

### `/init`
**Purpose:** Initialize Claude Code for your project

```
/init
```

What it does:
- Scans your codebase for structure and patterns
- Detects frameworks, languages, and dependencies
- Generates a `CLAUDE.md` file with project context
- Sets up initial configuration

When to use:
- First time using Claude Code in a project
- After major project restructuring

---

### `/compact`
**Purpose:** Compress conversation history to free context

```
/compact
```

What it does:
- Summarizes the current conversation
- Preserves key decisions and context
- Frees up token space for new work

When to use:
- Context indicator shows 75%+ usage
- Mid-task when running long but need continuity
- Before starting a complex new subtask

What's preserved:
- Key decisions made
- Current task state
- Important file references

What's lost:
- Detailed conversation history
- Exact wording of previous exchanges

---

### `/clear`
**Purpose:** Fresh conversation while keeping project memory

```
/clear
```

What it does:
- Wipes current conversation history
- Retains CLAUDE.md instructions
- Keeps project configuration loaded

When to use:
- Switching to a completely different task
- After completing a major feature
- When conversation got off track

**`/clear` vs New Session:**

| Scenario | Use `/clear` | Start New Session |
|----------|--------------|-------------------|
| Switching tasks, same project | ✅ | |
| Context is corrupted/confused | | ✅ |
| Testing fresh CLAUDE.md changes | | ✅ |
| Just finished a big feature | ✅ | |
| Claude is stuck in a loop | | ✅ |

---

### `/help`
**Purpose:** Get help with Claude Code

```
/help
```

Shows available commands and usage information.

---

### `/config`
**Purpose:** View and modify settings

```
/config
```

Opens configuration interface for:
- Model selection
- Permission settings
- Tool allowlists/blocklists
- MCP server management

---

### `/cost`
**Purpose:** View token usage and costs

```
/cost
```

Shows:
- Tokens used in current session
- Estimated cost
- Context window utilization

---

### `/review`
**Purpose:** Review pending changes

```
/review
```

Lists all file changes Claude has proposed, allowing you to:
- Accept individual changes
- Reject changes
- Request modifications

---

### `/undo`
**Purpose:** Undo last change

```
/undo
```

Reverts the most recent file modification made by Claude.

---

### `/diff`
**Purpose:** Show changes in diff format

```
/diff
```

Displays all pending changes in unified diff format for review.

---

## File Reference Syntax

### `@filename`
Reference specific files to load into context:

```
@src/components/Button.tsx
@package.json
```

### Glob patterns
```
@src/components/*.tsx
@**/*.test.ts
```

**Warning:** Using `@` on large directories can fill context quickly. Start specific, expand if needed.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Shift+Tab` | Toggle Plan Mode |
| `Shift+Enter` | New line in prompt |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+D` | Exit Claude Code |
| `Up Arrow` | Previous prompt |
| `Down Arrow` | Next prompt |
| `Tab` | Accept suggestion |
| `Escape` | Dismiss suggestion |

---

## Interaction Modes

### Default Mode
Normal conversational execution:
- Claude reads, reasons, and acts
- Good for small tasks, exploration, Q&A
- Immediate execution of changes

### Plan Mode (`Shift+Tab`)
Think-before-acting mode:
- Claude proposes a step-by-step plan
- You review and approve before execution
- Changes only happen after approval

When to use Plan Mode:
- Refactors touching multiple files
- Unfamiliar codebases
- Any task where "undo" would be painful
- Complex feature implementations

### Extended Thinking
Enable deeper reasoning for complex problems:
- Architecture decisions
- Debugging subtle issues
- Trade-off analysis

Enable via settings or per-session flag.

---

## MCP Commands

### View MCP Status
```
/mcp status
```

Shows which MCP servers are connected and available.

### Restart MCP Server
```
/mcp restart [server-name]
```

Restarts a specific MCP server if it's having issues.

---

## Tips for Effective Command Usage

1. **Use `/compact` proactively** - Don't wait until context is full

2. **Reference files with `@`** instead of pasting code - Keeps context cleaner

3. **Use Plan Mode for risky changes** - Better to review than to undo

4. **Start new sessions for fresh starts** - Sometimes `/clear` isn't enough

5. **Check `/cost` periodically** - Especially on long tasks

6. **Use `/review` before committing** - Catch issues before they're in git

---

## Command Quick Reference

| Command | Purpose |
|---------|---------|
| `/init` | Initialize project |
| `/compact` | Compress conversation |
| `/clear` | Fresh conversation |
| `/help` | Show help |
| `/config` | Settings |
| `/cost` | Usage stats |
| `/review` | Review changes |
| `/undo` | Undo last change |
| `/diff` | Show changes |
| `/mcp status` | MCP server status |
