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
- `config.toml` — minimal example configuration to edit.
- `flake.nix` — `icedos.lib.mkIceDOS { configRoot = self; }`.
- `extra-modules/` — placeholder for advanced custom modules.
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
