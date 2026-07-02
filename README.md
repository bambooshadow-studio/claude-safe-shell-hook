# Safe Shell Hook

A **pre-tool-use** hook for Claude Code that blocks destructive bash commands before they are executed.

## Features

- 馃洝锔?Blocks dangerous commands: `rm -rf`, `DROP TABLE`, `git push --force`, `TRUNCATE`, `DELETE FROM` (without WHERE), and more
- 馃摑 Logs every blocked attempt to `~/.claude/hooks/blocked.log` with timestamp, command, and project path
- 鉁?Passes through normal commands without interference
- 馃Ъ Zero dependencies 鈥?pure Python 3

## Installation

```bash
mkdir -p ~/.claude/hooks
curl -o ~/.claude/hooks/safe-shell-hook.py https://raw.githubusercontent.com/bambooshadow-studio/claude-safe-shell-hook/main/safe-shell-hook.py
chmod +x ~/.claude/hooks/safe-shell-hook.py
```

Or copy manually:

```bash
mkdir -p ~/.claude/hooks/
cp safe-shell-hook.py ~/.claude/hooks/
chmod +x ~/.claude/hooks/safe-shell-hook.py
```

## How it works

Claude Code calls pre-tool-use hooks before executing each tool. This hook intercepts bash commands and checks them against a pattern list:

### Blocked patterns

| Category | Patterns |
|---|---|
| **Filesystem** | `rm -rf`, `rm -r /`, `rm -rf /` |
| **Database** | `DROP TABLE`, `TRUNCATE`, `DELETE FROM` without WHERE |
| **Git** | `git push --force`, `git push -f` |
| **Disk** | `mkfs`, `dd if=... of=...`, `format` |
| **Permissions** | `chmod -R 777`, `chown -R` |
| **Remote exec** | `wget ... | bash`, `curl ... | bash` |

### Always-allowed commands

`echo`, `cat`, `ls`, `grep`, `find`, `mkdir`, `touch`, `cp`, `mv`, `cd`, `pwd`, `head`, `tail`, and others.

## Log output

Blocked commands are logged to `~/.claude/hooks/blocked.log`:

```
[2026-07-02T08:00:00+00:00] BLOCKED: Destructive recursive delete (rm -rf)
  Command: rm -rf /home/user/project/node_modules
  Project: /home/user/project
```

## Uninstall

```bash
rm ~/.claude/hooks/safe-shell-hook.py
```

## License

MIT
