# Opencode Skills

This repository contains my custom Opencode skills.

## Setup

1. Place this project inside `~/.config/opencode/skills`.
2. Keep each skill in its own folder.
3. Use `SKILL.md` as the main file for each skill.
4. Make sure Opencode reads configuration from `~/.config/opencode`.

Example structure:

```text
~/.config/opencode/
├── opencode.jsonc
├── commands/
└── skills/
    └── code-review-and-quality/
        └── SKILL.md
```

## Usage

1. Add a new skill by creating a new folder inside `skills/`.
2. Put the skill instructions in `SKILL.md`.
3. Use the skill from your Opencode workflow when needed.
4. To maintain a skill, edit its `SKILL.md` file directly.

## Simple Rules

- One skill = one folder.
- Keep the main instructions in `SKILL.md`.
- Keep each skill focused and concise.
