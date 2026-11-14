# Cross-game import

Bring things from another Prince of Persia game's unpacked project into this one. The other project is only read.
In the Studio: **Prefabs > Other games** (Mod > Create new > Import from another game...), then pick the other
game's project. Settings > Other games remembers the folders.

| What | Studio (Other games >) | CLI |
|---|---|---|
| Surfaces (textures) and static props | Surfaces, Props | `import-from <project> --source OTHER --object KEY --into ROOM@X,Y,Z` (`--texture KEY`: a surface) |
| Particles, lights, fog | Effects | `effects <project> --other GAME`, `--import GAME:KEY --room ROOM --at X,Y,Z` |
| Whole characters | Characters | `import-character <project> --source OTHER --character KEY --into ROOM[@X,Y,Z]` (`--list` first) |
| Behaviours | Behaviours | `import-behaviour <project> --from GAME --source ROOM:OBJ --to ROOM:OBJ,...` |
| Animation clips | Tasks > Animation bank > New clip > From another character's clip | `animation <project> new-clip --from ROOM:KEY --from-game GAME` |
| Sounds | Sounds panel > Sound of another game | `sound import <project> --from GAME BANK EVENT TARGETBANK` |
| Game materials | Create > Game materials of another game | `create --what game-material-import` |

## Whole characters

A character of another game becomes a **variant of this game's closest enemy** (ranked by bones matched by name,
then hip height; bosses last): that enemy's skeleton objects, AI and cloned behaviour carry it; the source's skinned
meshes are refitted onto that skeleton by bone name; the source's pictures go over the base's textures; a bank of its
own holds the source's clips retargeted (the first 8 by default). **Carry its own behaviour** carries the source's
behaviour instead when every call it makes has a counterpart here; else the base's clone stays and the refused calls
are listed. The retail characters tried so far (T2T and WW Mahasti, SoT SandGuard) all call other behaviours'
functions, so they keep the base's behaviour.

Refused in words: a character of this game, one with no bones, bank or skinned mesh, a base with fewer than half the
bones.

See [Tutorial e](tutorials/e-character-from-another-game.md).

## Limits

- A base's materials are kept: two source materials landing on one slot show the first.
- Calls into other behaviours' functions never carry (function ids are per game).
- A sound event's random pick or sequence is kept; chained or switched events are refused in words (import each
  sample as its own sound instead).
