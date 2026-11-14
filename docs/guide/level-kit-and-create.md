# The Level kit and Create

Two ways to add things to a room:

- the **Level kit** copies a retail group that already works (a swing bar, a trap with its trigger zone, a crank
  and the gate it opens) with new keys;
- **Create** makes a new, clean object or resource: every value at the game's default, nothing linked, new keys.

## The Level kit (Prefabs panel > Level kit)

Mod > Create new > Place from the Level kit... opens it. Pick an item, then **Place** (or **Preview**) where the level
view looks, or click in the level view. **From the zone's origin** (top of the Prefabs panel) takes the point
relative to the zone's origin; **Where the view looks** fills it in. A placed item moves as one group.

Rows: Parkour, Traps, Mechanisms, Pickups, Lighting, Cameras, Chase (WW), Tutorial (T2T), Story, Interactive.
`kit <project> --list` lists the items of the project's game.

| Game | Items (examples) |
|---|---|
| WW (64) | swing bar, trapeze, column, beam, ladders 6-15 m, rope, wall banners, moving ledge; spike tiles, spike line, spin blades, blade and axe pillars, wall saw, crusher; crank + pole / rotating wall / lift column, lever + gear, lever + raising bridge, plate + rising platform, collapsing floor, irrigation puzzle; story gate; torches, braziers, lamps; camera + alternate; Dahaka chase, tentacles, water curtain; Sand Wraith items |
| SoT | ladders, column, swing bar, beams, rope; hacksaw, spiked jaws, rotating blade; crank + gate, plate + gate, push block, plate + lift, elevator (zone), drawbridge, trapeze + drawbridge, mirrors, light-ray puzzle; sand cloud, fountain, save fountain, sand vision, dream portal; Farah snipe point, hole, teleport point, goal zone; light, camera |
| T2T | ladders, column, swing bar, beam, rope; disk-blade, arrow trap; plate + door, dagger switch + door, chain switch + door, push block, falling door, breaking beams, rebound plate, shutter, chain swing, chain wall, wall slide; save fountain, vase, life-upgrade statue; light, cameras; speed-kill hints; Dark Prince switches |

CLI: `kit <project> [--mod MOD] --place ID --at ROOM@X,Y,Z [--yaw D] [--local]` (traps: `--start zone|load`,
`--facing`, `--wall`, `--approach`; lights: `--range A,B --colour R,G,B`), `kit --list-placed`,
`kit --move ID --to ROOM@X,Y,Z`. Interactive items (levers, doors, fountains...): `kit --item ID --at ... [--with
X,Y,Z]` (`kit --help-interactive`).

Placing an item adds the move kits the level needs to its world list (`--no-worlds`: only noted). After lights,
**Light the room** bakes the room again.

## Create (Mod > Create new > Create...)

The Create page lists every creator by category. Placed creators take a point (**At the view's eye**, the picked
point, or **From the zone's origin**). **Create and select** writes it as one undo step.

| Category | Creators |
|---|---|
| Lights | Omni, Spot, Directional, Ambient light, Fog |
| Zones & triggers | Trigger zone, Script zone, Kill zone |
| Spawns & checkpoints | Checkpoint, Level start, Enemy spawner, Spawn point, Crow flock (WW), Boss arena, Time portal (WW), Story set |
| Effects & sound | Sound emitter, Particle emitter, Music zone (WW, T2T), New particle effect, Post-effect volume (WW, T2T) |
| Cameras & paths | Game camera, Waypoint |
| Helpers | Locator |
| Textures & materials | Texture, Material, Multi-material, Texture of another kind |
| Meshes & collision | Static mesh, Collision, LOD selector, Game-material table, Game materials of another game |
| Animation | Animation entry, Animation clip |
| Sound | Sound event, Level sound banks |
| Text | Menu text, Tutorial line, Subtitle line |
| Scripts & behaviours | Behaviour, Variable, Script function, Script file |
| Navigation | Navigation grid |
| Levels | Empty zone, Blockout zone |

A creator a game cannot have is listed with why (`create --list --all`).

CLI: `create <project> --list`, `create --describe ID` (its settings and defaults),
`create <project> [--mod MOD] --what ID [--into ROOM@X,Y,Z [--local]] [--yaw D] [--name N] [--set KEY=VALUE]...`.

Useful settings: Kill zone (fall: a box below the point; instant: on it; active or armed), Enemy spawner (`more`:
more spawn points, `KEY@dx,dy,dz` another enemy; the page's **Add the view's point as a spawn point**), Game camera
(`zone=KEY`, the zone it serves), Story set and Boss arena (see [Bosses and story sets](bosses-story-sets.md)).

## Prefabs

The Prefabs panel also has the **prefab library** (made from your install with **Make the prefab library**; WW 528,
SoT 498, T2T 489 prefabs), your own prefabs (**Save selection as prefab**) and **Other games** (see
[Cross-game import](cross-game.md)). CLI: `prefab <project> --list [--category C] | --place ID --at ROOM@X,Y,Z [--local]`.
