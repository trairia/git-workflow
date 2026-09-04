# git-topic-branch

A Git topic branch workflow plugin designed for Antigravity (`agy`) and Claude Code (`claude`). It automates topic branch creation, diff review, AI-assisted commit generation, and guided branch closing following safe Git execution practices.

## Features

- **Topic Branch Automation (`/topic`)**: Effortlessly spin up feature or bugfix branches from `main` (or a specified root branch).
- **Smart Diff Review & Commit Generation (`/close`)**: Automatically reviews working tree changes, drafts a concise commit message, and requests explicit approval before staging and committing.
- **Non-Destructive / Safe Execution**: Never performs destructive or automatic merges. Prompts clear copy-paste commands for final merges back into root branches.

## Available Commands

### `/topic <branch-slug> [root]`

Creates and checks out a new branch `<branch-slug>` from `<root>` (defaults to `main` if omitted).

- **Arguments**:
  - `<branch-slug>`: The name/slug for the new topic branch.
  - `[root]` *(optional)*: The base branch to branch off from (defaults to `main`).
- **Behavior**:
  Executes `git checkout -b <branch-slug> <root>` immediately to get you straight to work.

### `/close`

Guides you through wrapping up the current topic branch safely:

1. **Diff & Status Inspection**: Inspects `git status` and `git diff` to understand all unstaged and staged changes.
2. **AI Commit Generation**: Drafts a concise, context-aware commit message.
3. **Approval-Gated Execution**: Proposes staging and committing commands:
   ```bash
   git add .
   git commit -m "<AI_GENERATED_COMMIT_MESSAGE>"
   ```
   Requires explicit user approval before execution.
4. **Merge Instructions**: After committing, provides non-destructive copy-paste instructions to merge changes back into the root branch:
   ```bash
   git checkout <root>
   git merge <branch-slug>
   ```

## Installation & Usage

### Antigravity (`agy`) CLI

Direct install via Git repository:

```bash
agy plugin install https://github.com/trairia/git-workflow.git
```

Verify or list installed plugins:

```bash
agy plugin list
```

### Claude Code (`claude`) CLI

Clone and load locally using `--plugin-dir`:

```bash
git clone https://github.com/trairia/git-workflow.git
claude --plugin-dir ./git-workflow
```

### Workspace-level Configuration

If embedded directly within a project repository, expose skills via `.agents/skills.json`:

```json
{
  "entries": [
    { "path": "skills" }
  ]
}
```

## Repository Structure

- `plugin.json`: Plugin manifest registering the plugin name.
- `skills/git-topic-branch/SKILL.md`: Core skill specification containing workflow rules and prompts.
- `.agents/skills.json`: Workspace skill discovery configuration.

