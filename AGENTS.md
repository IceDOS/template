# AGENTS.md — IceDOS **template**

> Utilizes the **IceDOS** framework. The full bible — module structure, config flow,
> the `icedos rebuild --build` test loop, `validate.*` helpers, dep loading — lives in
> **core**: <https://github.com/IceDOS/core/blob/main/AGENTS.md> — this file covers the
> **template** repo specifically.

## Non-negotiable rules
- Build/test only via the `icedos` CLI — **never `sudo nixos-rebuild`**.
- **Never** `git commit/stash/reset/pull` — the user manages git.
- Format with `icedos nixf .` after editing any `.nix`.
- If a repo or the config root you need isn't checked out locally, **ask the user** for
  its path or permission to `git clone` it — don't guess or clone unprompted.

## Purpose
The **starter config** a new IceDOS user forks to create their **own config root** (any
name/location). Same structure as a real user config but **generic** — no personal data.
Cloning this and running the rebuild bootstraps a machine.

## Layout
- `config.toml` — minimal example configuration to edit; the global base config.
  **Optional** — a root can be defined entirely by `configs/*.toml` and/or `modules/`
  (the root is marked by `flake.nix`).
- `configs/` — extra config dirs (`icedos.system.extraConfigs`, default `["configs"]`):
  every `*.toml` here is autoloaded and strict-merged onto `config.toml`. A hidden
  `configs/.<name>.toml` loads too but is gitignored — secrets/host values that stay off
  git (replaces the old `.private.toml`). A file opts out with a top-level `enable = false`
  (default true).
- `modules/` — extra module dirs (`icedos.system.extraModules`, default `["modules"]`):
  placeholder for advanced custom Nix/IceDOS modules.
- `flake.nix` — `icedos.lib.mkIceDOS { configRoot = self; }`.
- `README.md` — install instructions (points at the core README + example user configs).
- `.editorconfig`, `.taplo.toml` — formatting for TOML/editors.
- `.state/` — generated; not hand-edited.

## Editing guidance
- Keep it **generic and minimal** — this is documentation-by-example. Don't add
  machine-specific paths, hardware UUIDs, or personal settings.
- When adding an example option, make sure it reflects a real module option and sane
  default.

## Bootstrap (from the README)
```bash
git clone https://github.com/icedos/template icedos && cd icedos
nix --extra-experimental-features "flakes nix-command pipe-operators" run path:. -- --boot
```
