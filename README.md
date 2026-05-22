# Aweb team template: developer + reviewer

A minimal two-agent Aweb team template for implementation work with an independent review loop.

This repository is meant to be used with the `aw` CLI.

Install `aw`:

```bash
npm install -g @awebai/aw
aw --version
```

Happy path (bootstrap all agent workspaces in one command):

```bash
mkdir my-team && cd my-team
aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes
# clones ./aweb-team-dev-review and bootstraps workspaces in ./aweb-team-dev-review/agents/*
```

That command:

- reads `team.yaml`
- installs the role playbooks as the active `aw roles` bundle
- installs `docs/team.md` as shared `aw instructions`
- materializes one agent directory per responsibility
- (hosted default) prompts for a username, then creates + connects every agent workspace

Then start your agents (choose one runtime):

Claude Code:

```bash
cd aweb-team-dev-review/agents/implementation
claude

cd ../review
claude
```

Codex:

```bash
cd aweb-team-dev-review/agents/implementation
codex

cd ../review
codex
```

## Real-time awakenings for mail/chat (recommended)

By default, agents do not automatically wake up when they receive aweb mail/chat. Without a wake-up path, you must manually check:

```bash
aw mail inbox
aw chat pending
```

Pick one:

- **Claude Code**: install the channel plugin, then start Claude Code with it enabled:
  ```
  /plugin marketplace add awebai/claude-plugins
  /plugin install aweb-channel@awebai-marketplace
  ```
  ```bash
  claude --dangerously-load-development-channels plugin:aweb-channel@awebai-marketplace
  ```

- **Codex**: start Codex through aweb so it can wake on incoming coordination:
  ```bash
  aw run codex
  ```

- **Pi**: install the Pi integration (awakening + bundled skills):
  ```bash
  pi install npm:@awebai/pi
  ```

## Structure

```text
team.yaml                  # maps responsibility dirs to aw role names and default names

docs/team.md               # shared team instructions installed with aw instructions set

roles/developer.md         # operational role guidance installed as role_name=developer
roles/reviewer.md          # operational role guidance installed as role_name=reviewer

agents/implementation/     # responsibility-specific workspace context for the builder
agents/review/             # responsibility-specific workspace context for the reviewer
```

Agent directory names describe responsibilities, not fixed agent identities. `team.yaml` provides suggested default names and aliases; the user can change them during bootstrap.

## Template contract

- `roles/` contains reusable operational playbooks. These become the team roles bundle used by `aw roles show`.
- `agents/<responsibility>/AGENTS.md` describes what that workspace is responsible for in this team.
- `CLAUDE.md` is a compatibility symlink to `AGENTS.md`.
- `docs/team.md` contains shared team-level operating instructions.
- `team.yaml` connects each responsibility to an `aw` role name and default identity values.

## Included team

| Responsibility   | Default name | Default alias | Role name   |
|------------------|-------------:|--------------:|-------------|
| `implementation` |    `builder` |         `dev` | `developer` |
| `review`         |   `reviewer` |      `review` | `reviewer`  |

## Options

- Use a local checkout of this template:
  ```bash
  aw team bootstrap /path/to/aweb-team-dev-review --yes
  ```
- Use a remote template:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes
  ```
- Fork the template first (optional; requires GitHub CLI `gh`):
  ```bash
  aw team bootstrap --fork gh:awebai/aweb-team-dev-review --yes
  ```
- Non-interactive username:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes --username <username>
  ```
- If you run the command from inside an existing git repo, use a cache dir:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes --template-cache-dir /tmp/aw-templates
  ```
- Put the agent workspaces outside the template repo:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes --home-root ./agents
  ```

Bring Your Own Domain (BYOD, self-hosted / local controller) one-step bootstrap:

```bash
aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git \
  --yes \
  --aweb-url http://localhost:8000 \
  --registry http://localhost:8010 \
  --namespace example.com \
  --team dev
```

If you do not yet have a local controller key for the namespace:

```bash
aw id create --domain example.com --name controller
```
