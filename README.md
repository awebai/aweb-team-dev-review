# Aweb team template: developer + reviewer

A minimal two-agent Aweb team template for implementation work with an independent review loop.

This repository is meant to be used with:

```bash
aw team bootstrap ./aweb-team-dev-review --work-repo ~/prj/my-project
```

The bootstrap command reads `team.yaml`, installs the role playbooks as the active `aw roles` bundle, installs `docs/team.md` as shared team instructions, creates one home directory per responsibility under `agents/`, and prints the `aw init` commands needed to connect each generated workspace.

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

## Current bootstrap status

This template works with the first `aw team bootstrap` implementation. That implementation currently expects a local template directory. GitHub clone/fork support, locally controlled team selection/creation, and automatic per-agent `aw init` execution are planned follow-up slices.
