# vstack

> Built on top of [gstack](https://github.com/garrytan/gstack) by [Garry Tan](https://x.com/garrytan).

**vstack** is my personalized fork of gstack — a Claude Code skill suite that turns AI coding assistants into a virtual engineering team. Where gstack is Garry's open-source software factory, vstack is how I blend Claude (Opus) and OpenAI Codex into a unified workflow.

## What's different from gstack

**Opus+Codex Dispatch System** — The plan-eng-review skill replaces the optional "outside voice" with a mandatory dual-model execution loop:

- **Backend/infra tasks** route to Codex with `xhigh` reasoning effort, full sandbox access
- **UI/frontend tasks** route to Claude Opus subagents running in parallel
- **Convergence loop** — Opus reviews every Codex output, iterates up to 3x, escalates to the user if no agreement
- **E2E testing router** — automatically picks between browser MCP (web apps), computer-use MCP (native/desktop), or hybrid (Electron)

**Upstream tracking** — A daily GitHub Action checks for gstack updates and opens PRs so I can review and merge upstream improvements without losing my customizations.

## How it works

```
┌──────────────────────────────────────────────┐
│              PLAN-ENG-REVIEW                  │
│                                               │
│  1. Review (Architecture → Code → Tests →    │
│     Performance)                              │
│  2. Classify tasks → Backend or UI            │
│  3. Dispatch:                                 │
│     ├── Backend → Codex (xhigh reasoning)    │
│     └── UI → Opus subagents (parallel)       │
│  4. Convergence loop until Opus agrees        │
│  5. E2E test routing (browser/computer-use)   │
└──────────────────────────────────────────────┘
```

## Setup

vstack includes all gstack skills. Install it the same way:

```bash
# Clone to Documents
git clone https://github.com/viraatdas/vstack.git ~/Documents/vstack

# Symlink skills into Claude Code
cd ~/.claude/skills
for skill in ~/Documents/vstack/*/; do
  name=$(basename "$skill")
  [ -d "$skill" ] && [ -f "$skill/SKILL.md" ] && ln -sf "$skill" "$name"
done

# Install Codex CLI (required for the dispatch system)
npm install -g @openai/codex
codex login
```

## Keeping up with gstack

vstack tracks upstream gstack via the `upstream` remote:

```bash
cd ~/Documents/vstack
git fetch upstream
git merge upstream/main  # or cherry-pick specific commits
```

A GitHub Action also runs daily and opens PRs when upstream has new commits.

## Credits

All the foundational skill architecture — the preamble system, review pipeline, browse daemon, QA automation, ship workflow, and 30+ skills — comes from [gstack](https://github.com/garrytan/gstack) by Garry Tan. vstack adds a dual-model orchestration layer on top.

## License

MIT (same as gstack)
