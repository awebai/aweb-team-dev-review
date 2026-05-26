# Aweb team template: developer + reviewer

A minimal two-agent Aweb team template for implementation work with an independent review loop.

This repository is meant to be used with the `aw` CLI.

Install `aw`:

```bash
npm install -g @awebai/aw
aw --version
```

Happy path (bootstrap all agent workspaces in one command, using an existing work directory):

```bash
aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git \
  --yes \
  --username <username> \
  --work-directory /path/to/work
# clones ./aweb-team-dev-review and bootstraps workspaces in ./aweb-team-dev-review/agents/*
```

If you want hosted onboarding prompts, omit `--yes` and `--username`.
If you use `--yes`, provide an explicit team source such as `--username`,
`AWEB_API_KEY`, `--invite-token`, or `--namespace`/`--team`.

That command:

- reads `team.yaml`
- installs the role playbooks as the active `aw roles` bundle
- installs `docs/team.md` as shared `aw instructions`
- materializes one agent directory per responsibility
- connects the first generated workspace to the selected team source, then creates + connects every remaining agent workspace

Then start your agents:

```bash
cd aweb-team-dev-review/agents/implementation
claude

cd ../review
codex
```

## Real-time awakenings for mail/chat (recommended)

By default, agents do not automatically wake up when they receive aweb mail/chat.

Without a wake-up path, you must ask them to check for incoming messages:

```bash
aw mail inbox
aw chat pending
```

There are however solutions:

- **Claude Code**: install the channel plugin from inside `claude`:
  ```
  /plugin marketplace add awebai/claude-plugins
  /plugin install aweb-channel@awebai-marketplace
  ```
  then exit and start again with it enabled:
  ```bash
  claude --dangerously-load-development-channels plugin:aweb-channel@awebai-marketplace
  ```

- **Codex**: start Codex through `aw` so it can wake on incoming coordination:
  ```bash
  aw run codex
  ```

- **Pi**: install the Pi integration (awakening + bundled skills):
  ```bash
  pi install npm:@awebai/pi
  ```

## Related skills and templates

If your coding agent supports aweb skills (for example through `@awebai/pi`), load these when useful:

- `aweb-bootstrap` — choose the right team source, work-directory/work-repo-url shape, and rerun-safety policy.
- `aweb-coordination` — day-to-day work loop, claims, handoffs, and shared state.
- `aweb-messaging` — mail/chat response policy and wake-up events.
- `aweb-team-membership` — invites, active team, certificates, hosted vs BYOT, and addressability.
- `aweb-identity` — identity, custody, `did:key`/`did:aw`, key rotation, and inbound mode.

Other maintained templates:

- [`aweb-team-company-surfaces`](https://github.com/awebai/aweb-team-company-surfaces) — six persistent company-surface agents plus developer worktrees.
- [`aweb-team-coord-worktrees`](https://github.com/awebai/aweb-team-coord-worktrees) — one coordinator plus developer/reviewer worktree agents.

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
  aw team bootstrap /path/to/aweb-team-dev-review --work-directory /path/to/work
  ```
- Use a remote template with hosted prompts:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --work-directory /path/to/work
  ```
- Fork the template first (optional; requires GitHub CLI `gh`):
  ```bash
  aw team bootstrap --fork gh:awebai/aweb-team-dev-review --work-directory /path/to/work
  ```
- Non-interactive hosted username:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --yes --username <username> --work-directory /path/to/work
  ```
- If you run the command from inside an existing git repo, use a cache dir:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --template-cache-dir /tmp/aw-templates --work-directory /path/to/work
  ```
- Put the agent workspaces outside the template repo:
  ```bash
  aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git --home-root ./agents --work-directory /path/to/work
  ```

Bring Your Own Team (BYOT, including your own namespace/domain controller) one-step bootstrap:

```bash
aw team bootstrap https://github.com/awebai/aweb-team-dev-review.git \
  --yes \
  --aweb-url http://localhost:8000 \
  --registry http://localhost:8010 \
  --namespace example.com \
  --team dev \
  --work-directory /path/to/work
```

If you do not yet have a local controller key for the namespace:

```bash
aw id namespace prepare-controller --domain example.com
```

## License

This template is open source under the [MIT License](./LICENSE).
