# UI and progression: fonts, menus, saves

**Tasks > Fonts, menus and saves** has the tabs below. CLI: `ui <project> ...` (the archive's data) and
`menus <project> ...` (openjade's own menus).

## Fonts

Every font opens in the **font editor**: its glyph sheet (the atlas and the rectangles). Change a glyph's rectangle or
spacing, or **Redraw from a TrueType font** (the font's own characters packed into its atlas; too big is refused
with the largest height that fits). SoT refuses spacing (it drops such a font).

CLI: `ui <project> --fonts | --font KEY [--glyph C --rect ... --spacing ...] [--from-ttf F --size PX] [--atlas OUT]`.

## Menu and HUD pictures

The textures openjade's pages and the in-game interface draw, replaced from a PNG fitted to each view.
CLI: `ui <project> --pictures | --picture KEY --with PNG`.

## Save captions and Special Load names

- Save captions are menu text (SaveNames.Checkpoint.<n>: SoT 0-42, WW 0-53, T2T 0-47; WW Chapter.<n>).
  CLI: `ui --captions [--set N=TEXT | --revert N]`.
- Special Load names renamed or hidden as menu text "SpecialLoad.<label>" (`ui --special-load --name L=NEW | --hide L
  | --restore L`). This needs a small openjade change (compile-checked, not deployed).

## openjade menus (mod data for openjade)

The **openjade menus** tab writes `Mods/<game>/<mod>.json`, which openjade reads beside the game:

- a TrueType font for the menu, tutorial or subtitle role (when the player left that role at the game's font);
- loading-screen pictures (in turn, or one per level);
- your own Special Load entries (a label and its world list);
- buttons on the main menu or Extras (start an entry, open a page, open a web link).

CLI: `menus <project> [--mod MOD] --list | --font ROLE FILE | --loading PNG [--world KEY] | --special-load L --world
KEY | --menu-entry PAGE ACTION TARGET --label T`. Play stages it; the `.jtmod` packs it. The Changes row's Revert takes
it all out. This is an engine change for review: compiled and unit-tested in a separate openjade folder, not deployed.

## Not mod data

The task's **Not mod data** tab (`ui --not-data`) lists what lives outside the archive and why: SoT POPData.bf and T2T
POP3_54.MFT fonts, openjade's TrueType settings, menu pages (openjade C++ pages), loading screens without pictures,
HUD layout (GameInterface scripts: use the script editor), unlocks and story flags (see [Live link](live-link.md)).
