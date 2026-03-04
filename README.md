# ttal-templates

Starter templates for [ttal](https://github.com/tta-lab/ttal-cli) — pick a scaffold that matches how you work.

## Scaffolds

| Scaffold | Agents | Best for |
|----------|--------|----------|
| [basic](./basic/) | 2 (manager, designer) | Getting started, testing A2A communication |
| [full-markdown](./full-markdown/) | 4 (manager, designer, researcher, debugger) | Full team, plans stored as markdown in your repo |
| [full-flicknote](./full-flicknote/) | 4 (compass, ink, owl, falcon) | Full team with agent personalities, plans stored in FlickNote |

## Usage

### With ttal onboard (recommended)

```bash
ttal onboard --scaffold basic
ttal onboard --scaffold full-markdown
ttal onboard --scaffold full-flicknote
```

This clones the repo, copies your chosen scaffold into the workspace, and runs initial setup.

### Manual setup

```bash
git clone https://github.com/tta-lab/ttal-templates.git
cp -r ttal-templates/basic/ ~/my-workspace/
cd ~/my-workspace

# Copy config template
cp config.toml ~/.config/ttal/config.toml
# Edit: set chat_id and team_path

# Add bot tokens
echo 'MANAGER_BOT_TOKEN=your-token-here' >> ~/.config/ttal/.env

# Verify
ttal doctor
```

## What's in each scaffold

Every scaffold includes:

- **Agent directories** with `CLAUDE.md` identity files (required for ttal to discover agents)
- **`config.toml`** — team configuration template (copy to `~/.config/ttal/config.toml`)
- **`README.md`** — scaffold-specific getting started guide

## Requirements

- [ttal CLI](https://github.com/tta-lab/ttal-cli) installed
- tmux, taskwarrior, git
- Telegram bot tokens (one per agent) — create via [@BotFather](https://t.me/BotFather)
