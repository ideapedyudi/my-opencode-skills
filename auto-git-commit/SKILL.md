---

name: auto-git-commit
description: Automatically create Git commits from current repository changes using a safe workflow and English Conventional Commit messages.
---------------------------------------------------------------------------------------------------------------------------------------------

# Auto Git Commit Skill

Use this skill when the user asks to create a Git commit, run git add, run git commit, or generate a commit message based on repository changes.

## Goal

Perform a safe Git commit workflow.

The assistant must:

1. Check the Git working tree status.
2. Inspect current unstaged and staged changes.
3. Detect sensitive files before staging.
4. Stage safe changes.
5. Generate an English Conventional Commit message based on the actual diff.
6. Run git commit.
7. Show the final commit result.

## Required Workflow

### 1. Check Repository Status

Run this command:

```
git status --short
```

If there are no changes, do not create a commit.

Tell the user that the working tree is clean.

### 2. Inspect Current Changes

Run these commands:

```
git diff --stat
git diff
```

If staged changes exist, also run:

```
git diff --cached --stat
git diff --cached
```

The commit message must be based on the actual changes found in the diff.

### 3. Detect Sensitive Files

Before running git add, check the changed file list.

Do not automatically stage or commit files that look sensitive.

Sensitive file patterns include:

* .env
* .env.*
* *.pem
* *.key
* id_rsa
* id_ed25519
* credentials
* auth.json
* token
* secret
* *.sqlite
* *.db

If sensitive files are detected:

1. Stop the workflow.
2. Tell the user which suspicious files were found.
3. Ask whether the files should be excluded or intentionally committed.

Do not continue automatically.

### 4. Stage Changes

If no sensitive files are detected, stage all changes:

```
git add -A
```

### 5. Generate Commit Message

Commit messages must always be written in English.

Use Conventional Commit format:

```
type(scope): summary
```

Allowed commit types:

* feat
* fix
* refactor
* docs
* style
* test
* chore
* build
* ci
* perf

Use the type based on the actual changes:

* Use feat for new features.
* Use fix for bug fixes.
* Use refactor for code restructuring without behavior changes.
* Use docs for documentation changes.
* Use style for formatting or style-only changes.
* Use test for tests.
* Use chore for configuration, dependency, tooling, or maintenance.
* Use build for build system changes.
* Use ci for CI/CD workflow changes.
* Use perf for performance improvements.

Commit message rules:

* Always use English.
* Use imperative mood.
* Keep the title concise.
* Do not use Indonesian words.
* Use a scope only when it is clear.
* Use a body only when the change needs extra explanation.
* The message must reflect the real Git diff, not assumptions.

Examples:

```
feat(auth): add login validation
fix(api): handle missing mongodb uri
chore(opencode): add auto git commit skill
refactor(ui): simplify table component state
docs(readme): update installation guide
```

### 6. Commit Changes

Run:

```
git commit -m "<English commit message>"
```

For complex changes, use a body:

```
git commit -m "<English title>" -m "<English body>"
```

### 7. Show Final Result

After committing, run:

```
git log -1 --oneline
git status --short
```

Then summarize the result in Indonesian.

The final response must include:

* Commit hash
* Commit message
* Summary of committed changes
* Current working tree status

## Safety Rules

The assistant must not:

* Run git push unless the user explicitly asks.
* Use Indonesian in commit messages.
* Stage or commit sensitive files automatically.
* Create an empty commit unless explicitly requested.
* Amend commits unless explicitly requested.
* Reset, checkout, restore, clean, rebase, merge, or force push.
* Use --no-verify unless explicitly requested.

If commit fails because of hooks, lint, tests, or conflicts:

1. Explain the error.
2. Stop the workflow.
3. Do not bypass the failure automatically.

## Response Language

Respond to the user in Indonesian.

However, all Git commit messages must be written in English.
