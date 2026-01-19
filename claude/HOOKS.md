# Claude Code Hooks

Hooks are shell commands that execute automatically in response to Claude Code events. They enable automated quality gates and custom workflows.

## Hook Configuration

Hooks are configured in `.claude/settings.json` or `~/.claude/settings.json`:

```json
{
  "hooks": {
    "PostToolUse": {
      "Write": {
        "command": "prettier --write ${file}",
        "description": "Format files after writing"
      },
      "Edit": {
        "command": "eslint --fix ${file}",
        "description": "Lint files after editing"
      }
    },
    "PreCommit": {
      "command": "npm run test:affected",
      "description": "Run affected tests before commit"
    },
    "PostSession": {
      "command": "echo 'Session ended' >> ~/.claude/session.log",
      "description": "Log session end"
    }
  }
}
```

## Available Hook Types

### PostToolUse
Triggered after specific tools are used.

**Available Tools:**
- `Write` - After file creation
- `Edit` - After file modification
- `Bash` - After command execution
- `Read` - After file reading

**Variables:**
- `${file}` - The file path that was affected
- `${tool}` - The tool name that was used

### PreCommit
Triggered before git commits.

**Use Cases:**
- Run tests
- Run linter
- Check formatting
- Validate types

### PostSession
Triggered when a Claude Code session ends.

**Use Cases:**
- Clean up temporary files
- Log session statistics
- Send notifications

### PrePrompt
Triggered before each prompt is processed.

**Use Cases:**
- Add context
- Check prerequisites
- Log prompts

## Example Configurations

### Auto-Format on Write
```json
{
  "hooks": {
    "PostToolUse": {
      "Write": {
        "command": "prettier --write ${file} && eslint --fix ${file}",
        "description": "Format and lint new files"
      }
    }
  }
}
```

### Test Before Commit
```json
{
  "hooks": {
    "PreCommit": {
      "command": "npm run typecheck && npm run test",
      "description": "Verify types and tests pass"
    }
  }
}
```

### TypeScript Check on Edit
```json
{
  "hooks": {
    "PostToolUse": {
      "Edit": {
        "command": "npx tsc --noEmit ${file}",
        "description": "Type check edited files",
        "glob": "*.ts,*.tsx"
      }
    }
  }
}
```

### Notify on Session End
```json
{
  "hooks": {
    "PostSession": {
      "command": "osascript -e 'display notification \"Claude Code session ended\" with title \"Claude Code\"'",
      "description": "macOS notification on session end"
    }
  }
}
```

## Hook Options

### `glob`
Only run hook for files matching pattern:

```json
{
  "PostToolUse": {
    "Write": {
      "command": "prettier --write ${file}",
      "glob": "*.js,*.ts,*.jsx,*.tsx"
    }
  }
}
```

### `timeout`
Maximum time (ms) for hook execution:

```json
{
  "PreCommit": {
    "command": "npm test",
    "timeout": 60000
  }
}
```

### `continueOnError`
Whether to continue if hook fails:

```json
{
  "PostToolUse": {
    "Write": {
      "command": "optional-tool ${file}",
      "continueOnError": true
    }
  }
}
```

## Best Practices

1. **Keep hooks fast** - Slow hooks interrupt workflow
2. **Use specific globs** - Don't run on every file type
3. **Handle errors gracefully** - Use `continueOnError` for non-critical hooks
4. **Log failures** - Add logging for debugging
5. **Test hooks** - Verify they work before relying on them

## Debugging Hooks

If hooks aren't working:

1. **Check syntax** - Validate JSON configuration
2. **Test command manually** - Run the command in terminal
3. **Check permissions** - Ensure commands are executable
4. **View logs** - Check Claude Code output for errors
5. **Simplify** - Start with simple hooks, add complexity

## Security Considerations

- Hooks run with your user permissions
- Be careful with `${file}` variable injection
- Don't run untrusted commands
- Review hooks in shared configurations
