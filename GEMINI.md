# Gemini Configuration

This file configures the tools that Gemini Code can use in this project.

## Allowed Tools

Gemini Code is pre-allowed to use the following tools for ticket synchronization:

- **Bash** - Execute shell commands for running sync scripts and GitHub CLI operations
- **Read** - Read configuration files (repos.txt, platforms.yml, README.md)
- **Edit** - Update configuration files when needed
- **Write** - Create temporary files or update existing configuration
- **Grep** - Search for patterns in files and logs
- **Glob** - Find files matching patterns
- **WebFetch** - Fetch content from external ticket platforms
- **WebSearch** - Search for relevant tickets and issues
- **LS** - List directory contents and verify file structure
- **TodoRead/TodoWrite** - Track synchronization tasks and progress
- **Task** - Launch agents for complex multi-platform searches

## Project Context

This project automatically syncs external platform tickets to GitHub issues using:
- GitHub CLI for issue management
- Platform-specific APIs for ticket retrieval
- Intelligent deduplication and formatting
- Scheduled daily execution via GitHub Actions

## Performance Optimization

**CRITICAL**: This project prioritizes speed and efficiency:

1. **Parallel Execution**: ALWAYS use parallel tool calls when possible
   - Batch all gh CLI commands
   - Run platform searches simultaneously
   - Create multiple issues in parallel

2. **Smart Searching**:
   - Use Task tool for complex multi-platform searches
   - Limit results to prevent context overflow
   - Focus on recent tickets (last 6 months)

3. **Duplicate Prevention**:
   - Search for existing external-sync labels first
   - Check for exact URL matches in issue bodies
   - Skip creating issues when in doubt

4. **Batching Strategy**:
   - Process multiple repos simultaneously when possible
   - Group similar operations together
   - Minimize sequential operations

## Common Commands

When working with this project, commonly used commands include:
- `gh issue list --state all --search "external-sync"` - Find existing synced issues
- `gh issue create --title "..." --body "..." --label "external-sync,platform"` - Create synced issues
- `gh issue list --state open --limit 20` - Check recent issues
