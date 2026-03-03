# Full (Markdown) — 4-Agent Team

A complete ttal team with task management, research, design, and worker lifecycle. Plans and research are stored as markdown files in your project repos.

## Agents

- **manager** — task orchestrator. Routes work, manages priorities, handles daily focus.
- **researcher** — investigator. Explores codebases, reads docs, writes structured findings.
- **designer** — architect. Turns research into step-by-step implementation plans.
- **lifecycle** — worker manager. Spawns workers, triages PR reviews, handles merges.

## The pipeline

```
task add "Build feature X" +research
  → researcher investigates, writes findings to docs/research/
  → task moves to +design
  → designer writes plan to docs/plans/
  → you approve
  → lifecycle spawns a worker
  → worker executes, creates PR
  → lifecycle triages review, merges
```

## Quick start

1. Set up your workspace:
   ```bash
   ttal onboard --scaffold full-markdown
   ```
2. Edit `~/.config/ttal/config.toml`:
   - Set `chat_id` and `team_path`
3. Create 4 Telegram bots via @BotFather
4. Add tokens to `~/.config/ttal/.env`:
   ```
   MANAGER_BOT_TOKEN=...
   RESEARCHER_BOT_TOKEN=...
   DESIGNER_BOT_TOKEN=...
   LIFECYCLE_BOT_TOKEN=...
   ```
5. Run `ttal doctor` to verify
6. Start the daemon: `ttal daemon start`
