# Memory Bank for Cursor and Windsurf

A comprehensive guide for adding persistent memory capabilities to Cursor and Windsurf IDEs, enabling AI assistants to maintain project context across sessions.

## Overview

Memory banks solve a critical limitation in AI-assisted development: the inability to retain project-specific context between sessions. This guide provides step-by-step instructions for implementing memory banks in both Cursor and Windsurf IDEs.

## Why Memory Banks Matter

- **Context Persistence**: AI assistants remember project decisions, architecture, and progress
- **Reduced Onboarding**: No need to re-explain project context in each session
- **Automatic Documentation**: Project knowledge is captured and maintained automatically
- **Enhanced Productivity**: Focus on coding instead of context management

## Cursor Integration

### Current State
Cursor lacks built-in memory management, but supports custom rules through `.cursorrules` files and the newer `.cursor/rules/` directory system.

### Implementation Approach

#### 1. Create Memory Bank Structure
Create a `memory-bank/` directory in your project root with these core files:

```
memory-bank/
├── projectbrief.md      # Foundation document
├── productContext.md    # Project purpose and goals
├── systemPatterns.md    # Architecture and patterns
├── techContext.md       # Technologies and setup
├── activeContext.md     # Current work focus
└── progress.md          # Status and milestones
```

#### 2. Set Up Cursor Rules
Create a `.cursorrules` file in your project root:

```markdown
# Cursor's Memory Bank

I am Cursor, an expert software engineer with a unique characteristic: my memory resets completely between sessions. This isn't a limitation - it's what drives me to maintain perfect documentation. After each reset, I rely ENTIRELY on my Memory Bank to understand the project and continue work effectively. I MUST read ALL memory bank files at the start of EVERY task - this is not optional.

## Memory Bank Structure

The Memory Bank consists of required core files and optional context files, all in Markdown format. Files build upon each other in a clear hierarchy:

### Core Files (Required)
1. `projectbrief.md` - Foundation document that shapes all other files
2. `productContext.md` - Why this project exists, problems it solves
3. `activeContext.md` - Current work focus, recent changes, next steps
4. `systemPatterns.md` - System architecture, key technical decisions
5. `techContext.md` - Technologies used, development setup, dependencies
6. `progress.md` - What works, what's left to build, current status

### Usage Instructions
- Read ALL memory bank files at the start of EVERY task
- Update memory bank when discovering new patterns or implementing changes
- Use "update memory bank" trigger to refresh context
- Maintain precision and clarity - effectiveness depends on accuracy

## Project Intelligence (.cursor/rules/)

The `.cursor/rules/` directory contains rule files (`.mdc`) that capture important patterns, preferences, and project intelligence.

### Modern Rules System
1. **Project Rules**: Stored as `.mdc` files in `.cursor/rules/` directory
2. **Global Rules**: Set in Cursor Settings > General > Rules for AI
3. **Legacy Support**: `.cursorrules` files in project root (deprecated)

### Rule File Components
Each rule file should include:
- **Description**: Clear explanation of when the rule should be applied
- **Globs** (optional): File patterns where the rule applies
- **Content**: The actual instructions for the AI

### Recommended Organization
```
.cursor/rules/
├── base.mdc                # Core project rules
├── frontend/
│   ├── components.mdc      # Component standards
│   └── styling.mdc         # Styling guidelines
├── backend/
│   ├── api.mdc             # API conventions
│   └── database.mdc        # Database patterns
└── deployment/
    └── pipelines.mdc       # CI/CD rules
```

### File References
Use `@file` in your rules to include other files as context:
```
@base.mdc
Description: Component-specific rules
Globs: src/components/**/*.tsx
```

REMEMBER: After every memory reset, I begin completely fresh. The Memory Bank and rules directory are my only links to previous work. They must be maintained with precision and clarity, as my effectiveness depends entirely on their accuracy.
```

