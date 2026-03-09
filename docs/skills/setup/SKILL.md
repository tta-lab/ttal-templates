---
name: setup
description: First-time ttal setup. Installs ttal, then runs ttal onboard for daemon, hooks, and config. Run this after cloning a ttal workspace template.
---

# TTAL Setup

Run setup steps automatically. Only pause when user action is required (Telegram bot creation, pasting tokens). Fix things yourself when possible.

**Principle:** Do the work. Only ask the user when you genuinely need their input (secrets, choices).

**UX Note:** Use `AskUserQuestion` for all user-facing questions.

## 1. Install ttal

Check if ttal is installed:

```bash
which ttal && echo "TTAL_OK=true" || echo "TTAL_OK=false"
```

If TTAL_OK=false, detect platform and install:
- macOS: `brew tap tta-lab/ttal && brew install ttal`
- Linux/other: `go install github.com/tta-lab/ttal-cli@latest`
  - If `go` not found: `brew install go` (macOS) or guide user to install Go

Verify: `ttal version`

## 2. Run ttal onboard

`ttal onboard` handles everything: prerequisites (tmux, taskwarrior, ffmpeg), workspace scaffold, taskwarrior UDAs, daemon install, worker hooks. Just run it:

```bash
ttal onboard
```

If onboard detects issues, it will fix them automatically (`--fix` mode). Parse its output for any errors that need user attention.

After onboard completes, verify the workspace team_path points to current directory:

```bash
WORKSPACE=$(pwd)
grep "team_path" ~/.config/ttal/config.toml
```

If team_path doesn't match current workspace, update it:

```bash
WORKSPACE=$(pwd)
sed -i '' "s|team_path = .*|team_path = \"$WORKSPACE\"|" ~/.config/ttal/config.toml
```

## 3. Telegram Bot Setup (Optional)

AskUserQuestion: "Would you like to set up Telegram integration? This lets you manage agents from your phone. (You can skip this and add it later.)"

If yes:

### 3a. Get chat_id

Tell user:
1. Open Telegram, search for `@userinfobot`
2. Send `/start` — it replies with your chat ID
3. Ask user to paste their chat ID

Update config.toml with the pasted chat_id.

### 3b. Create agent bots

Discover agents in the workspace:

```bash
for dir in */CLAUDE.md; do dirname "$dir"; done
```

For each agent, tell user:
1. Open Telegram, search for `@BotFather`
2. Send `/newbot`
3. Name: `<agent-name> (ttal)` (or whatever they want)
4. Username: `<agent-name>_ttal_bot` (must end in `bot`)
5. Copy the bot token

After user creates all bots, ask them to paste each token. Write to `~/.config/ttal/.env`:

```bash
# For each agent:
echo "AGENTNAME_BOT_TOKEN=<pasted-token>" >> ~/.config/ttal/.env
```

### 3c. Notification bot

Tell user to create one more bot for system notifications (PR status, CI results). Write token to .env.

### 3d. Verify

```bash
ttal daemon restart   # pick up new tokens
ttal doctor           # should show all green
```

## 4. Verify & Done

Run final health check:

```bash
ttal doctor
```

If all green, print success with discovered agents and next steps:

```
✓ ttal is ready!

Your agents:
  (list discovered agent directories)

Next steps:
  • Start your agents: ttal team start
  • Add a project: ttal project add myapp --path /path/to/repo
  • Create a task: ttal task add --project myapp "Build the thing"
```

If warnings/errors remain, explain each and offer to fix.
