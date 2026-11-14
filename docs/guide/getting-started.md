# Getting started: workspace, mods, Play, Build and share

## What you need

- One of the games installed: SoT (`prince.bf`), WW (`prince.bf`) or T2T (`pop3.bf`).
- openjade (`openjade.exe`) to play your mod. The Studio asks for it once (Play > Set openjade.exe...).
- Blender only if you want to edit models in Blender (optional).

## 1. Open a game: the workspace

A workspace is one unpacked game. The Studio reads it and never changes it: your changes live in a mod.

- **Studio:** on the start page, **Unpack this game...** (or File > Unpack a game...) unpacks a detected install
  into `Documents/Jade Studio/<game>/`. **Open unpacked project...** (File > Open project..., Ctrl+O) opens a
  folder bfcook unpacked before. Dropping a `.bf` on the window opens the unpack wizard too.
- **CLI:** `jadestudio-cli unpack <archive.bf> <dir> [--game sot|ww|t2t]`, then `jadestudio-cli info <dir>`.

The first open builds the asset index (a few seconds; it is cached). The Studio works while it runs.

## 2. Make a mod

- **Studio:** Mod > New mod... (a name, an author and a template: Empty, Texture swap, Character look, New room
  in a level, New zone). Mod > Switch mod changes the active mod. Mod > Mod properties... edits the card (title,
  version, description, picture, changelog, "Needs other mods").
- **CLI:** `jadestudio-cli new-mod <project> <name> [--author A] [--description D]`, `use-mod <project> <name>`,
  `mod <project> --properties [--title T] [--version V] ...`.

The mod's files are in `<project>/jadestudio/mods/<mod>/` (Mod > Show the mod folder). Only what changed is there.

## 3. Find what to change

- **Assets panel:** browse by collection (Characters, Weapons, Props, Effects, Sounds, Videos, Text) or by level.
- **Go to anything** (Ctrl+T): names of objects, levels, rooms, tasks and commands.
- **Command palette** (Ctrl+Shift+P): every menu command by name.
- **Level view:** open a level, click an object; the **Inspector** shows its parts.
- **View > Simple mode / Full mode:** Simple shows game names only; Full adds keys, bins, types and raw data.
- **CLI:** `jadestudio-cli index <project> --search WORDS` and `--key HEX` (what uses it, what it uses).

## 4. Change something

Every change is **one undo step** with a row in the **Changes** panel (Revert per row). Edit > Undo / Redo works
after a restart too: the mod keeps its history (CLI: `mod <project> --history | --undo | --redo`).

When an asset is used in several places, the Studio asks once where the change applies: **Everywhere**, **This
level** or **This room**.

The quickest changes are the tasks: see [Swaps](swaps.md). The other pages of this guide cover the rest.

## 5. Check and play

- **Play > Check the mod now** runs the mod check: every changed file is read as the cook will read it; scripts
  are compiled. Problems go to the **Problems** panel, each with a **Fix** where one exists.
  CLI: `jadestudio-cli check <project> [--mod MOD]`.
- **Play** (F5, the Play bar under the menu bar): builds an overlay archive of the mod, starts openjade at the
  chosen level and checkpoint, and lists the problems of its log afterwards. "Play from the level view camera"
  starts where the level view looks. The Play bar's "..." options: check before playing, the live link, a full
  archive instead of the overlay.
  CLI: `jadestudio-cli play <project> [--level NAME] [--checkpoint NAME.gao] [--spawn X Y Z]`.
- Stop: Play > Stop the play-test (Shift+F5).

A new zone starts at its own checkpoint. SoT starts through `--map 0101_Entree --checkpoint <the zone's>`.

## 6. Build and share

- **Build > Build and share...**: fill in the mod card, run the checks, see conflicts with other mods (your other
  mods of the workspace, or `.jtmod` files you add), and build the `.jtmod`.
- **Build > Cook to .bf...** makes a whole game archive for yourself; **Build > Verify a .bf...** checks one.
- **Mod > Install into my openjade folder...** cooks the mod into an openjade folder and keeps the original
  archive as `<archive>.jadestudio-original` (`mod --uninstall DIR` puts it back).
- **CLI:** `jadestudio-cli jtmod <project> [out.jtmod]`, `cook <project> [out.bf] --verify`,
  `mod <project> --conflicts [--with OTHER.jtmod]`, `mod <project> --install DIR`.

Builds go to `<project>/jadestudio/builds/<mod>/` (Build > Show the build folder).

## Next

- [Tutorial a: replace a texture and a sound](tutorials/a-texture-and-sound.md) is the shortest way through all of this.
- [Mod lifecycle and sharing](mod-lifecycle.md): rename, merge, import a `.jtmod`, rebase, install.
