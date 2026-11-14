# What each game supports, and why

Most of the Studio works the same on The Sands of Time (SoT), Warrior Within (WW) and The Two Thrones (T2T). Where a
game refuses something, the Studio says why in words; this page collects those differences. The reasons come from
the games' own data (what retail objects and scripts exist) and from openjade's engine code.

## Differences by area

| Area | SoT | WW | T2T |
|---|---|---|---|
| New zone: how it starts | through `--map 0101_Entree --checkpoint <zone's>` (works with every openjade) | its own level and checkpoint | its own level; also a row in Global's level-defaults table |
| Connect a zone (the exit) | ChangeMap trigger (a fade, a restart at the zone's checkpoint) | ChangeMap trigger | streaming portal: T2T has no ChangeMap, the zone's room moves to the doorway |
| Special Load lists the zone | yes, with an openjade that lists a mod's levels in Special Load | yes | yes |
| Player start (loadout) | dagger, sword 0-3, sand tanks 4-10, power tanks, life 0-10 | powers 0-9, tanks, life, sword, secondary | powers 0-4, tanks 3-6, life 0-6, primary, secondary |
| Enemy families with a spawner group | 3 | 13 | 10 |
| Enemy difficulty task | yes (health, and damage as a decimal value) | yes | yes for Reptus, Chameleon, Hunter, Enchantress, Illusion (health); damage in whole steps; the others keep their scripted numbers |
| Patrol network default shape | loop | both ways | loop |
| Navigation special codes | 0x10 / 0x18 path costs not generated (no rule in the collision explains them) | - | drop ring (code 4) on request, off by default |
| Linked grids | the engine crosses them only for the object in player slot 1 | crossed when the search allows | crossed when the search allows |
| Music zones | refused: SoT has no music zone object | sector objectType 16 | sound trigger F30000E5 |
| Music and ambience panel | refused (none in its data) | yes | yes |
| Vorbis sound | refused | an `.ogg` kept as Vorbis (a WAV is not encoded) | refused |
| New bank name length | 11 | 32 | 3 |
| Post effects (controllers 39-41) | refused: none in its archive | yes | yes |
| Rooms and portals | rooms and portals (by world key) | rooms and portals | occluders only (retail T2T has no rooms or portals) |
| Story variants (0xEE worlds) | none (said so) | 21 variant worlds | none (said so) |
| Story sets | refused: none built | life-upgrade walls, endings | endings, chariots |
| Time portal | refused | yes | refused |
| Crow flock | - | yes | - |
| Animated texture (type 9) | yes | refused: no retail one | refused: no retail one |
| Cutscene line | voice channel written in WW's shape (no retail SoT cut says a line) | as retail | as SoT |
| Cutscene actors | on the camera (as WW) | on the camera | through a copy of 100_Quai's scripted event |
| Font glyph spacing | refused (SoT drops such a font) | yes | yes |
| Fonts outside the archive | POPData.bf: refused, openjade reads it | - | POP3_54.MFT: refused, openjade reads it |
| Farah | snipe point, hole, teleport, goal zone (Level kit); co-op refused | - | refused: her T2T object is a cinematic body with no follow or shoot behaviour |
| Dark Prince, chariots | - | - | Level kit Story items, story sets |
| Sand Wraith, Dahaka chase | - | Level kit | - |
| Speed-kill hints | - | - | Level kit Tutorial row |
| Save caption numbers | 0-42 | 0-53 (and Chapter.<n>) | 0-47 |
| Traversal rules | wall run without a facing test; in-air catch 0x20 or 0x40; beam end under a low ceiling | the reference rules | wall run and beam end as WW; in-air catch 0x20 or 0x40 |

## Kit items per game

See the table in [The Level kit and Create](level-kit-and-create.md). Each item comes from a retail room of its own
game; `kit <project> --list` shows the project's.

## Bosses per game

See [Bosses and story sets](bosses-story-sets.md): WW Shahdee, Kaileena, Griffin, Dahaka; SoT Vizier, Sand King;
T2T Twins, Klompa, Mahasti, Vizier.

## Sample mods per game

The sample scripts ([Sample mods](samples.md)) ran on copies of all three games on 2026-11-14; that page lists which
steps refused where.
