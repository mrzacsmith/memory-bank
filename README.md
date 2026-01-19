# AI Coding Assistant Memory & Configuration

A comprehensive guide for adding persistent memory and configuration to AI coding assistants, covering both **Cursor IDE** and **Claude Code CLI**.

## Repository Structure

```
.
├── cursor/                     # Cursor IDE resources
│   ├── memory-bank.md          # Memory bank setup guide
│   └── rules/                  # Example .mdc rule files
│
├── claude/                     # Claude Code CLI resources
│   ├── CLAUDE.md               # Memory file guide
│   ├── COMMANDS.md             # Slash commands reference
│   ├── HOOKS.md                # Lifecycle hooks guide
│   ├── settings-example.json   # Settings configuration
│   ├── mcp-example.json        # MCP server configuration
│   └── skills/                 # Skill examples
│
└── README.md
```

---

## Cursor IDE

### What is Cursor Memory Bank?

Cursor's memory bank provides persistent project context through:
- **Memory Bank Files** - Markdown files that store project knowledge
- **Rules** - `.mdc` files that teach Cursor project-specific patterns

### Quick Setup

1. Create a `memory-bank/` directory with core files:
   - `projectbrief.md` - Foundation document
   - `productContext.md` - Project purpose
   - `activeContext.md` - Current work focus
   - `systemPatterns.md` - Architecture patterns
   - `techContext.md` - Tech stack details
   - `progress.md` - Status tracking

2. Create `.cursor/rules/` directory for rule files

3. Add a `.cursorrules` file (see `cursor/memory-bank.md` for template)

### Documentation
- See [`cursor/memory-bank.md`](cursor/memory-bank.md) for detailed setup

---

## Claude Code CLI

### What is Claude Code?

Claude Code is Anthropic's official CLI for Claude, providing:
- **CLAUDE.md** - Project memory file
- **Skills** - Reusable workflow templates
- **MCP** - Model Context Protocol for external integrations
- **Slash Commands** - Built-in commands like `/init`, `/compact`, `/clear`
- **Hooks** - Automated lifecycle events

### Quick Setup

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Initialize a project
claude
/init
```

### Memory (CLAUDE.md)

The `CLAUDE.md` file is automatically loaded at the start of every conversation:

```markdown
# My Project

Brief description of the project.

## Tech Stack
- Next.js 15
- TypeScript
- Tailwind CSS

## Commands
- `npm run dev` - Start dev server
- `npm run test` - Run tests

## Conventions
- Use functional components
- Prefer named exports
```

**Locations:**
- `./CLAUDE.md` or `./.claude/CLAUDE.md` - Project-specific
- `~/.claude/CLAUDE.md` - Global (all projects)

### Skills

Skills are reusable workflows in `.claude/skills/` or `/skills/`:

```
skills/
└── pr-review/
    ├── SKILL.md           # Main instructions
    ├── checklist.md       # Reference material
    └── examples.md        # Good/bad examples
```

Skills are invoked automatically based on triggers or explicitly with `@skill-name`.

### MCP (Model Context Protocol)

MCP connects Claude to external tools. Configure in `.claude/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "${GITHUB_TOKEN}" }
    }
  }
}
```

**Popular MCP Servers:**
- `server-github` - GitHub API access
- `server-playwright` - Browser automation
- `server-postgres` - PostgreSQL access
- `server-filesystem` - File system access

### Essential Commands

| Command | Purpose |
|---------|---------|
| `/init` | Initialize project with CLAUDE.md |
| `/compact` | Compress conversation, free context |
| `/clear` | Fresh conversation, keep memory |
| `/help` | Show available commands |
| `/cost` | View token usage |
| `Shift+Tab` | Toggle Plan Mode |

### Documentation
- [`claude/CLAUDE.md`](claude/CLAUDE.md) - Memory file guide
- [`claude/COMMANDS.md`](claude/COMMANDS.md) - Commands reference
- [`claude/HOOKS.md`](claude/HOOKS.md) - Lifecycle hooks
- [`claude/skills/`](claude/skills/) - Skill examples

---

## Comparison: Cursor vs Claude Code

| Feature | Cursor | Claude Code |
|---------|--------|-------------|
| Interface | IDE Extension | CLI |
| Memory | memory-bank/ + .cursorrules | CLAUDE.md |
| Rules/Skills | .mdc files | SKILL.md files |
| External Tools | Limited | MCP Protocol |
| Plan Mode | N/A | Shift+Tab |
| Context Management | Manual | /compact, /clear |

### When to Use Which

**Use Cursor when:**
- You prefer an integrated IDE experience
- Working primarily with visual code editing
- Team uses Cursor as standard IDE

**Use Claude Code when:**
- You prefer terminal workflows
- Need MCP integrations (GitHub, databases, browsers)
- Want Plan Mode for complex changes
- Working in CI/CD pipelines (headless mode)

---

## Additional Resources

### Cursor
- [Cursor Documentation](https://cursor.sh/docs)
- [@mrzacsmith/cursor-rules](https://www.npmjs.com/package/@mrzacsmith/cursor-rules) - CLI for managing rules

### Claude Code
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [MCP Servers Registry](https://github.com/modelcontextprotocol/servers)

### Community Resources
- [Cascade Memory Bank](https://github.com/GreatScottyMac/cascade-memory-bank) - Windsurf memory solution

---

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - See individual files for specific terms.

---

*Last updated: January 2026*