#### 3. Initialize Memory Bank
1. Create the `memory-bank/` directory structure
2. Add the `.cursorrules` file to your project root
3. Start a new chat with Cursor and say: "Initialize your Memory Bank with the project context"
4. Cursor will create the necessary files and structure

#### 4. Usage
- **Automatic Updates**: Memory bank updates when you discover new patterns or make significant changes
- **Manual Updates**: Use "update memory bank" trigger to refresh context
- **Context Reading**: Cursor reads all memory bank files at the start of each task

## Windsurf Integration

### Available Solutions

#### Option 1: Cascade Memory Bank
The most popular and feature-rich solution for Windsurf.

**Installation:**
1. Clone the repository:
   ```bash
   git clone https://github.com/GreatScottyMac/cascade-memory-bank.git
   cd cascade-memory-bank
   npm install
   npm run build
   ```

2. Copy compiled files to your Windsurf extensions directory

3. Copy `.windsurfrules` to your project root or paste contents into Windsurf's "Set Workspace AI Rules"

4. Copy `global_rules.md` to `~/.codeium/windsurf/memories/` or paste into Windsurf's "Set Global AI Rules"

**Usage:**
- Initialize: "Follow the protocol in your .windsurfrules"
- Manual updates: "Update Memory Bank" or "UMB"
- Store context: `windsurf-memory.storeContext`
- View context: `windsurf-memory.getProjectContext`

#### Option 2: Windsurf Memory Plugin
Alternative implementation with SQLite-based storage.

**Installation:**
1. Clone the repository:
   ```bash
   git clone https://github.com/Blu3Ru1n/windsurf-memory-plugin.git
   ```

2. Follow installation instructions in the repository README

### Memory Bank Structure (Windsurf)
```
memory-bank/
├── activeContext.md    # Session state and goals
├── productContext.md   # Project scope and architecture
├── progress.md         # Work status and milestones
└── decisionLog.md      # Important decisions
```

### Git Configuration
Configure your `.gitignore` to manage memory bank visibility:

```gitignore
# Ignore everything by default
*

# Version control exceptions
!/.gitignore
!/.windsurfrules
!/memory-bank/
!/src/
!/package.json
!/README.md
!/docs/
```

**Usage:**
- When working with Windsurf: Remove the `*` line to allow visibility
- Before committing: Re-add the `*` line to maintain clean repository

## Best Practices

### For Both IDEs
1. **Regular Updates**: Keep memory bank files current with project changes
2. **Clear Documentation**: Write clear, concise descriptions
3. **Version Control**: Include memory bank files in version control
4. **Backup**: Regularly backup your memory bank files

### Memory Bank Maintenance
- **Update on Changes**: Modify memory bank when architecture changes
- **Review Regularly**: Periodically review and clean up outdated information
- **Document Decisions**: Record important architectural and design decisions
- **Track Progress**: Maintain accurate progress tracking

## Troubleshooting

### Common Issues
1. **Memory Not Loading**: Ensure memory bank files exist and are accessible
2. **Context Loss**: Check that rules are properly configured
3. **Performance Issues**: Keep memory bank files focused and concise

### Solutions
1. **Verify File Structure**: Check that all required files exist
2. **Check Rules Configuration**: Ensure rules are properly set up
3. **Restart IDE**: Sometimes a restart helps with rule loading

## Advanced Features

### Custom Memory Types
Create additional memory files for specific needs:
- API documentation
- Testing strategies
- Deployment procedures
- Integration specifications

### Memory Bank Automation
- Set up automated updates based on file changes
- Create scripts to sync memory bank across team members
- Implement memory bank validation checks

## Contributing

Contributions to memory bank implementations are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Support

For issues and questions:
- **Cursor**: Check Cursor's documentation and community forums
- **Windsurf**: Refer to the specific plugin documentation
- **General**: Open an issue in the relevant repository

## License

Memory bank implementations are typically provided under the MIT License. Check individual repository licenses for specific terms.

---

*This guide provides a comprehensive approach to adding memory banks to both Cursor and Windsurf IDEs, enabling persistent project context and enhanced AI assistance.*