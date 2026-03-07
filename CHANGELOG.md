# Changelog

All notable changes to this project will be documented here.

## [1.0.0] - 2026-03-07

### Added
- `hooks/model-router-hook.sh` — `UserPromptSubmit` hook that classifies task complexity and auto-switches the active model in `settings.json`
- `hooks/session-init.sh` — `SessionStart` hook that injects sub-agent model-selection rules into every session
- `prompt.md` — classification prompt used by the advisor
- `install.sh` — one-command installer
- Community files: `CONTRIBUTING.md`, `SECURITY.md`, issue templates, PR template
