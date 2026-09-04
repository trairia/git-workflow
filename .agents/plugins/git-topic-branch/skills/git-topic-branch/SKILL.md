---
name: git-topic-branch
description: >-
  Activate this skill when the user uses the `/topic` or `/close` commands to manage git topic branches.
---

# Git Topic Branch Operations

This skill provides a simple workflow for creating topic branches and committing/closing them.

## Commands

### `/topic branch-slug [root]`

When the user enters the `/topic` command (e.g., `/topic feature-a` or `/topic feature-a develop`):
1. Parse the `branch-slug` and the optional `root` branch name.
2. If `root` is omitted, default to `main`.
3. Create and switch to the new topic branch from the specified root branch by executing:
   `git checkout -b <branch-slug> <root>`
4. **IMPORTANT**: The user has explicitly allowed automatic execution of `git checkout` and `git switch` commands. You MUST run this command automatically without asking for further confirmation.

### `/close`

When the user enters the `/close` command:
1. Check the current git status and `git diff` to understand the changed files.
2. Generate an appropriate commit message automatically based on the changes.
3. Propose the following commands to stage and commit the changes. Because of the global Git rules, you should present the AI-generated commit message to the user and ask for their approval to execute the commands:
   ```bash
   git add .
   git commit -m "<AI_GENERATED_COMMIT_MESSAGE>"
   ```
4. After the commit is successfully executed (or if the user committed manually), display the commands to merge the topic branch back into its root branch (e.g., `main`), but **DO NOT** execute them:
   ```bash
   git checkout <root>
   git merge <branch-slug>
   ```
   *(Ensure you replace `<root>` and `<branch-slug>` with the actual branch names).*
