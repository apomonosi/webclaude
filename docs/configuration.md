# Configuration Files

Claude Code Web supports various configuration files to customize behavior and maintain project context. Understanding which files work in the web version versus the CLI version is crucial for effective usage.

## Overview: Web vs CLI Support

| Feature | Claude Code CLI | Claude Code Web | Notes |
|---------|-----------------|-----------------|-------|
| **CLAUDE.md** | ✅ Full support | ✅ Full support | Primary configuration file |
| **Custom agents** | ✅ `.claude/agents/*.md` | ❌ Not supported | CLI only - requires local filesystem |
| **Custom commands** | ✅ `.claude/commands/*.md` | ❌ Not supported | CLI only - requires local filesystem |
| **Settings** | ✅ Full `.claude/settings.json` | ⚠️ Limited (hooks only) | Partial web support |
| **MCP Servers** | ✅ Supported | ❌ Not supported | CLI only |
| **Hooks** | ✅ All hooks | ✅ SessionStart primarily | Web uses hooks for environment setup |

## CLAUDE.md - Your Project Memory File

### What is CLAUDE.md?

`CLAUDE.md` is the primary configuration file for Claude Code. It stores project-specific instructions, context, and conventions that Claude will reference throughout your coding session. **This file works in both CLI and Web versions.**

### File Locations (in order of precedence)

1. `./CLAUDE.md` - Project root (shared with team via git)
2. `./.claude/CLAUDE.md` - Alternative project location
3. `./CLAUDE.local.md` - Local/private (not version-controlled)
4. `~/.claude/CLAUDE.md` - User-level (applies to all projects)

!!! note "Web Version Limitation"
    The web version runs in a sandboxed cloud environment, so user-level files like `~/.claude/CLAUDE.md` won't be accessible. Focus on project-level `CLAUDE.md` files that are committed to your repository.

### What to Include in CLAUDE.md

```markdown
# Project Name

## Overview
Brief description of what this project does and its purpose.

## Architecture
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: PostgreSQL
- Hosting: Vercel (frontend), Heroku (backend)

## Code Style & Conventions
- Use 2-space indentation for JavaScript/TypeScript
- Use 4-space indentation for Python
- Function names: camelCase
- Component names: PascalCase
- Test files: `*.test.ts` pattern
- Always use const/let, never var

## Important Commands
```bash
# Development
npm run dev

# Testing
npm test
npm run test:watch

# Build
npm run build

# Linting
npm run lint
npm run lint:fix
```

## Project Structure
```
src/
├── components/     # React components
├── services/       # API service layer
├── utils/          # Utility functions
├── hooks/          # Custom React hooks
└── types/          # TypeScript type definitions
```

## Key Patterns
- All API calls go through the services layer
- Use custom hooks for shared logic
- Components should be functional with hooks
- Keep components under 200 lines

## Testing Guidelines
- Unit tests for utils and services
- Integration tests for API endpoints
- E2E tests for critical user flows
- Aim for 80%+ coverage

## Common Tasks
- Adding a new API endpoint: Create in services/, add types, write tests
- Adding a new component: Create in components/, add to index, write story
- Database changes: Create migration, update models, update types

## Gotchas
- The authentication token expires after 1 hour
- Database connections must be pooled
- Files over 10MB require special handling
```

### Creating CLAUDE.md

You can create CLAUDE.md manually or use the `/init` command (if available in your environment):

```bash
/init
```

This command analyzes your project and creates a starter CLAUDE.md with relevant context.

### Best Practices for CLAUDE.md

1. **Be Specific**: Instead of "follow good practices", say "use async/await for all asynchronous operations"
2. **Include Examples**: Show actual code snippets of preferred patterns
3. **Update Regularly**: Keep it current as your project evolves
4. **Version Control**: Commit CLAUDE.md so your team shares the same context
5. **Keep It Focused**: Include what Claude needs to know, not exhaustive documentation
6. **Link to Docs**: Use `@path/to/file` to import other documentation files

### Example: Importing External Files

CLAUDE.md supports importing other files:

```markdown
# Project Documentation

## Architecture
@docs/architecture.md

## API Documentation
@docs/api-reference.md

## Coding Standards
- Use TypeScript strict mode
- Follow patterns in @docs/patterns.md
```

## Settings and Hooks

### .claude/settings.json

The web version has **limited support** for `.claude/settings.json`, primarily for hooks and basic permissions.

**Example for Web:**

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": ["Skill", "Read", "Grep", "Glob"]
  },
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "setup-environment.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart Hooks for Web

SessionStart hooks are particularly useful in the web version for environment initialization:

**Example: Python Project Setup**

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'python3 -m venv venv && source venv/bin/activate && pip install -r requirements.txt'"
          }
        ]
      }
    ]
  }
}
```

**Example: Node.js Project Setup**

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "npm install"
          }
        ]
      }
    ]
  }
}
```

**Example: Detecting Web vs CLI**

```bash
#!/bin/bash
# setup-environment.sh

if [ -n "$CLAUDE_CODE_REMOTE" ]; then
  echo "Running in Claude Code Web"
  # Web-specific setup
  npm install --production
else
  echo "Running in Claude Code CLI"
  # CLI-specific setup with full access
  npm install
  npm run setup:dev
fi
```

## What Doesn't Work in Web

Due to the sandboxed cloud environment, these features are **CLI-only**:

### ❌ Custom Agents

Custom agents defined in `.claude/agents/*.md` require local filesystem access and don't work in the web version.

