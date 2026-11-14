# Mod lifecycle and sharing

## Manage mods (Mod menu)

| Studio | CLI |
|---|---|
| Mod > Rename, copy or delete mods... | `mod <project> --rename NAME --to NEW`, `--duplicate NAME --to NEW`, `--delete NAME` (to `jadestudio/trash/`) |
| Mod > Merge another mod into this one... | `mod <project> [--mod INTO] --merge FROM [--theirs]` (one undo step; a file both change keeps yours unless `--theirs`; conflicts listed) |
| Mod > Import a .jtmod as a new mod... | `mod <project> --import FILE.jtmod --name NAME` |
| Mod > Check the mod against the game (rebase)... | `mod <project> --rebase [--check]` (files that no longer apply move to `<mod>/rebase-left/`) |
| Mod > Check mods together... | `mod <project> --overlaps [--mods A,B]` (files two mods change, rooms both add to, exits into one room) |
| Mod > Install into my openjade folder... | `mod <project> --install DIR`, `--uninstall DIR` |
| Mod > Mod properties... (changelog, "Needs other mods") | `mod <project> --properties [--changelog TEXT] [--depends A,B]` |
| Changes panel: Revert a row | `revert <project> <file>`, `mod --additions`, `mod --revert-addition N` |
| Edit > Undo / Redo (also after a restart) | `mod <project> --history`, `--undo`, `--redo` |

When the workspace opens on another base archive (another install, a patched game), the Studio says so: run the
rebase check.

A removed zone can leave files the cook ignores: `mod <project> --clean-leftovers`.

## Sharing

**Build > Build and share...**: the card, the checks, the conflict check (with your other mods and with `.jtmod`
files you add), then the `.jtmod`. CLI: `jtmod <project> [out.jtmod]`, `mod --conflicts [--with X.jtmod] [--mods A,B]`.

**Build > Check .jtmod against the cook...** applies the `.jtmod` onto the base archive and compares it with a cook
(`check-jtmod <project> <mod.jtmod> <reference.bf>`).

## Several mods

A workspace holds many mods; only the active one is edited. Each mod folder holds only what changed: it is small and
fits in git.
