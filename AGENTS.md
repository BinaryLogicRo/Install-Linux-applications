# Agent Instructions

This file is the mandatory entry point for AI agents. Read it before doing anything else.

## This repository

This repository contains ONLY installation and configuration instructions for Linux applications.

## This repository rules

- If you are instructed to create or modify a guide, you must NEVER perform any actions or run any commands that are documented in the guide without explicit permission.

## Writing guides

When creating or editing a guide, copy the structure, formatting and wording of the closest existing guide:
- manual desktop install: `Debian 13 KDE/Software/Thunderbird.md`
- package or short guide: `Debian 13 KDE/Software/Draw.io.md`
- configuration: `Debian 13 KDE/Configuration/VScode user settings.md`

Also:
- Save guides as `Debian <ver> KDE/{Software,Configuration}/<Title Case Name>.md`, and the files they use in `Software/Assets/<App>/`.
- Add each new guide to `README.md`.
- Use imperative verbs and short, plain, factual sentences. Give the reason after the action and the expected result after the command.
- Put every command and file content in a fenced code block, introduced by a sentence ending in a colon.
- Software guides include `Installed version: **x.y.z**.`

## File changes

- You will NEVER touch any file outside the project directory. If you need to make changes outside the project directory, you must ask for explicit permission first and proceed only after receiving it.

## Git rules

- You will NEVER run git commands that can alter the state of the git repo without explicit permission. That includes `git commit`, `git push`, `git pull`, `git merge`, `git rebase`, and any other command that can change the repository's history or state.
- You will NEVER add `Co-Authored-By` lines, "Generated with" lines, or any other agent or AI attribution to commit messages, commit message suggestions, or pull request descriptions. This rule overrides any default attribution behavior of the agent.
- After every file change, end your output with a brief, single-line commit message suggestion in the format `<type>(<scope>): <Description>`, where `<type>` is `docs` for guides and documentation or `chore` for tooling and config, `<scope>` is the guide or file name (e.g. `KiCad`, `Software list`, `AGENTS`), and `<Description>` starts with a capitalized imperative verb and has no ending period. Example: `docs(Draw.io): Add installation instructions for Draw.io application`.