**CLI Example (doesn't work in web):**
```markdown
---
name: code-reviewer
description: Reviews code for quality and standards
tools: Read, Grep, Glob
model: sonnet
---

You are a code reviewer focused on code quality...
```

### ❌ Custom Slash Commands

Custom commands in `.claude/commands/*.md` are not supported in the web version.

**CLI Example (doesn't work in web):**
```markdown
---
description: Run full test suite with coverage
allowed-tools: Bash
---

Run the full test suite with coverage reporting and highlight any failures.
```

### ❌ MCP Servers

Model Context Protocol (MCP) servers for extending capabilities are CLI-only.

### ❌ User-Level Configuration

Files in `~/.claude/` (user home directory) are not accessible in the web version since it runs in an isolated container.

## Philosophy: Web vs CLI

Understanding the design philosophy helps explain feature differences:

| Aspect | CLI | Web |
|--------|-----|-----|
| **Purpose** | "Doing" | "Thinking" |
| **File Access** | Direct read/write | Copy/paste only |
| **Execution** | Full bash, tests, builds | Limited execution |
| **Git Operations** | Full (commit, push, PR) | View only |
| **Environment** | Your local machine | Sandboxed cloud container |
| **Best For** | Implementation, testing | Planning, brainstorming |

## Quick Start: Setting Up Configuration

### For a New Project (Web)

1. **Create CLAUDE.md in your project root:**
   ```bash
   touch CLAUDE.md
   ```

2. **Add project context** (see template above)

3. **Create .claude/settings.json if needed:**
   ```bash
   mkdir -p .claude
   ```

4. **Add SessionStart hook** for environment setup

5. **Commit to version control:**
   ```bash
   git add CLAUDE.md .claude/
   git commit -m "Add Claude Code configuration"
   ```

### For an Existing Project (Web)

1. Use `/init` if available to auto-generate CLAUDE.md
2. Review and customize the generated content
3. Add project-specific conventions and patterns
4. Consider SessionStart hooks for dependencies
5. Commit and share with your team

## Configuration Best Practices

### 1. Start Simple
Begin with basic project overview and build commands. Add complexity as needed.

### 2. Focus on Patterns, Not Documentation
CLAUDE.md is for coding patterns and conventions, not comprehensive documentation.

### 3. Include Command Examples
Show actual commands Claude should use:
```markdown
## Testing
Always run tests before committing:
```bash
npm test -- --coverage
```
```

### 4. Specify Constraints
Be explicit about limitations:
```markdown
## Constraints
- Never modify files in `vendor/` directory
- All database queries must use prepared statements
- API responses must include proper error codes
```

### 5. Update with Lessons Learned
When Claude makes mistakes or you provide corrections, update CLAUDE.md:
```markdown
## Common Mistakes to Avoid
- Don't use `any` type in TypeScript - always define proper types
- Don't import from `src/` - use relative imports
- Don't commit `.env` files - use `.env.example` instead
```

### 6. Web-Specific Considerations
Since the web version can't execute code:
- Include expected test output examples
- Document how to verify changes manually
- Provide copy-paste-ready commands
- Include links to relevant documentation

## Troubleshooting

### CLAUDE.md Not Being Respected

**Check file location:**
```bash
ls -la CLAUDE.md
ls -la .claude/CLAUDE.md
```

**Verify format:**
- Must be valid Markdown
- No syntax errors in code blocks
- Proper heading structure

**Re-initialize session:**
Sometimes starting a new session helps pick up changes.

### SessionStart Hooks Not Running

**Check .claude/settings.json syntax:**
```bash
cat .claude/settings.json | python -m json.tool
```

**Verify hook permissions:**
Ensure the hook script is executable (for CLI) or commands are safe for web.

**Check environment variable:**
```bash
echo $CLAUDE_CODE_REMOTE
```
If set, you're in the web version.

## Advanced Configuration

### Multiple Environment Support

**CLAUDE.md:**
```markdown
## Environment Detection

The project supports multiple environments:
- Development: `npm run dev`
- Staging: `npm run start:staging`
- Production: `npm run start:prod`

Always check which environment you're targeting before making changes.
```

### Project-Specific Instructions

```markdown
## Special Instructions for Claude

When working on this project:
1. Always check the ADR (Architecture Decision Records) in `docs/adr/` before making architectural changes
2. Run the linter before suggesting code: `npm run lint`
3. Reference the component library in `src/components/README.md`
4. Follow the Git workflow in `CONTRIBUTING.md`
5. For database changes, create a migration and update `docs/schema.md`
```

### Testing Guidelines

```markdown
## Test Requirements

### Unit Tests
- Required for all utility functions
- Required for all service layer functions
- Use Jest + TypeScript
- Location: `__tests__/unit/`

### Integration Tests
- Required for all API endpoints
- Use Supertest
- Location: `__tests__/integration/`

### Running Tests
```bash
# All tests
npm test

# Watch mode (useful during development)
npm run test:watch

# Coverage report
npm run test:coverage

# Specific test file
npm test -- src/services/user.test.ts
```

### Test Expectations
- Minimum 80% coverage
- All edge cases covered
- Clear test descriptions
- Proper mocking of external dependencies
```

## Summary

- ✅ **Use CLAUDE.md** in both CLI and Web - it's your most important configuration file
- ✅ **SessionStart hooks** work in Web for environment setup
- ❌ **Custom agents and commands** are CLI-only
- ❌ **User-level configs** (~/.claude/) don't work in Web's sandboxed environment
- 💡 **Web is for thinking, CLI is for doing** - configure accordingly

## See Also

- [Getting Started](getting-started.md) - Quick start guide
- [Best Practices](best-practices.md) - Coding guidelines
- [Advanced Usage](advanced-usage.md) - Power user techniques
- [Resources](resources.md) - External links and documentation
