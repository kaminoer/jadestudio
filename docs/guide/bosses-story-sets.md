# Bosses and story sets

## Boss arena (Create > Boss arena)

A retail boss's whole working set (its spawner, intro and after-fight cutscenes, markers, trigger zones, props)
is copied into your room with new keys, the boss's marker at the point you choose. The level lists the boss's world.
Objects of other rooms the set needs are copied too, and every value is renamed to the copies; the plan proves the
copy is **closed** (no copy names an object of the retail room).

| Game | Bosses |
|---|---|
| WW | Shahdee (deck, Sacrifice Room), Kaileena (Throne Room, Central Hall 2nd / 3rd; her summoned guards come with her), Griffin, Dahaka final |
| SoT | Vizier, Sand King wave |
| T2T | Twins, Klompa, Mahasti, Vizier (with their speed kills and cameras) |

Settings: **Boss** (auto = the game's first), **Intro** (retail, animation, none), intro animation, skip intro,
spawn mode, **After the fight** (retail or none; a change of level is always left out).

CLI: `create <project> --what boss-arena --into ROOM@X,Y,Z [--local] --set boss=ID [--set intro=...] [--set after=none]`;
`create --describe boss-arena` lists the ids.

The fight needs a navigation grid where the boss stands: generate the room's navigation afterwards. Left out on
purpose: WW's GP710 story manager, SoT's sand vision and monster-room portal, the T2T Vizier's teleport. No boss has
throw-target data in the trilogy.

## Story sets (Create > Story set)

| Set | Game | What it is |
|---|---|---|
| ww-life-upgrade, -045 ... -425 | WW | the nine life-upgrade walls; **Life upgrade** setting (`reward=` retail, auto or a bit 1..256) picks the bit (auto: the first bit no wall of the mod grants) |
| ww-ending, ww-ending-dahaka | WW | Kaileena's Central Hall fight and its ending; the Dahaka ending |
| t2t-ending | T2T | the final Sand Prince fight and its scene |
| t2t-chariot-race, t2t-chariot-enemy | T2T | a chariot race's start, an enemy chariot zone |
| t2t-life-upgrade-statue | T2T | the life-upgrade statue of 810_LU_STATUE with its sound trigger and its glow and tunnel sound actors (copied from the level's sound world, so the statue's sounds are wired to the copy) |

SoT has no story set (refused in words). CLI: `create <project> --what story-set --into ROOM@X,Y,Z --set set=ID
[--set reward=auto]`; `create --describe story-set` says which bit grants what.

## Time portal (Create > Time portal, WW)

A copy of TimeGate_Tools' time warp whose table points at your destination room: `create --what time-portal --into
ROOM@X,Y,Z --set to=ROOM2 --set land=KEY [--set era=past|present] [--set fountain=N]`. Both rooms stay loaded: to
hide the other era, make each era a visibility room with no portal between them (Level setup > Rooms, in a game
room or a zone's own room). SoT and T2T refuse in words.

## Story items of the Level kit

- WW: become the Sand Wraith, back to the Prince, Sand Wraith worlds load / unload.
- T2T: to the Dark Prince, back to the Prince, water turns him back.
- SoT: Farah snipe point, hole, teleport point, goal zone; the SoT ending (end video, credits).
- WW: Dahaka chase (with leaps), tentacles, water curtain (Chase row).

## Player start (Level setup > Player start)

What the Prince starts the level with. WW: powers, sand tanks, life, sword, secondary weapon. SoT: the Dagger of
Time, sword 0-3, sand tanks 4-10, power tanks, life. T2T: powers 0-4, tanks 3-6, life 0-6, primary (sword, Dagger of
Time, King's sword) and secondary. CLI: `level <project> --mod MOD --level L --loadout [--powers ...] [--tanks ...]
[--life ...] [--sword ...] [--dagger yes|no]`.
