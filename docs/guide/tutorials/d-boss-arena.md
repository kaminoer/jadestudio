# Tutorial d: a boss arena

You copy a retail boss fight into a zone of your own: the boss's spawner, its intro cutscene, its markers, trigger
zones and props, then the navigation grid the fight needs, a checkpoint before it, and a kill zone under the floor.
Works on SoT, WW and T2T.

Sample script: `samples/tutorial-d-boss-arena.ps1` (see [Sample mods](../samples.md)).

## 1. A zone for the arena

Make a Blockout zone `MyArena` ([Tutorial b](b-new-zone.md), step 1). A flat floor of 10 m or more is enough to try;
the bosses' own arenas are larger.

## 2. Pick the boss

**Studio:** Mod > Create new > Create... > Spawns & checkpoints > **Boss arena**. The **Boss** list shows the game's
bosses; one the Studio cannot copy says why. **auto** takes the game's first.

**CLI:**

```
jadestudio-cli create --describe boss-arena
```

| Game | Ids |
|---|---|
| WW | ww-shahdee-sacrifice, ww-shahdee-deck, ww-kaileena-throne, ww-kaileena-hall-2, ww-kaileena-hall-3, ww-griffin, ww-dahaka-final |
| SoT | sot-vizier, sot-sand-king |
| T2T | t2t-twins, t2t-klompa, t2t-mahasti, t2t-vizier |

## 3. Place it

**Studio:** tick **From the zone's origin**, point 0, 0, 0 (the boss's marker goes there; the whole set turns about
it). **Intro**: retail (its cutscene), animation (the boss plays its intro animation where it stands) or none. **After
the fight**: none for a test (retail plays the after-fight cutscene; a change of level is always left out). **Create
and select**. The plan's "closed" line says no copy names an object of the retail room.

**CLI:**

```
jadestudio-cli create <project> --mod my_arena --what boss-arena --into MyArena@0,0,0 --local --set boss=auto --set after=none
```

It takes a while (WW Shahdee: about 20 s): the planner follows every value of the set into other rooms and worlds.

## 4. Navigation

The fight needs a grid where the boss stands (its spawner refuses without one: "No Grid of loaded. No Fight").

**Studio:** Room > Enemy navigation... > **Generate**. **CLI:**

```
jadestudio-cli navigation <project> --mod my_arena --room MyArena --generate
```

## 5. A checkpoint and a kill zone

```
jadestudio-cli create <project> --mod my_arena --what checkpoint --into MyArena@-3,-3,0 --local --name MyArena_BeforeFight
jadestudio-cli create <project> --mod my_arena --what kill-zone --into MyArena@0,0,-6 --local
```

(Studio: Create... > Checkpoint, Create... > Kill zone.)

## 6. Check and play

```
jadestudio-cli level <project> --mod my_arena --level MyArena --check
jadestudio-cli check <project> --mod my_arena
jadestudio-cli play <project> --level MyArena
```

Walk into the arena: the intro plays and the boss fights. A WW Kaileena arena brings her summoned guards' spawner;
T2T Twins, Klompa and the Vizier bring their speed kills and cameras.

## What was checked

The sample made the game's first boss (auto) on copies of all three games.
