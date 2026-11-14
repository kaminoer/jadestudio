# Tutorial e: bring a character from another game

You bring a whole character of another Prince of Persia game into a zone of this one (for example T2T's Mahasti into
WW), as a variant of this game's closest enemy, then make it appear through a spawner. Works between any two of SoT,
WW and T2T.

Sample script: `samples/tutorial-e-other-game-character.ps1` (see [Sample mods](../samples.md)).

## Before you start

- The other game unpacked too (its project is only read). Settings > Other games remembers its folder.
- A zone to put the character in: a Blockout zone `MyGuest` ([Tutorial b](b-new-zone.md), step 1).

## 1. List the other game's characters

**Studio:** Mod > Create new > Import from another game... (Prefabs panel > Other games) > **Characters**, pick the
other game's project. Each character shows its bones, meshes, vertices and clips; one that cannot come over says why.

**CLI:**

```
jadestudio-cli import-character <project> --source <other project> --list
```

The keys of the characters the Studio's tests use: T2T `6603A4BD` ACT_Enemy_Mahasti (into WW), WW `13016FBF`
ACT_Enemies_Mahasti (into SoT), SoT ACT_SandGuard_01 (into T2T).

## 2. Choose the base

The character becomes a variant of one of this game's enemies: its skeleton objects, AI and cloned behaviour carry
it. The panel ranks the bases (bones matched by name, then hip height; bosses last) and picks the closest.

**CLI:**

```
jadestudio-cli import-character <project> --source <other project> --character 6603A4BD --bases
```

`--base FAMILY` or `--base ROOM:KEY` picks another.

## 3. Import

**Studio:** the clips to carry (the first 8 by default), a name, **Carry its own behaviour** ticked, then **Import where
the level view looks**. One undo step.

**CLI:**

```
jadestudio-cli import-character <project> --source <other project> --character 6603A4BD --into MyGuest@1502,-1502,0 --mod my_guest --carry-behaviour
```

The output ends with `imported as <name> (<key>)`: the new template's key. The plan lists every call of the source's
behaviour that has no counterpart here; when one is refused, the base's behaviour runs instead (all the retail
characters tried so far fall back: they call other behaviours' functions, whose ids differ per game).

## 4. Make it appear

The imported character is a template, like the game's enemies. A spawner makes it appear.

**Studio:** Create... > **Enemy spawner**, the new character as the enemy, then Room > Enemy navigation... >
**Generate**.

**CLI** (the template lives in the zone's room, so name it with `--source`):

```
jadestudio-cli enemy <project> --mod my_guest --enemy <key> --source MyGuest --at MyGuest@1503,-1497,0 --trigger 1501,-1499,0
jadestudio-cli navigation <project> --mod my_guest --room MyGuest --generate
```

## 5. Look at it, check, play

Open the zone in the level view; the animation editor's Skeleton tab (Mesh) shows the refitted mesh posed by its
carried clips. Then:

```
jadestudio-cli check <project> --mod my_guest
jadestudio-cli play <project> --level MyGuest
```

## What was checked

The sample imported and spawned a character on copies of the three games (see [Sample mods](../samples.md)).
