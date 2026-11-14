# Enemies and navigation

## Enemies

Enemies appear the way the game makes them appear: a **spawner** names an enemy **template** (an `ACT_` actor of a
technical world, or a variant you made); the spawner makes a copy at each spawn point when the Prince walks into its
trigger zone (or when the room loads). No copy of the enemy itself goes into your room.

- **Create > Enemy spawner** (or Mod > Create new): the enemy (or a family), the point, **More spawn points**, the
  trigger zone or "on load". A spawner holds 5 enemies; more points get another spawner.
- **Families:** each family brings its own retail spawner group and its weapons (WW 13, SoT 3, T2T 10 families with
  a group; the rest use the default group). `enemy <project> --families` lists them.
- **New enemy variant** (task): an enemy of your own (skin, stats, behaviour, animations). See
  [Textures, models and characters](textures-models-characters.md).
- **Swap a level's enemies** and **Enemy difficulty** (tasks): see [Swaps](swaps.md).
- **Crow flock** (Create, WW): 105_Hall's crows manager with its points and zone.

CLI: `enemy <project> [--mod MOD] (--enemy KEY [--source ROOM] | --family ID) --at ROOM@X,Y,Z [--at ...]
[--trigger X,Y,Z [--trigger-size W,D,H] | --on-load] [--name NAME]`. A template that lives in a zone's room needs
`--source ROOM`.

## Navigation grids

Enemies walk on grids only: 1 m samples over the floor, one height per grid. **Without a grid enemies stand still**,
and WW / T2T spawners that need one refuse to start. Empty and Blockout zones get a grid; copied levels keep theirs.

In the level view: **Room > Enemy navigation...**

- **Show navigation**, **Generate** (from the room's collision; **Generate a grid here**, **Grids where enemies
  stand**), **Paint** walkable / blocked (one undo step per stroke).
- **Link grids** (grids at different heights, where an upper floor ends in a drop over a lower one within 3 m).
  Retail pairs exist only in SoT, where only the player-slot-1 object crosses them.
- **Drop ring** (T2T only, off by default): code 4 where the floor ends in a drop, as retail's rooftop rooms do.

CLI: `navigation <project> --room ROOM --list | --generate [--seed X,Y,Z] [--new-grid | --replace KEY]
[--drop-ring] | --link | --remove KEY`, `nav-paint <project> --room R --box X0,Y0,X1,Y1 --code walkable|blocked`.

## Patrol networks

A patrol network is a route of nodes an enemy walks.

- **Generate network** (Enemy navigation...): from the picked point (else where the view looks), shape **loop**
  (SoT / T2T default), **chain** or **both ways** (WW default), the number of stops. Nodes go on walkable floor;
  edges only where the line stays on walkable floor.
- **Name the root:** pick the network, a spawner and a slot: the enemy of that slot appears on the root and
  patrols the network (WW / T2T `destTable`, SoT `hash_99E6CCB9`).
- **Move nodes:** drag a node's cross in the level view; edges that leave walkable floor are named.

CLI: `navigation <project> --room R --network [--at X,Y,Z] [--shape loop|chain|both-ways] [--stops N]`,
`--enemies`, `--name-root SPAWNER [--node KEY] [--slot N]`, `--move-node KEY --to X,Y,Z`.

## Limits

- SoT's 0x10 / 0x18 path-cost codes are not generated (no rule in the room's collision explains them).
- The Empty template's retail floor has rim-only collision; Blockout zones use your own floor.
