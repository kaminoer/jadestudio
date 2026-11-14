# Tutorial b: a new zone with parkour, a checkpoint, enemies and an exit

You make a level of your own from Blockout geometry, add a swing bar (or column), a ladder and a light from the
Level kit, a second checkpoint, two enemies with a navigation grid and a patrol network, a kill zone under the floor,
and an exit from a game level into your zone and back. Works on SoT, WW and T2T.

Sample script: `samples/tutorial-b-zone.ps1` (see [Sample mods](../samples.md)).

## Before you start

- A mod (Mod > New mod...). The CLI's `--mod NAME` makes it when it does not exist.
- Blockout geometry: a `.glb` from Blender or any 3D tool, with a floor. To try first:
  `jadestudio-cli import-geometry --sample blockout.glb --spawn` writes a 10 x 10 m floor, a ramp, a wall, a pillar
  and a `spawn` node.

**One order on every game:** the zone, everything inside it, then the exit (step 7). The exit can also be made
with the zone (New zone's **Connect it to a game level**, `zone ... --from-level`): the order does not
matter. A T2T portal moves the zone's room to the doorway, and the zone's recorded origin moves with it, so "From the
zone's origin" and `--local` still land in the room afterwards.

## 1. The zone

**Studio:** Mod > Create new > New zone... Name `MyZone`, template **Blockout**, your `.glb`. Leave **Connect it to
a game level** off (the exit comes in step 7). **Plan the zone**, read the plan, **Create**. The zone opens in the
level view.

**CLI:**

```
jadestudio-cli zone <project> --mod my_zone --template blockout --name MyZone --file blockout.glb
```

The output says the **zone origin** (1500, -1500, 0 by default): your file's 0,0,0
stands there. The zone has a checkpoint on the highest walkable top near the origin (or at the `spawn` node), a
navigation grid, the game's move kits and a bank set.

## 2. Parkour from the Level kit

**Studio:** Mod > Create new > Place from the Level kit... (the Prefabs panel, Level kit). Tick **From the zone's
origin** at the top and type 2, -3, 0 (or click **Where the view looks**). Pick **Swing bar** (WW, SoT) or **Column**
(T2T) in the Parkour row, **Place**. Then a ladder against the wall (-2, 4, 0) and a light from the Lighting row
(0, 0, 3).

**CLI:**

```
jadestudio-cli kit <project> --list
jadestudio-cli kit <project> --mod my_zone --place ww-swing-bar --at MyZone@2,-3,0 --local
jadestudio-cli kit <project> --mod my_zone --place ww-ladder-6 --at MyZone@-2,4,0 --local
jadestudio-cli kit <project> --mod my_zone --place ww-light-wall-torch --at MyZone@0,0,3 --local
```

The room is named by the zone's name (`MyZone`) or its pack name. After lights, Room > Light the room... bakes the
room again (`light <project> --room MyZone --match`).

## 3. A checkpoint

**Studio:** Mod > Create new > Create... > Spawns & checkpoints > **Checkpoint**. Tick **From the zone's origin**,
point -2, 2, 0, name it, **Create and select**.

**CLI:**

```
jadestudio-cli create <project> --mod my_zone --what checkpoint --into MyZone@-2,2,0 --local --name MyZone_Check2
```

## 4. Enemies

**Studio:** Create... > **Enemy spawner**: the family (or an enemy), the point, **More spawn points** (or **Add the
view's point as a spawn point**), the trigger zone near the start. **Create and select**.

**CLI** (world coordinates: the origin plus your offsets):

```
jadestudio-cli enemy <project> --families
jadestudio-cli enemy <project> --mod my_zone --family pirate --at MyZone@1503,-1497,0 --at MyZone@1503,-1502,0 --trigger 1501,-1499,0
```

The spawner's group comes from a retail room with its weapons; the spawn points that no grid covers get one.

## 5. Navigation and a patrol network

**Studio:** in the level view, **Room > Enemy navigation...**: **Generate** (the grid from the room's collision,
including the kit items). Then pick a point on the floor and **Generate network** (shape and stops beside it).
Then pick the network, the spawner and slot 0, **Name the root**: that enemy appears on the root and patrols.

**CLI:**

```
jadestudio-cli navigation <project> --mod my_zone --room MyZone --generate
jadestudio-cli navigation <project> --mod my_zone --room MyZone --network --at 1501,-1501,0 --stops 4
jadestudio-cli navigation <project> --mod my_zone --room MyZone --enemies
jadestudio-cli navigation <project> --mod my_zone --room MyZone --name-root <spawner key> --slot 0
```

## 6. A kill zone under the floor

The Blockout floor has open edges; the level check warns "pit". **Studio:** Create... > Zones & triggers > **Kill
zone** (kind fall) below the floor. **CLI:**

```
jadestudio-cli create <project> --mod my_zone --what kill-zone --into MyZone@0,0,-6 --local
```

## 7. The exit

**Studio:** Mod > Create new > Connect a zone...: the zone, the level (the sample uses WW `055_ExitTo045`, SoT
`0101_Entree`, T2T `100_Quai`), **Plan**, **Connect**. WW and SoT get a ChangeMap in the level's room and a way back
in the zone; T2T gets a portal, and the zone's room (with its recorded origin) moves to stand beyond the doorway. One
undo step.

**CLI:**

```
jadestudio-cli zone <project> --mod my_zone --connect MyZone --from-level 055_ExitTo045
```

A Level kit item placed after it with **From the zone's origin** / `--local` still lands in the room (on T2T too).

## 8. Check it

**Studio:** Level setup > **Check level** (the level view toolbar's Level setup...), the level view's Traversal
overlay, Play > Check the mod now.

**CLI:**

```
jadestudio-cli level <project> --mod my_zone --level MyZone --check
jadestudio-cli traversal <project> --mod my_zone --room MyZone
jadestudio-cli check <project> --mod my_zone
```

## 9. Play and build

**Studio:** **Play** (F5) with the zone in the Play bar's level picker: it starts at the zone's checkpoint. Or walk
into the exit in the game level. Build > Build and share... for the `.jtmod`.

**CLI:**

```
jadestudio-cli zone <project> --mod my_zone --list
jadestudio-cli play <project> --level MyZone
jadestudio-cli use-mod <project> my_zone
jadestudio-cli cook <project> my_zone.bf --verify
jadestudio-cli jtmod <project> my_zone.jtmod
```

## What was checked

Every step ran on copies of the three games (see [Sample mods](../samples.md) for what refused where).
