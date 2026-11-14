# Swaps: the quick tasks

The tasks are one-page wizards for the common changes. Open them from the **Tasks** panel, the start page, the
**Mod > Change the game** menu, or Ctrl+T (type the task's name). Each ends in one undo step with a Changes row.

| Task (Studio) | What it does | CLI |
|---|---|---|
| Swap a texture | Your picture (PNG, TGA, BMP, JPEG, DDS; any size) in place of a game texture | `swap <project> texture --key KEY --file PIC` |
| Swap a model | A `.glb` in place of an object's model (a sword, a prop) | `swap <project> model --key KEY --file F.glb` |
| Change a character's look | A new skin: its textures, or (Full mode) a `.glb` rigged to its own skeleton | `swap <project> texture ...` on its textures |
| Swap an animation | A `.glb` animation on the same bones | `swap <project> animation --key KEY --file F.glb` |
| Tweak a character's stats | Health, speed, damage... values of a character or enemy | `variables <project> --room R --objects K,... --set NAME=VALUE` |
| Use a texture pack | A folder of pictures matched to the game's textures by name | `texture-pack <project> --import DIR` (`--export DIR` writes one) |
| Export textures | A character's, a level's or the whole game's textures, to paint | `swap ... --export FILE`, `texture-pack --export DIR` |
| Replace a sound | A WAV, OGG or FLAC file in place of a game sound | `simple <project> sound BANK EVENT FILE` |
| Replace a video | Your video (or a gallery still) in place of a game video | `simple <project> video TARGET FILE` |
| Change a text | A menu text or a tutorial hint, in any language | `simple <project> text KEY TEXT [--tutorial]` |
| Change a level's mood | Every light of a level tinted, fogs moved toward a colour (7 presets or your colours) | `level-wide <project> --level L --mood PRESET` |
| Swap a level's enemies | Every enemy of one family a level spawns becomes another family | `level-wide <project> --level L --roster FROM:TO` |
| Enemy difficulty | Scale the enemies' health and damage | `level-wide <project> --difficulty --health X --damage Y` |
| Fonts, menus and saves | See [UI and progression](ui-progression.md) | `ui ...`, `menus ...` |
| Build and share | See [Getting started](getting-started.md) | `jtmod`, `mod --conflicts` |

## Notes

- A swap is converted the way the cook will import it and checked before it is written.
- `--export` writes the asset as the mod has it (a texture as `.png` or its own format, a model as `.glb`).
  "Edit in Blender" is `--export`, your edit, then `--file` (or the Blender panel, see
  [Textures, models and characters](textures-models-characters.md)).
- `--room R` picks the view of one room when an asset is in several; `--match-size` resizes a picture to the
  texture's size. There is **no size limit**: openjade draws any size; DXT sides are rounded up to multiples of 4.
- Enemy difficulty works on all three games. On T2T damage goes up in whole steps (x1 to x4), and the sand guards,
  dark warriors, guards, sand gate guards, thralls, archers and the Brute keep their numbers (their script sets
  them); the plan names each one left.
