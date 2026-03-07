# Contributing

## Dev setup

```bash
git clone https://github.com/tzachbon/claude-model-advisor.git
cd claude-model-advisor
```

No build step. The hooks are plain bash scripts.

## Making changes

- Edit scripts in `hooks/`
- Test locally by installing with `./install.sh` and running Claude Code
- Check the log at `~/.claude/hooks/model-advisor.log`

## Commit style

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add new tier keyword
fix: correct haiku pattern matching
docs: update installation steps
chore: rename internal variable
```

Keep messages short and in lowercase imperative mood.

## Pull requests

- One concern per PR
- Fill in the PR template
- Explain the "why", not just the "what"

## Reporting bugs

Use the bug report issue template. Include your shell, OS, and a snippet from `model-advisor.log`.
