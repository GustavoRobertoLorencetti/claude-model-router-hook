# claude-model-advisor

A Claude Code hook system that automatically switches your active model based on task complexity and injects sub-agent model-selection rules into every session.

## How It Works

- Every prompt is classified by task complexity using keyword and pattern matching (no API calls).
- If your active model does not match the task tier, it auto-switches `settings.json` and injects a `systemMessage` into the chat: `[model-advisor] Switched sonnet -> opus`.
- The switch is visible in the chat on every affected message. No need to re-send.
- Prefix any prompt with `~` to bypass the advisor entirely.
- Every session receives context with mandatory sub-agent model rules:
  - `haiku` - file lookups, `grep`, `glob`, `git status`, quick reads
  - `sonnet` - writing/editing code, debugging, general agents
  - `opus` - architecture, deep multi-file analysis, plan-mode agents
- All classifications are logged to `~/.claude/hooks/model-advisor.log`.

## Installation

### Step 1 — Create the hooks directory

```bash
mkdir -p ~/.claude/hooks
```

### Step 2 — Copy the hook scripts

```bash
cp hooks/session-init.sh ~/.claude/hooks/session-init.sh
cp hooks/model-advisor.sh ~/.claude/hooks/model-advisor.sh
chmod +x ~/.claude/hooks/session-init.sh ~/.claude/hooks/model-advisor.sh
```

### Step 3 — Update `~/.claude/settings.json`

Add these entries to the `hooks` object. Preserve any existing hooks.

Under `SessionStart`, add alongside any existing entries:

```json
{
  "type": "command",
  "command": "/home/yourname/.claude/hooks/session-init.sh",
  "timeout": 2
}
```

Add a new `UserPromptSubmit` section:

```json
"UserPromptSubmit": [
  {
    "matcher": "",
    "hooks": [
      {
        "type": "command",
        "command": "/home/yourname/.claude/hooks/model-advisor.sh",
        "timeout": 2
      }
    ]
  }
]
```

Use the full absolute path for `command`. Find your home path with: `echo $HOME`

### Step 4 — Restart Claude Code

Restart to activate the hooks.

## Model Tiers

| Tier | Use for |
|------|---------|
| `haiku` | Git ops, renames, formatting, file lookups, quick reads |
| `sonnet` | Feature work, debugging, writing/editing code, planning |
| `opus` | Architecture, deep multi-file analysis, complex refactors |

## Override

Prefix any prompt with `~` to skip classification and keep the current model.

## Log

Activity is logged to `~/.claude/hooks/model-advisor.log`:

```
[2026-03-07 12:00:00] model=sonnet rec=opus action=AUTOSWITCH->opus prompt="analyze the entire..."
[2026-03-07 12:01:00] model=opus rec=match action=ALLOW prompt="git commit changes"
[2026-03-07 12:02:00] OVERRIDE prompt="~ keep opus for this..."
```
