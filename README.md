# Aweb team template: developer + reviewer

A minimal two-agent Aweb team template for implementation work with an independent review loop.

This repository is meant to be used with the `aw` CLI.

Install `aw`:

```bash
npm install -g @awebai/aw
aw --version
```

Happy path (fork this template and bootstrap all agent workspaces in one command):

```bash
mkdir my-team && cd my-team
aw team bootstrap --fork gh:awebai/aweb-team-dev-review --yes --home-root ./agents
```

That command:

- reads `team.yaml`
- installs the role playbooks as the active `aw roles` bundle
- installs `docs/team.md` as shared `aw instructions`
- materializes one agent directory per responsibility
- (hosted default) prompts for a username, then creates + connects every agent workspace

Then run your agents:

```bash
cd agents/implementation
aw run codex

cd ../review
aw run codex
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

| Responsibility | Default name | Default alias | Role name |
|---|---:|---:|---|
| `implementation` | `builder` | `dev` | `developer` |
| `review` | `reviewer` | `review` | `reviewer` |

## Options

- Use a local checkout of this template:
  ```bash
  aw team bootstrap /path/to/aweb-team-dev-review --yes --home-root ./agents
  ```
- Use a remote template without forking:
  ```bash
  aw team bootstrap gh:awebai/aweb-team-dev-review --yes --home-root ./agents
  ```
- Non-interactive username:
  ```bash
  aw team bootstrap gh:awebai/aweb-team-dev-review --yes --home-root ./agents --username <username>
  ```
- If you run the command from inside an existing git repo, use a cache dir:
  ```bash
  aw team bootstrap gh:awebai/aweb-team-dev-review --yes --template-cache-dir /tmp/aw-templates --home-root ./agents
  ```

BYOD (self-hosted / local controller) one-step bootstrap:

```bash
aw team bootstrap gh:awebai/aweb-team-dev-review \
  --yes \
  --home-root ./agents \
  --aweb-url http://localhost:8000 \
  --registry http://localhost:8010 \
  --namespace example.com \
  --team dev
```

If you do not yet have a local controller key for the namespace:

```bash
aw id create --domain example.com --name controller
```
