# Aweb team template: dev + review

This is a minimal two-agent team template for `aw team bootstrap`.

- `roles/` contains role playbooks that bootstrap installs as an aw team roles bundle.
- `agents/` contains responsibility-scoped agent homes. Directory names are responsibilities, not fixed agent names.
- `team.yaml` maps each responsibility to an aw `role_name` and suggested default identity name/alias.
- `docs/team.md` is installed as shared team instructions.

Example:

```bash
aw team bootstrap ./aweb-team-dev-review --work-repo ~/prj/my-project --yes
```

After bootstrap, run the printed `aw init` commands in each generated agent home to connect the workspaces.
