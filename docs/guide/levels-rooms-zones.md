# Levels, rooms and zones

## The level view

Open a level from the Assets panel's Levels, the Play bar's level picker or Ctrl+T. The level view draws the rooms
as the engine loads them. Click to select; the gizmo moves, turns and scales (with snapping, several objects at
once); Ctrl+D duplicates; Delete deletes (it warns when something still names the object). Overlays: navigation,
ledges and wall runs, the Traversal check, portals, trigger boxes. The **Outliner** lists rooms and objects; the
technical worlds are greyed out.

The level view's **Room** menu: Enemy navigation..., Light the room..., Game material for the selection...,
Alternate cameras. The **Level setup** window (Mod > Change the game > Level setup..., or the level view's toolbar)
has tabs: Worlds this level loads, Sound banks, Check level, Player start, Order and Special Load, Rooms, Story
variants, Position in the world.

CLI: `object <project> --move KEY[,KEY] --to ROOM@X,Y,Z [--yaw D]`, `copy <project> --from ROOM --objects KEY,...
--into ROOM[@X,Y,Z]`, `level <project> --level L --worlds | --check [--fix] | --loadout | --variants | --order`.

## New zone: a level of your own

**Mod > Create new > New zone...** (or the Tasks panel's Levels card). Give it a name and a template:

| Template | What you get | CLI |
|---|---|---|
| Empty room | one room with a retail floor and a checkpoint | `zone <project> --mod MOD --template empty --name NAME` |
| Copy of a level | every room of a game level under new keys | `--template copy --from LEVEL` |
| Blockout | your glTF geometry, a checkpoint on its highest walkable top near the origin, or at a node named `spawn` | `--template blockout --file F.glb` |

**Plan the zone** shows what it will make; **Play it** starts it. Tick **Connect it to a game level** to make the
exit at the same time (see below). A zone gets a navigation grid, the game's technical worlds and every parkour
move kit; T2T zones also get a level-defaults row.

A new zone stands at the **zone origin** 1500, -1500, 0 (clear of what the technical worlds keep near the world's
origin). Other commands take **world** coordinates; "From the zone's origin" (GUI) and `--local` (CLI) take points
relative to the zone's origin. `zone --move NAME --by DX,DY,DZ | --to X,Y,Z` moves a whole zone.

`jadestudio-cli import-geometry --sample OUT.glb --spawn` writes a small Blockout file to try.

## Import geometry

**Import geometry** (task, level view button; `import-geometry <project> --mod MOD --room ROOM --file F.glb`): one
object per glTF node (or per material), with triangle-mesh collision from its own mesh, a `<name>_col` node or
`extras.collision`. Grab edges, ledges and wall runs are detected; override them with `extras.surface` / `--surface
NODE=KIND` (auto floor ledge nograb wall wallrun column pole beam). **Apply game material** puts a game texture on
imported geometry (`--material NAME=GAME`, `--list-materials`). Lighting: quick (a flat ambient) or your own vertex
colours and lightmap.

**Light the room** (Room menu; `light <project> --room ROOM [--match] [--shadows]`) bakes the static geometry's
vertex colours from the room's lights. It skips an object showing a LOD set or selector, with a note.

## Connect a zone: the way in and back

**Connect a zone** (task; `zone <project> --mod MOD --connect ZONE --from-level LEVEL [--at X,Y,Z]`): an exit
copied from a retail one into a game level, and the way back in the zone. WW and SoT use the ChangeMap trigger (a
fade and a restart at the zone's checkpoint); T2T has no ChangeMap and uses the streaming portal (the zone's room
moves to the doorway). A connected zone cannot be removed until its exits are.

Connect and a patrol network (`navigation --network`) work in either order. A Connect is refused only when it
would make the zone's room ask for keys none of its level's worlds holds, and it names them.

## Rooms, portals, order

- **Level setup > Rooms:** add an Empty room to a game level or a zone (`zone --add-room NAME --level LEVEL`),
  rename a zone (`zone --rename`), list rooms, portals and occluders (`zone --rooms`).
- **Rooms, portals and occluders** editor (`zone --visibility LEVEL`, `zone --room R --object K --make-room |
  --make-portal A,B | --make-occluder | --remove-visibility`): WW and SoT rooms and portals; T2T occluders only.
  A zone's own room can be one too: on WW its list of objects is placed where the zone's room loads it.
- **Order and Special Load** (`level --order`, `level --special-load`): the level order and openjade's Special
  Load list.
- **Story variants** (WW; `level --variants`): the 0xEE worlds a level loads by story condition.
- **Player start** (`level --level L --loadout ...`): what the Prince starts the level with. See
  [Bosses and story sets](bosses-story-sets.md).

## Checks

- **Level setup > Check level** (`level --level L --check [--fix]`): missing move kits, no bank set, shared
  collision, values naming objects the level never loads, enemies without navigation, pits with no kill zone, fog
  away from the room, objects lit by another room's colours, WW traps nothing starts.
- **Traversal check** (the level view overlay; `traversal <project> --room ROOM [--jump X,Y,Z@DX,DY]`): running
  jumps, wall runs, column catches, wall jumps, chimneys and climbing down, by each game's rules.
