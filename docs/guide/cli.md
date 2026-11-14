# CLI reference

This page is generated from `jadestudio-cli`'s own help, so it always matches what the CLI prints.
Every Studio action has a command here (and the reverse). Exit codes: 0 done, 1 usage or failure, 2 refused
(the reason is printed), 3 a check found something. `--dry-run` plans and prints only.

## Overview (`jadestudio-cli` with no arguments)

```
jadestudio-cli <command> ...

Workspace (a bfcook project; the studio keeps its data in <project>/jadestudio/):
  info <project> [--sha256]            game, archive, views, mods, active mod
  mods <project>                       the mods of the workspace (* = active)
  new-mod <project> <name> [--author A] [--version V] [--description D] [--title T] [--image card.png]
                                       creates a mod and makes it active
  use-mod <project> <name|->           makes a mod active (- : none, work on the base)
  mod <project> --delete|--rename|--duplicate|--merge|--import|--rebase|--conflicts|--overlaps|--install|
      --properties|--history|--undo|--redo|--additions|--revert-addition ...
                                       the mod's life: delete, rename, merge, import a .jtmod, rebase, conflicts,
                                       install into openjade, card, undo history (mod --help)
  behaviour <project> --list | [--mod MOD] --template ID --set FIELD=VALUE... [--dry-run]
                                       Add a behaviour: the game's own pieces wired (behaviour --help)
  check <project> [--mod MOD]          every changed file imported as the cook will (scripts compiled)
  video <project> [--mod MOD] --list | --replace TARGET --with FILE | --revert ID...
                                       the mod's video replacements (sound replace/add <project> for sounds)

Edits (the active mod's overlay; the base project is never written):
  set <project> <relative> <file> [--label L] [--scope S]
                                       the overlay copy of <relative> becomes <file>
  revert <project> <relative>          drops the overlay copy: the base file again
  changes <project>                    the mod's change log and overlay files
  in-place <project>                   base files edited in place (bfcook or by hand)
  adopt-in-place <project>             moves those edits into the active mod, restores the base

Builds (default output: <project>/jadestudio/builds/<mod>/):
  cook <project> [out.bf] [--paranoid] [--force] [--verify] [--against ref.bf]
  jtmod <project> [out.jtmod]          the active mod as a sealed JTMod v9 .jtmod
  check-jtmod <project> <mod.jtmod> <reference.bf> [--applied out.bf] [--keep]
                                       applies the .jtmod onto the base archive with the JTMod
                                       engine and compares the result with <reference.bf>
  unpack <archive.bf> <dir> [--flat] [--no-scripts] [--game sot|ww|t2t] [--overwrite]

Other:
  help guide [PAGE] | help tutorial [NAME]
                                       the user guide and its tutorials (docs/guide; Help > User guide)
  profile [sot|ww|t2t]                 the per-game facts the studio uses
  index <project> [--cache DIR] [--search Q] [--key HEX]
                                       the asset index: counts, a name search, one key's uses/used-by
  sound ...                            sound commands (jadestudio-cli sound --help)
  play ...                             play-testing in openjade (jadestudio-cli play --help)
  zone <project> --mod MOD --template empty|copy|blockout --name NAME [--from LEVEL] [--dry-run] | --list
       | --remove NAME | --connect ZONE --from-level LEVEL | --move NAME --by DX,DY,DZ|--to X,Y,Z
                                       a new level of the mod: make, list, remove, connect, move it (zone --help)
  mod <project> [--mod MOD] --clean-leftovers [--dry-run]
                                       the leftovers of removed levels taken out of the mod (mod --help)
  import-geometry ...                  level geometry from a glTF file (jadestudio-cli import-geometry --help)
  light <project> [--mod MOD] --room ROOM [--ambient R,G,B] [--shadows] [--occlusion] [--prefabs] [--dry-run]
                                       Light the room: bakes static geometry's vertex colours (light --help)
  light <project> [--mod MOD] --room ROOM --set KEY [--range A,B] [--colour R,G,B] [--kind K] [--enable|--disable]
       [--main-bank on|off] [--dry-run]   one light object's light, as the Inspector edits it (light --help)
  repair-views <project> [--mod MOD] [--dry-run]
                                       keeps both when a view and its object's raw data were edited (repair-views --help)
  prefab <project> --list [--category C] [--search Q] | [--mod MOD] --place ID --at ROOM@X,Y,Z [--yaw D] [--dry-run]
                                       the prefab library: list it, place a prefab as the panel does (prefab --help)
  animation ...                        banks, clips, events, new clips, retargeting (jadestudio-cli animation --help)
  character ...                        a new enemy variant of a character (jadestudio-cli character --help)
  enemy <project> [--mod MOD] --enemy KEY --at ROOM@X,Y,Z [--at ...]... [--trigger X,Y,Z | --on-load] [--dry-run]
                                       enemies of the game through a spawner, no copy of them (enemy --help)
  navigation <project> [--mod MOD] --room ROOM [--list] [--generate [--seed X,Y,Z]... [--new-grid|--replace KEY]]
       [--drop-ring] [--remove KEY] [--link] [--network [--at X,Y,Z] [--shape S]] [--dry-run]
                                       a room's enemy navigation grids and patrol networks (navigation --help)
  level <project> --level NAME --worlds|--check ...
                                       the worlds a level loads, its move kits, the level checks (level --help)
  traversal <project> [--mod MOD] --room ROOM [--jump X,Y,Z@DX,DY] [--object KEY [--surface KIND]]
                                       the Prince's jump / wall-run / column queries on a room (traversal --help)
  cameras <project> [--mod MOD] --level NAME | --room ROOM [--copy KEY --into ROOM[@X,Y,Z]]
                                       a level's retail alternate cameras (CAMALT), listed and copied (cameras --help)
  copy <project> [--mod MOD] --from ROOM --objects KEY[,KEY...] --into ROOM[@X,Y,Z] [--unique]
       [--links bring|clear|keep] [--also KEY,...] [--lighting room|source] [--no-worlds] [--dry-run]
                                       objects copied into a room with their linked objects (copy --help)
  import-from <project> --source <other-game-project> --object KEY --into ROOM@X,Y,Z [--yaw D] [--scale S] [--dry-run]
                                       a static prop (or --texture KEY: a surface) of another PoP game (import-from --help)
  blender <project> --status | --start --key MESH [--launch] | --push | --pull [--send N] | --stop   Edit in Blender's session
  swap <project> texture|model|animation --key KEY (--file F | --export F) [--room R] [--match-size]   the swap wizards
  texture-pack <project> (--export DIR | --import DIR) [--room R]   texture packs (any size)
  rebind <project> --room R --object KEY --behaviour MODEL          point an object at another behaviour
  variables <project> --room R --objects K,... (--list | --set NAME=VALUE...)   stats / bulk variables
  nav-paint <project> --room R --box X0,Y0,X1,Y1 --code walkable|blocked   paint the navigation grid
  import-behaviour <project> --from GAME (--list ROOM | --source ROOM:OBJ --to ROOM:OBJ,...)   a behaviour of another game
  import-character <project> --source <other-game-project> --character KEY --into ROOM[@X,Y,Z] [--base F] [--dry-run]
                                       a whole character of another PoP game on this game's closest enemy (--help)
  particles <project> [ROOM:]KEY [--show] [--set FIELD=VALUE]... [--preview S] [--write-view FILE] [--dry-run]
                                       a particle generator as the particle editor shows and writes it (particles --help)
  effects <project> --post-effect [ROOM:]KEY [--set K=V]... | --other GAME [--search T] |
       --import GAME:KEY --room ROOM --at X,Y,Z   post effects; effects of another game (effects --help)
  object <project> [--mod MOD] --move KEY[,KEY...] --to ROOM@X,Y,Z [--yaw D] [--dry-run]
                                       objects moved as one, as the level view's gizmo writes it (object --help)
  view <project> <view file> --get [POINTER] | --set POINTER=JSON... [--remove POINTER] [--dry-run]
                                       a view's fields, as the editors' forms set them (view --help)
  raw <project> --pack PACK --list | --block KEY --extract FILE | --override FILE | --revert
                                       a block's bytes exported, overridden in the mod or put back (raw --help)
  object <project> --room ROOM --object KEY --structure | --add PART --from DONOR | --remove PART |
         --add-controller ... | --remove-controller I   parts and controllers of an object (object --structure --help)
  object <project> [--mod MOD] --room ROOM --object KEY --reparent PARENT|none [--dry-run]
                                       re-parent an object, keeping its place in the world (object --structure --help)
  object <project> --room ROOM --object KEY --collision [--shape I --move X,Y,Z --scale X,Y,Z --turn D |
         --paint --at X,Y,Z --radius R --material TAG]   an object's collision shapes and faces (object --collision --help)
  create <project> --list | --describe ID | [--mod MOD] --what ID [--into ROOM@X,Y,Z [--local]] [--yaw D]
       [--name N] [--set KEY=VALUE]... [--dry-run]   a new, clean object or resource (create --help)
  kit <project> --list | [--mod MOD] --place NAME --at ROOM@X,Y,Z [--yaw D] [--start zone|load] [--dry-run]
       [--range A,B] [--colour R,G,B]  a parkour object, trap or light (torch, brazier...) of the Level kit (kit --help)
  kit <project> [--mod MOD] --item ID --at ROOM@X,Y,Z [--with X,Y,Z] [--yaw D] [--hold S] [--dry-run]
                                       an interactive Level kit object that works (kit --help-interactive)
  ui <project> --fonts | --font KEY [...] | --pictures | --picture KEY --with PNG | --captions | --special-load | --not-data
                                       fonts, menu / HUD pictures, save captions, Special Load names (ui --help)
  menus <project> [--mod MOD] --list | --font ROLE FILE | --loading PNG [--world KEY] | --special-load L --world KEY
        | --menu-entry PAGE ACTION TARGET --label T   the mod's data for openjade's own menus (menus --help)
  live [--port N | --staging DIR] <command> ...   the running game: pick, teleport, camera, variables, breakpoints,
                                       unlocks, hot reload (live --help)
  level-wide <project> --level L --mood PRESET | --roster FROM:TO | --difficulty --health X --damage Y
                                       a level's mood, enemy roster swap, enemy difficulty (level-wide --help)
  simple <project> sound|video|text ...  the Simple-mode tasks: Replace a sound / a video, Change a text (simple --help)
  cutscene <project> [--mod MOD] --room ROOM --shot X,Y,Z@LX,LY,LZ[:S]... --line TEXT|ID [--trigger X,Y,Z] [--dry-run]
                                       an in-engine cutscene with a Prince line (cutscene --help)
```

## info

No help page of its own: see its line in the overview above.

## mods

No help page of its own: see its line in the overview above.

## new-mod

No help page of its own: see its line in the overview above.

## use-mod

No help page of its own: see its line in the overview above.

## mod

```
usage: jadestudio-cli mod <project> <action> ...

The mods of a workspace (the Mod menu does the same):
  --delete NAME                     moves the mod's folder to <project>/jadestudio/trash/
  --rename NAME --to NEW            renames a mod (folder, card, builds, undo history, dependencies)
  --duplicate NAME --to NEW         a copy under a new name
  [--mod INTO] --merge FROM [--theirs] [--dry-run]
                                    mod FROM's changes added to INTO (the active mod) as one undo step; a file
                                    both change keeps INTO's version (--theirs: FROM's); the conflicts are listed
  --import FILE.jtmod --name NAME   a new mod made from a .jtmod, for further editing (becomes active)
  [--mod MOD] --rebase [--check]    checks the mod against this workspace's game archive; without --check the
                                    files that no longer apply move to <mod>/rebase-left/ and the mod is
                                    marked as made on this archive (one undo step)
  [--mod MOD] --conflicts [--with OTHER.jtmod]... [--mods A,B,...]
                                    the conflict check of Build and share: the mod built to a .jtmod and
                                    compared with other .jtmods and/or other mods of the workspace
  --overlaps [--mods A,B,...]       without building: files two mods both change, rooms both add objects
                                    to, and exits two mods add into the same room (exit code 3 when any)
  [--mod MOD] --install DIR         Install into my openjade folder: cooks the mod and puts it in DIR as the
                                    game's archive, the original kept as <archive>.jadestudio-original
  --uninstall DIR                   puts the original archive back
  [--mod MOD] --properties [--title T] [--author A] [--version V] [--description D] [--image F]
       [--changelog TEXT] [--depends A,B | --depends -]
                                    the mod card (prints it; changes what is given)
  [--mod MOD] --history             the mod's undo history (it survives a restart; CLI edits are in it)
  [--mod MOD] --undo | --redo       one step of that history back or forward
  [--mod MOD] --additions           the mod's structure changes (new bins, inserted blocks, added views)
  [--mod MOD] --revert-addition N   takes addition N of that list out (one undo step)
  [--mod MOD] --clean-leftovers [--dry-run]
                                    removes the leftovers of removed levels (the cook ignores them)
Exit codes: 0 done, 1 usage or failure, 3 a check found something (conflicts, overlaps, files that no
longer apply, dependency problems).
```

## behaviour

```
usage: jadestudio-cli behaviour <project> --list
       jadestudio-cli behaviour <project> [--mod MOD] --template ID --set FIELD=VALUE... [--dry-run]
Add a behaviour, as the task does: the game's own pieces wired (a lever opens a door, a zone starts enemies...).
VALUE: an object or area as ROOM:KEY (the room's pack name or #slot, the object's key in hex); a zone as its level
name or WOL key; a number or duration as a number. --list names every template and its fields.
```

## check

```
usage: jadestudio-cli check <project> [--mod MOD]
Every changed file of the mod imported onto the game through its converter, as the cook will: a script
that does not compile is listed with the compiler's message. Also the structure changes and the mods it
needs. Exit codes: 0 all apply, 3 problems listed, 1 failure.
```

## video

```
usage: jadestudio-cli video <project> [--mod MOD] --list
       jadestudio-cli video <project> [--mod MOD] --replace TARGET --with FILE
       jadestudio-cli video <project> [--mod MOD] --revert ID...
The mod's video replacements, as the Videos panel makes them. TARGET is relative to the game's Video folder
("Cine/CINE0001.bik"); FILE may be another format openjade plays there (an .mp4; a .png/.jpg for gallery items).
--list also lists the mod's sound operations (their ids revert with --revert or sound revert).
```

## set

No help page of its own: see its line in the overview above.

## revert

No help page of its own: see its line in the overview above.

## changes

No help page of its own: see its line in the overview above.

## in-place

No help page of its own: see its line in the overview above.

## adopt-in-place

No help page of its own: see its line in the overview above.

## cook

No help page of its own: see its line in the overview above.

## jtmod

No help page of its own: see its line in the overview above.

## check-jtmod

No help page of its own: see its line in the overview above.

## unpack

No help page of its own: see its line in the overview above.

## help

```
usage: jadestudio-cli help guide [PAGE]      the user guide: its index, or a page (id, file name or title)
       jadestudio-cli help tutorial [NAME]   the tutorials: their list, or one (a..e, its file name or title)
Options: --markdown prints the page as written; --width N wraps at N columns.
The pages are docs/guide/*.md next to Jade Studio; the Studio shows them
in Help > User guide and Help > Tutorials.
```

## profile

No help page of its own: see its line in the overview above.

## index

No help page of its own: see its line in the overview above.

## sound

```
usage: jadestudio-cli sound <command> ...
  list    <game folder>                          banks, event and sample counts
  events  <game folder> <bank>                   events and the samples they play
  sets    <game folder> [bank]                   bank sets (zones by WOL key)
  export  <game folder> <bank> <event|all|r:id> <out dir>      WAV files
  replace <game folder> <bank> <event|r:id> <in.wav|ogg> <out.patch> [--bank edited.sb0]
          [--stream NAME]   (streamed samples: whole bank + new stream file)
  add     <game folder> <bank> <in.wav|ogg> <out.patch> [--event 0xID] [--like 0xEVENT]
          [--ima] [--rate HZ] [--loop] [--bank edited.sb0]
  replace <project> [--mod MOD] <bank> <event|r:id> <in.wav|ogg>
          the sample replaced in the mod (as the Sounds panel; one undo step, a Changes row)
  add     <project> [--mod MOD] <bank> <in.wav|ogg> [--event 0xID] [--like 0xEVENT] [--ima] [--rate HZ] [--loop]
          a new event with a new sample in the mod
  revert  <project> [--mod MOD] ID...                the mod's sound/video operations taken out (ids: video --list)
  new-bank <project> [--mod MOD] <name|auto> [--like BANK]   a bank new to the game (BANK_ADD, Sound catalog)
  banks   <project> [--mod MOD]                      the mod's new banks and their sounds
  import  <project> [--mod MOD] --from sot|ww|t2t [--from-folder DIR] <source bank> <event> <target bank>
          [--event 0xID] [--ima|--pcm] [--each]   a sound of another game into a bank of this game (its
          random pick / sequence kept; --each: every sample a sound of its own)
  music   <project> [--mod MOD] --level NAME [--use N,N|music|all] [--pick 0xEVENT | --replace FILE [--bank NAME]
          [--like BANK] [--loop] [--vorbis]]   a level's music and ambience: list, point at another event, replace
  (add / replace into a project take --vorbis: a WW .ogg kept as a Vorbis sample; add takes a bank the mod adds)
  check   <game folder> [--no-decode]            round trips and decode-all report
  zone    <project> --mod MOD --level NAME [--like LEVEL] [--add BANK]... [--remove BANK]... [--none]
          a level's bank set (the set named after its WOL key) as mod MOD has it; --like copies
          LEVEL's set, --add / --remove change it, --none takes the mod's set out (one zone_audio
          operation of the mod's sound.json); without a change it prints the set (exit 3: none)
Banks: "prince", "prince.sb0", "sound/english/kl_vo1.sb0". Ids are hex.
replace/add exit 3 when no BANK_PATCH can carry the edit (a streamed sample, or a
bank jtmod cannot patch): the edited bank (and stream file) are written beside
out.patch for a whole-bank replacement (BANK_REPLACE, STREAM_ADD).
```

## play

```
usage: jadestudio-cli play <project> [--level NAME] [--checkpoint NAME.gao|KEY] [--spawn X Y Z [YAW]]
                           [--exec "CONSOLE COMMAND"]... [--openjade PATH] [--install DIR] [--staging DIR]
                           [--full] [--dry-run] [--no-wait] [--live-link]
       jadestudio-cli play log <openjade.log | play-test folder>
       jadestudio-cli play build <project> <out.bf> [--from MOD_OVERLAY_DIR] [--full]

play     builds the active mod (an overlay archive over the base archive; --full: a whole cooked
         archive), stages %LOCALAPPDATA%/Jade Studio/playtest/<game>/, starts openjade there at the
         start point, waits for it to exit and lists the problems in its log. --dry-run prints the
         staging layout and the command line and starts nothing (and builds nothing).
         openjade.exe: --openjade, else %JADESTUDIO_OPENJADE%, else <install>/openjade.exe.
         The install folder defaults to the workspace's game folder (media settings, else the base
         archive's folder), and Video/ then comes from its video folder (the Videos panel's).
         --spawn: engine coordinates (+Z up); YAW in degrees, from +X toward +Y.
         --live-link: the game listens for the live link on a free port, written to live-link.txt
         in the play-test folder, where jadestudio-cli live finds it.
         A new level of the mod (jadestudio-cli zone, the New Zone wizard) starts at its checkpoint
         unless --checkpoint names another; SoT runs --map 0101_Entree --checkpoint <the zone's>
         (works with every openjade; one that lists a mod's levels in Special Load also lists it there).
log      lists the script faults, script output, load errors and warnings of a log.
build    writes the overlay archive (or with --full the cooked archive) of a project and an
         optional mod overlay folder, and prints how long it took.
Exit codes: 0 ok, 1 usage or failure, 3 the game logged errors.
```

## zone

```
usage: jadestudio-cli zone <project> --mod MOD --template empty|copy|blockout --name NAME [--from LEVEL]
                           [--game sot|ww|t2t] [--floor ROOM:KEY] [--spawn ROOM:KEY]
                           [--no-level-defaults] [--dry-run]
                           blockout: --file FILE.glb|.gltf [--split nodes|materials]
                           [--lighting quick|vertex] [--ambient R,G,B] [--box]
                           [--material NODE_OR_MATERIAL=GAMEMATERIAL]... [--auto-uv METRES] [--subdivide METRES]
                           [--surface NODE_OR_MATERIAL=KIND]... (auto floor ledge nograb wall wallrun column pole beam)

Writes a new level <NAME>_wol_<key>.bin and its room(s) into mod MOD (created when missing):
  empty   one room: a retail floor and a checkpoint sector copied in (or --floor / --spawn,
          ROOM a pack name or key, KEY an object key, hex); the level lists the technical
          worlds (Prince, camera, HUD, move kits...) of --from (default: the game's choice).
  copy    every room of level --from that is not a technical world, under new keys.
  blockout  the empty room without the retail floor: the --file's geometry imported into it
          (jadestudio-cli import-geometry --help), the checkpoint on its highest walkable top
          face near the origin, or at a node named "spawn".
LEVEL: a level name ("055_ExitTo045"), its WOL key or its pack key (hex).
T2T: the level also gets a row in Global's level-defaults lookup (--no-level-defaults: not).
--game: refuse when the project is another game. --dry-run: plan and print only (key
reservations in mod.json are kept). Prints the openjade / jadestudio-cli play lines to start it.

       jadestudio-cli zone <project> --mod MOD --list
       jadestudio-cli zone <project> --mod MOD --remove NAME
--list: the mod's new levels, their bins and how each is started. --remove: takes a new level
out of the mod (its bins, its T2T level-defaults row; the mod's other zones are kept).
(Also: --rename ZONE --name NEW, --add-room NAME --level LEVEL [--at X,Y,Z], --rooms LEVEL: zone --rename --help)

       jadestudio-cli zone <project> --mod MOD --connect ZONE --from-level LEVEL [--room ROOM]
                           [--at X,Y,Z] [--heading DEG] [--checkpoint CP] [--zone-at X,Y,Z]
                           [--mechanism auto|changemap|portal] [--no-return] [--dry-run]
Connects the mod's zone ZONE to game level LEVEL: an exit copied from a retail one into ROOM (default:
the room under the point; a portal goes into the level's primary room) at X,Y,Z (default: 4 m ahead
of the level's checkpoint), and the way back in the zone. changemap (WW, SoT: a fade and a restart at
the zone's checkpoint; back at checkpoint CP of LEVEL) or portal (T2T: walked through; the zone's room
moves to the doorway, HEADING the way in, 0 = +Y). A new zone is connected as it is made with
--name ... --from-level LEVEL (and the same options). Prints what to try in the game.

       jadestudio-cli zone <project> --mod MOD --move NAME --by DX,DY,DZ | --to X,Y,Z|default [--dry-run]
Moves the mod's zone NAME in the world: every object of its rooms (objects the mod edits through a view
too; parked templates stay), its navigation grids and its cutscenes' camera keys and triggers, one undo step.
--to: where its origin goes (default: the game's zone origin, 1500,-1500,0, clear of the objects the
technical worlds keep near the world origin); --by: an offset. The origin is recorded in the mod's zones.json.
Blockout: --origin X,Y,Z|default|world places a new zone (default: the game's zone origin; world: 0,0,0).
Coordinates given to other commands (kit, enemy, cutscene...) are world coordinates; `kit --local` takes
them relative to the zone's origin.
Exit codes: 0 written (or planned), 1 usage or failure, 2 refused (the reason is printed).
```

### jadestudio-cli zone --rename --help

```
usage: jadestudio-cli zone <project> --mod MOD --rename ZONE --name NEW [--dry-run]
       jadestudio-cli zone <project> --mod MOD --add-room NAME --level LEVEL [--at X,Y,Z] [--dry-run]
       jadestudio-cli zone <project> [--mod MOD] --rooms LEVEL

--rename: a zone of the mod gets another name: its level pack (the Special Load entry and what --map takes) and
          the room packs named after it. Keys, contents and its bank set stay. One undo step.
--add-room: a new room in LEVEL (a game level or a zone of the mod): a retail floor and a checkpoint sector,
          as the New zone wizard's Empty room, standing at X,Y,Z (default 0,0,0); the level lists it before its
          main room. Its checkpoint restarts in LEVEL and is not the level's default start. One undo step.
--rooms:  the rooms LEVEL loads, their objects and their rooms/portals/occluders (controller type 36).
```

## import-geometry

```
usage: jadestudio-cli import-geometry <project> --mod MOD --room ROOM --file FILE.glb|.gltf
                           [--split nodes|materials] [--lighting quick|vertex] [--at X,Y,Z]
                           [--ambient R,G,B] [--box] [--prefix NAME] [--template ROOM:KEY] [--dry-run]
                           [--material NODE_OR_MATERIAL=GAMEMATERIAL]... [--auto-uv METRES] [--subdivide METRES]
                           [--surface NODE_OR_MATERIAL=KIND]... (auto floor ledge nograb wall wallrun column pole beam)

Imports the file's meshes into ROOM (a room of the game or of the mod's new zones: its pack name,
its name before _wow_, or its key in hex) of mod MOD (created when missing), one undo step:
  --split      nodes: one object per glTF node (default); materials: one per glTF material.
  --lighting   quick: a flat ambient in the vertex colours (--ambient, 0..1, default 0.5,0.5,0.5);
               vertex: the file's COLOR_0, and a lightmap a node's or material's extras.lightmap
               names (an image index or a file), drawn through TEXCOORD_1.
  --at         added to every position (metres, engine axes: +Z up).
  --box        boxes from the bounds instead of collision meshes.
Collision: each object's own mesh, or node <name>_col / <name>_collision for node <name>,
or extras.collision on a node: "box", "none" or another node's name.
A node named "spawn" marks where a Blockout zone starts.
Game materials: --material NAME=GAME (repeatable) makes the glTF material or node NAME draw the game's
material GAME (a key in hex or a curated name, --list-materials; texture:KEY for a game texture); a glTF
material's or node's extras.gameMaterial / extras.gameTexture does the same in the file. The material and its
textures are copied into the room under their own keys unless the room holds them.
  --auto-uv    box-projected UVs at METRES per texture tile (world space: neighbouring boxes tile on).
  --subdivide  cuts visual triangles to edges of at most METRES (for Light the room; collision keeps the file's).
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.

       jadestudio-cli import-geometry <project> --templates [--count N]
Lists the game's static objects a Blockout can be made from (mesh, multi-material, collision
mesh), smallest first, with counts of how retail static geometry is lit and collides.

       jadestudio-cli import-geometry <project> --mod MOD --room ROOM --apply OBJECT=GAMEMATERIAL [--slot N]
                           [--auto-uv METRES] [--subdivide METRES] [--dry-run]
Apply game material: static object OBJECT (hex) of ROOM imported again from its own mesh and collision with
GAMEMATERIAL on material slot N (default: every slot); it takes the old object's place in the room's list.

       jadestudio-cli import-geometry <project> --list-materials | --survey-materials [--count N]
The curated game materials (id, key, texture, metres per tile), or every plain material of the
game's baked static rooms by the area it covers.

       jadestudio-cli import-geometry --sample OUT.glb [--spawn]
Writes a small blockout to try: a 10 x 10 m floor (top at 0), a ramp with a simpler Ramp_col,
a textured wall, a vertex-coloured pillar (with --spawn: a node named spawn at 1,1,0).
```

## light

```
usage: jadestudio-cli light <project> [--mod MOD] --room ROOM [--ambient R,G,B] [--shadows] [--occlusion]
                           [--prefabs] [--objects KEY,KEY...] [--fog-from ROOM [--replace-fog]] [--match] [--dry-run]
Light the room: bakes the vertex colours of the room's static geometry the mod added (Import geometry;
with --prefabs also placed static prefabs) from the room's light objects (omni, spot, directional; the
engine's model: ambient + colour x N.L x linear falloff between the light's ranges). Dynamic objects (the
Prince, enemies, traps, anything animated or scripted) are never written: they take the lights at run time.
  --ambient    0..1 per channel (default: the colour of the room's ambient object, else 0.3).
  --shadows    a ray from each vertex to each light against the room's collision.
  --occlusion  darker ambient in corners (short rays against the room's collision).
  --match      match the room's other objects: every baked visual the mod added (also placed prefabs and kit
               items with behaviours, which keep their source room's colours otherwise) is baked, every
               visual lit at run time (flames, weapons) is linked to the room's ambient and fog, and the
               ambient object takes the bake's ambient colour: statics and dynamics share one ambient.
  --fog-from   the ambient light and fog pair most objects of game room ROOM use is copied into the room
               (when it has none: a Blockout zone) and the lit objects name it (the Prince takes his ambient
               colour from what he stands on; fog follows the visual's fog link). Same undo step. A room with a pair
               keeps it; --replace-fog copies ROOM's pair in instead and relinks every lit object to it.
Vertex colours show the texture x colour x 2 (0.5 = the texture as painted). Run it again after moving a
light. --dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.

       jadestudio-cli light <project> [--mod MOD] --room ROOM --set KEY [--range START,END] [--colour R,G,B]
                           [--kind omni|spot|directional] [--enable | --disable] [--main-bank on|off] [--dry-run]
Edits one light object of the room (a game room or a zone's) as the Inspector's Light component does: its
class resource's gro-json view (added when missing), checked as the cook imports it, one undo step.
  --range      START,END in metres (the light falls off linearly between them)
  --colour     0..1 per channel (stored doubled for old light versions, which the game halves)
  --main-bank  on: the light lights the Prince and enemies at run time (0x200); off: only the bake uses it
Then bake the room again: jadestudio-cli light <project> --room ROOM.

       jadestudio-cli light <project> [--mod MOD] --room ROOM --report
What the engine draws each object with (stored colours of baked visuals; ambient + selected main-bank lights
of dynamic ones) next to the baked floor under it; the room's lights (bank, bounds) and controllers.

       jadestudio-cli light <project> --survey
How the game's rooms are lit: light objects by kind and flags, torch lights, which visuals are baked.
```

## repair-views

```
usage: jadestudio-cli repair-views <project> [--mod MOD] [--dry-run]
A view of an object the mod adds (the level view's moves write one) whose raw data was rewritten since
(Light the room, ...) makes the cook refuse: "both <view> and the raw data of <key> were edited".
Keeps both: each such view is exported again from the new data with its own edits (a move, a link)
put back on top, checked as the cook imports it; one undo step of the mod. --dry-run: list them only.
Exit codes: 0 repaired (or nothing to repair), 1 failure.
```

## prefab

```
usage: jadestudio-cli prefab <project> --list [--category C] [--search Q] [--limit N]
       jadestudio-cli prefab <project> [--mod MOD] --place ID --at ROOM@X,Y,Z [--yaw DEGREES] [--links bring|clear|keep]
                             [--also KEY,KEY...] [--source-lights] [--template-alone] [--local] [--dry-run]

The prefab library (the Prefabs panel): objects of the game sorted into categories (Parkour, Interaction,
Hazards, Characters, Environment, and Mine: the prefabs you saved), made from the game's own data and cached in
<project>/jadestudio/cache/prefabs (made here when missing: about a minute on WW).
  --list       id, category, name, where it comes from; --category and --search narrow it (case ignored)
  --place      a prefab by id ("slot:key" or "mine:N" as --list prints it) or by its exact name
  --at         the room (pack name, stem or key; a zone's room too) and where the group's foot stands
  --yaw        turns it about the vertical (degrees), as the panel's Turn field does
  --local      --at is relative to the zone's origin (zone --move records it), as the panel's "From the
               zone's origin"; else world coordinates
  --links      what happens to the objects its values name (default bring: they come too)
  --also       objects of its room naming it, taken along (a trigger that starts it)
  --source-lights  the copies keep the source room's ambient / fog lights (default: the room's own pair)
  --template-alone an enemy the game only spawns is placed as its template (hidden, inactive); without it such a
               prefab is refused here (the panel places a spawner: jadestudio-cli enemy does that)
Placing is one undo step of the mod (the copy with new keys, the worlds its values name added to the level's list).
--dry-run: plan and print only. Exit codes: 0 written (or listed / planned), 1 usage or failure, 2 refused.
```

## animation

```
usage: jadestudio-cli animation <project> [--mod MOD] <action> ...

  bank ROOM CHARACTER            a character's bank, entry by entry (CHARACTER: its object key, hex)
  show ROOM CLIP                 a clip's channels (CLIP: a channel set or an animation key)
  pose ROOM CHARACTER CLIP --tick T [--no-root] [--mesh]
                                 the skeleton posed by the clip at tick T (joint positions, metres);
                                 --mesh: also its skinned meshes posed (bounds and vertex counts)
  rig ROOM CHARACTER [--bone B]  the character's rig: its bones in their tree with the vertices each moves;
                                 --bone (index or name): that bone's parent, children, weights, and where it
                                 sits at rest and at bind (the rig view of the animation editor and bank task)
  programs ROOM                  the programs a new event key can run (every program the room's clips run)
  matrices ROOM CLIP             the clip's matrix keys (camera and object-driving channels)
  event-add ROOM CLIP CHANNEL --tick T --program N [--sound EVENT] [--dry-run]
                                 a new key in event channel CHANNEL at tick T running program N of
                                 'programs' (its sound event changed with --sound); one undo step
  matrix ROOM CLIP CHANNEL KEY --values "16 numbers" [--dry-run]
                                 a matrix key's values (rows: X, Y, Z axes, position); one undo step
  events ROOM CLIP               the clip's event keys (footsteps, sounds, camera keys) in words
  event ROOM CLIP CHANNEL KEY (--move TICK | --remove | --copy TICK | --sound EVENT) [--dry-run]
                                 one event key moved, removed, copied to another tick, or its sound
                                 event changed (one undo step, through the clip's events view)
  new-clip ROOM CHARACTER (--glb FILE [--anim N] [--fps F] | --from SRCROOM:KEY [--from-game sot|ww|t2t |
           --from-project PATH]) [--entry N] [--rate R] [--scale S] [--no-events] [--no-rest-correction]
           [--name NAME] [--dry-run]
                                 a new clip written into the character's bank: from the glTF file's
                                 animation N (times at F per second; default the clip's rate), or a clip
                                 of another character (an animation or channel set KEY of room SRCROOM,
                                 of this game or another game's project: retargeted by bone name). --entry
                                 replaces that entry (default: a new entry at the end); --rate ticks per
                                 second (60 = 1x); --scale the root motion scale (default: hip heights);
                                 --no-events: the template clip's event keys are not kept.
ROOM: a pack name, its stem (before _wow_) or key. --mod: the mod worked in (default: the active one).
Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## character

```
usage: jadestudio-cli character <project> [--mod MOD] --base KEY --name NAME [--source ROOM]
                                [--texture FILE.png | --texture KEY=FILE.png]... [--stat NAME=VALUE]...
                                [--clone-behaviour] [--animation INDEX=ANIM]... [--place ROOM[@X,Y,Z]]
                                [--spawn [--trigger X,Y,Z [--trigger-size W,D,H] | --on-load] [--no-grid] | --bare]
                                [--no-worlds] [--dry-run]

A variant of base character KEY (an object key, hex) of room ROOM (a pack name, its stem or key; default:
the room holding KEY, an ACT_ world first), named NAME, copied into the --place room (a game room or a zone's,
by pack name or stem; default: the base's room) with new keys for everything it changes, so the original
never changes:
  --texture    a texture of the base (KEY; without one, its largest) gets the picture (PNG)
  --stat       a variable of its own values (and, cloned, of its behaviour's defaults) gets VALUE
  --clone-behaviour  a behaviour of its own, its script functions renumbered (else the original's, shared)
  --animation  entry INDEX of its bank plays animation ANIM (hex; an animation of the base's room with the
               same skeleton; INDEX = the entry count adds an entry)
  @X,Y,Z       where its foot stands (default: the base's place)
An enemy template (inactive, hidden or parked out of the world: what retail spawners spawn, an ACT_ actor) is
placed through a spawner (--spawn, the default for a template): the variant is copied into the room as a
template, and a retail spawner group (spawner, spawn marker, weapons, trigger zone) is copied with it, the
spawner pointed at the variant, its marker at @X,Y,Z (needed), facing the trigger:
  --trigger    the trigger zone's centre (default: the room's checkpoint nearest the point); --trigger-size
               its box (default 4,4,4 m)
  --on-load    no trigger zone: the spawner starts when the room loads (its autoStart)
  --no-grid    no navigation grid added at the marker when none covers it at its height
  --bare       copy the template alone (it never appears: for a spawner of your own)
The worlds it needs (its ACT_ world...) are added to the target level's world list unless --no-worlds.
--mod: the mod worked in (default: the project's active mod, left as it is; naming another makes it the active
one; a mod that does not exist is made, but not by a dry run).
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## enemy

```
usage: jadestudio-cli enemy <project> [--mod MOD] --enemy KEY [--source ROOM] --at ROOM@X,Y,Z [--at ROOM@X,Y,Z]...
                            [--trigger X,Y,Z [--trigger-size W,D,H] | --on-load] [--no-grid] [--no-worlds]
                            [--name NAME] [--family ID] [--dry-run]
       jadestudio-cli enemy <project> --families

--mod: the mod worked in (default: the project's active mod, left as it is; naming another makes it the active
one; a mod that does not exist is made, but not by a dry run).
Enemies of the game placed as the game places them: enemy template KEY (an object key, hex: an ACT_ actor, or a
variant made with `character`) of room ROOM (default: the room holding KEY, an ACT_ world first) appears at each
--at point (ROOM: a game room or a zone's, by pack name, stem or key). No copy of the enemy is made: a retail
spawner group (spawner, a spawn marker per point, its weapons, a trigger zone) is copied into the room, the
spawner names the template, the level lists the template's world (in key order, unless --no-worlds), and a
navigation grid is added under each marker no grid covers at its height (unless --no-grid).
  --enemy      each --at takes the --enemy (and --source) before it: several kinds can share a spawner
  --at         where an enemy appears (its feet). The points of one room share one spawner, up to what it holds
               (5 enemies); more points, or another room, get another spawner
  --trigger    the trigger zone's centre (default: the room's checkpoint nearest the first point): the Prince
               walking in starts the spawner; --trigger-size its box (default 4,4,4 m)
  --on-load    no trigger zone: the spawner starts when the room loads
  --name       the name stem of the new objects (default: the first enemy's name)
  --family     the enemy family whose retail spawner group (and weapons) is copied (default: the family of the
               enemies when they are all of one); an --at with no --enemy before it spawns the family's own enemy
  --families   list the game's enemy families: id, name, enemy, the retail group and its weapons
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## navigation

```
usage: jadestudio-cli navigation <project> [--mod MOD] --room ROOM [--list]
                                 [--generate [--seed X,Y,Z]... [--new-grid | --replace KEY] [--drop-ring]] [--remove KEY]
                                 [--link] [--network [--at X,Y,Z] [--shape loop|chain|both-ways] [--stops N]
                                  [--spacing M] [--radius M] [--grid KEY]] [--dry-run]
                                 [--enemies] [--name-root SPAWNER [--node KEY] [--slot N]] [--move-node KEY --to X,Y,Z]

--mod: the mod worked in (default: the project's active mod, left as it is; naming another makes it the active
one; a mod that does not exist is made, but not by a listing or a dry run).
The enemy navigation grids of room ROOM (a pack name, its stem or its key; a zone's room too):
  --list       its grids (extent, height, walkable samples) and whether a grid covers each checkpoint,
               spawn marker and enemy at its height
  --generate   a grid made from the room's collision, flooded from the --seed points (default: its
               checkpoints, enemies and spawners):
                 (default)       replaces the room's first grid (a room without one gets one)
                 --replace KEY   replaces grid KEY
                 --new-grid      adds a grid next to the others for each seed no grid covers at its height
                                 (without --seed: for every checkpoint, spawn marker and enemy no grid covers;
                                 several on one floor share one grid). Where grids overlap the engine takes
                                 the one nearest in height: one grid per floor level, as retail rooms have.
                 --drop-ring     The Two Thrones: where the floor ends in a drop, a band of code 4 (enemies cross it only
                                 when their search allows drops), as retail's rooftop rooms (510_RoofPart, 410_FireAlley)
  --link       joins the room's grids at different heights where one's floor ends in a drop over another's within
               3 m: code 2 on the lower grid, code 4 on the upper (the engine's grid connections, searches with flag 4)
  --remove KEY takes grid KEY out (its list record, the grid and its cells)
  --network    a patrol network (route) generated on the room's grid: plain network nodes on walkable
               floor, edges only where the line between two nodes stays on walkable floor (corners become nodes),
               the network added to the room's network list; one undo step. --at: where it starts (its root;
               default the widest open spot); --shape: loop (SoT / T2T default), chain, both-ways (WW default: a
               chain walked both ways); --stops (4), --spacing (6: metres apart along the floor), --radius (10 m
               from the start); --grid: the grid walked (default: the one at the start). An enemy walks it once
               its values name the root (WW / T2T destTable; SoT hash_99E6CCB9 or the network).
  --enemies    the room's networks and spawners, and what each slot of a spawner's destination list names
  --name-root SPAWNER  writes a network's root into the spawner's destination list (WW / T2T destTable, SoT
               hash_99E6CCB9), slot --slot (0): the enemy of that slot appears on the root and patrols the network
               (every copy of its values, one undo step). --node: the network (its key, its root or any node;
               default the room's last network): its root is written.
  --move-node KEY --to X,Y,Z  moves a network node (one undo step, its object-json view); edges that leave walkable
               floor at the new place are named (the level view's Move nodes drags them)
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## level

```
usage: jadestudio-cli level <project> [--mod MOD] --level NAME --worlds [--add NAME|KEY]... [--remove NAME|KEY]...
                           [--add-kits]
       jadestudio-cli level <project> [--mod MOD] --level NAME --check [--fix] [--all-objects]
       jadestudio-cli level <project> [--mod MOD] --level NAME --variants [--add-variant KEY]... [--remove-variant KEY]...
       jadestudio-cli level <project> [--mod MOD] --order [edits] | --special-load [--name NAME]   (level --order --help)
       jadestudio-cli level <project> [--mod MOD] --level NAME --loadout [--show | --clear | [--powers all|none|0-9|NAME,...]
                           [--tanks max|N|keep] [--life max|N] [--sword NAME|keep] [--secondary NAME|none] [--dagger yes|no]
                           [--no-fill]]

--worlds: the worlds the level loads (its WOLInfo row, or its directory pack), in load order, the primary
  (its main room) last; technical worlds and move kits marked; the kits its rooms need. --add / --remove
  change the list (a world's pack name such as Prince_ActKitBeam, a kit name such as SwingPole, or a key);
  --add-kits adds the kits it needs. Edits need --mod (made active; created when missing).
--check: the level checks (missing kits, no bank set, shared collision, variables naming objects the level
  never loads, enemy templates placed directly, enemies without navigation, WW traps nothing activates).
  --fix applies every automatic fix (one undo step each) and checks again. --all-objects looks at every
  object, not only those the mod adds or changes.
--loadout (all three games): what the Prince starts the level with (a JS_PlayerStart object in its main room).
  --show prints it; --clear makes it do nothing. Otherwise it is set: values not given keep the level's current
  ones, or (a level without one) all powers, max tanks, max life and the Water Sword. --powers: the ladder level
  (all = 9) or power names (recall, eye-of-the-storm, wind-of-sand, ravages, cyclone-of-sand, breath-of-fate; the
  ladder brings the lower ones along). --tanks: what the powers give, or max (6). --sword: water_sword, lion_sword,
  ... (0-7) or keep. --secondary: axe_01, sword_45... or none. Edits need --mod.
  The Sands of Time: --dagger yes|no (give the Dagger of Time), --powers 0-10 power tanks (at most --tanks),
  --tanks 4-10 or keep, --life 0-10 (+15 each), --sword prince_sword, mighty_warrior, avenging_warrior,
  enlightened_warrior (0-3) or keep; no secondary. Default: dagger, 10 tanks, 10 power tanks, max life, sword 3.
  The Two Thrones: --powers 0-4 or names (recall, eye-of-the-storm, wind-of-sand, breath-of-fate), --tanks 3-6
  or keep, --life 0-6, --sword prince_sword02, dagger_of_time, kings_sword (0-2) or keep (the primary weapon),
  --secondary kings_sword, axe_rack_15, ... or none. Default: all powers, 6 tanks, max life, the Dagger of Time.
--variants: the level's story-variant worlds (0xEE keys: a room variant loaded only while the story's
  condition has its value) and every variant world of the game; --add-variant / --remove-variant KEY change
  the list (one undo step). The Sands of Time and The Two Thrones have none (said so).
Without --mod the workspace's active mod is used as it is; reading (no --add, --remove, --add-kits, --fix)
writes nothing into the project.
Exit codes: 0 done (check: no error), 1 usage or failure, 2 refused or the check found errors.
```

### jadestudio-cli level --order --help

```
usage: jadestudio-cli level <project> --order [--mod MOD]
       jadestudio-cli level <project> --mod MOD --order --insert NAME --key HEX [--at N] [--dry-run]
       jadestudio-cli level <project> --mod MOD --order --remove ROW | --move ROW --to N | --rename ROW --name NAME
                                                | --current ROW [--dry-run]
       jadestudio-cli level <project> --special-load [--mod MOD] [--name NAME]

--order: the level timeline (TimeLine.bin, 090002CC): the story order of the levels, a row {key, name} each
         (Warrior Within: the keys are the levels' TEC_StoryGate objects; its scripts ask the timeline for a
         level's place). ROW is a row's name, key or number. The current row (*) follows its level. One undo
         step each, in the mod. The Two Thrones stores an empty timeline; Sands of Time has none.
--special-load: openjade's Special Load list in its order (the game's own entries, then every level of the
         archive and the mod by name; The Two Thrones keeps its own entries first). --name: where a level of
         that name stands (a zone's place is its name: rename the zone to move it).
```

## traversal

```
usage: jadestudio-cli traversal <project> [--mod MOD] --room ROOM [--all] [--verbose] [--list]
       jadestudio-cli traversal <project> [--mod MOD] --room ROOM --jump X,Y,Z@DX,DY [--wallrun | --walljump | --climbdown]
       jadestudio-cli traversal <project> [--mod MOD] --room ROOM --object KEY
                           [--surface auto|floor|ledge|nograb|wall|wallrun|column|pole|beam] [--grab on|off]
                           [--run on|off] [--dry-run]
The Prince's own move queries (each game's own scripts, ported per game: the running jump, the wall run, the
in-air catch, the beam's end, the wall jump and climbing down) on
the room as the mod has it:
  (no switch) the Traversal check of the objects the mod adds (--all: every object): every border over a
              drop with a floor across is jumped at from 1 m before it; a border without a take-off edge
              (he rolls off it), a gap longer than the 6.5 m jump, a higher far side without a grab edge, a
              column without the catch material, events the scripts never see, walls 3 m apart or less
              where a wall jump stops (a wall that does not hold him). --verbose lists what works too:
              jumps, chimneys (back-and-forth wall jumps), borders he climbs down from.
  --jump      the jump button while running at X,Y,Z (his feet) toward DX,DY: what he does and why;
              with --wallrun the wall-run button instead; --walljump: in a jump at X,Y,Z toward DX,DY he meets
              a wall: does it hold him, and where the kick off it goes; --climbdown: walking over a grab edge
              at X,Y,Z toward DX,DY (TryVault): does he lower himself to hang from it.
  --object    an object's surface (what its collision says); --surface (with --grab / --run) writes its
              collision again for that surface (an object the mod adds: Import geometry's), one undo step.
Exit codes: 0 done (the check found nothing or only information), 1 usage or failure, 2 refused,
3 the check found warnings.
```

## cameras

```
usage: jadestudio-cli cameras <project> [--mod MOD] (--level NAME | --room ROOM [--room ROOM]...) [--filter TEXT]
       jadestudio-cli cameras <project> [--mod MOD] --room ROOM --copy KEY --into ROOM[@X,Y,Z] [--yaw D] [--dry-run]
The retail alternate cameras (CAMALT): the views a game camera switches to with the camera button (its
alternateCamera; T2T alternateCameras), and the cameras named *CAMALT*:
  (list)   every alternate camera of the level's rooms (its technical worlds left out) or of the rooms named:
           key, room, name, where it stands and looks, its look target and the cameras naming it. --filter keeps
           those whose name holds TEXT.
  --copy   copies the alternate camera KEY of --room (with its look target) into the mod's room --into, under new
           keys, where retail has it or at X,Y,Z turned --yaw degrees; one undo step. Name the copy in a camera's
           alternateCamera (Variables editor) to use it.
Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## copy

```
usage: jadestudio-cli copy <project> [--mod MOD] --from ROOM --objects KEY[,KEY...] --into ROOM[@X,Y,Z]
                           [--unique] [--links bring|clear|keep] [--also KEY[,KEY...]] [--clone-behaviour never|always]
                           [--lighting room|source] [--no-worlds] [--dry-run]

Copies objects KEY (hex) of room --from into room --into (a pack name, its stem or key; a zone's room too),
with everything they need, into the workspace's active mod, as one undo step:
  --mod      another mod (created when missing): it becomes the workspace's active mod. Without it, or with
             the active mod's name, nothing outside the mod is written
  --unique   new keys for the objects, their behaviour instances and children (needed for a second copy in
             one room; the game keeps saved state per object key)
  --links    objects their own variables name (a spike tile's button, the button's tiles, a spawner's
             markers): bring (default: copied too, the values naming the copies), clear (the values become
             0) or keep (the values stay: the game takes an object that is not loaded for a live one)
  --also     objects of --from copied with them (objects that name them: a trigger that starts a spawner)
  --clone-behaviour  always: the copy gets a behaviour of its own; never: shared ids (default: when re-keyed)
  --lighting room: the copies use the target room's own ambient light and fog (default when it has a pair);
             source: they keep the source room's lights (brought when the room lacks them, never moved)
  --no-worlds  the worlds their values name (an enemy's ACT_ world) are only noted, not added to the target
             level's world list (default: added with the copy, in its undo step)
  @X,Y,Z     the foot of the chosen objects stands there (linked objects keep their places around them)
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## import-from

```
usage: jadestudio-cli import-from <target-project> --source <other-project> --object KEY [--room SOURCE_ROOM]
                          --into ROOM@X,Y,Z [--yaw DEGREES] [--scale S] [--keep-colors]
                          [--collision source|mesh|box|none] [--name NAME] [--mod MOD] [--dry-run]
       jadestudio-cli import-from <target-project> --source <other-project> --list-props [--search Q] [--count N]
       jadestudio-cli import-from <target-project> --source <other-project> --texture KEY|ID [--as-material NAME]
                          [--apply OBJECT --into ROOM [--slot N] [--auto-uv M] [--subdivide M]] [--png FILE] [--mod MOD]
       jadestudio-cli import-from <target-project> --source <other-project> --list-surfaces | --survey-materials [--count N]

Brings assets of another Prince of Persia game (Sands of Time, Warrior Within, The Two Thrones: any of the
six directions) into the target project's active mod. The source project is only read, never written.
  --object       a static object of the source (a mesh, its materials' textures and its collision; no
                 animation, behaviour or skin): imported into ROOM (target pack name, stem or key; a zone's
                 room too) with its origin at X,Y,Z, turned --yaw degrees about the vertical, scaled by --scale,
                 as one undo step. --keep-colors keeps its baked vertex colours (else the quick ambient;
                 Light the room bakes it for its new room either way). --collision: its own (default), from
                 its mesh, a box, or none. --room picks the source room when the key is in several.
  --list-props   the source's static props, one per mesh, the most placed first, with sizes (cached in the
                 target's jadestudio/cache/other-games/).
  --texture      a surface of the source: a material or texture key, or a curated id (--list-surfaces).
                 --as-material NAME names it in the mod: import-geometry --material NODE=NAME and Apply
                 game material then take NAME (as they take sot:KEY / ww:KEY / t2t:KEY directly).
                 --apply OBJECT puts it on a static object of ROOM (Apply game material, one undo step).
                 --png FILE writes the decoded picture.
  --survey-materials  the source's plain materials by the area of static rooms they cover.
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## blender

```
usage: jadestudio-cli blender <project> --status [--session ID]
       jadestudio-cli blender <project> [--mod MOD] --start --key MESH [--room ROOM] [--launch] [--blender EXE]
       jadestudio-cli blender <project> [--mod MOD] --push [--session ID]
       jadestudio-cli blender <project> [--mod MOD] --pull [--session ID] [--send N] [--dry-run]
       jadestudio-cli blender <project> --stop [--session ID] [--close-blender] [--remove]

Edit in Blender's live session, as the Studio's Blender panel runs it: the session folder under
<project>/jadestudio/blender/ (model.glb, the slots' pictures, session.json) that Blender opens with the add-on
in tools/jadestudio/blender (loaded from there, not installed), and the sends Blender writes back into it
(Send to Jade Studio). A session started here shows in the Studio's Blender panel and the other way round.
  --status   every session (or one): its model, the newest send, what is in the mod, whether its Blender runs
  --start    writes a new session for mesh MESH (a key with a mesh-glb view; --room picks the view of one room)
             and, with --launch, starts Blender on it (--blender EXE, else JADESTUDIO_BLENDER, else the newest
             installed Blender); earlier sends of that mesh's session are dropped
  --push     writes the session's model and pictures again from the mesh as the mod has it now, keeping its
             sends (the panel's Write again); Blender: open the session again, or Reload textures
  --pull     takes a send into the mod (the newest, or --send N) as one undo step: the .glb imported through
             mesh-glb as the cook will (material slots by their slot_<n> names), recorded in studio.json
  --stop     ends the session: --close-blender closes the Blender started for it (its unsaved work is lost:
             send first), --remove deletes the session folder
  --session  a session's folder name (or its start); default: the newest
Exit codes: 0 done (or listed / planned), 1 usage or failure, 2 refused.
```

## swap

```
usage: jadestudio-cli swap <project> [--mod MOD] texture|model|animation --key KEY (--file FILE | --export FILE)
                          [--room ROOM] [--match-size] [--dry-run]
Swap a texture (a picture: PNG, TGA, BMP, JPEG, DDS; any size, DXT sides round up to multiples of 4; --match-size
resizes it to the texture's size), a model or an animation (a .glb) as the wizards do: converted into the asset's
view as the cook will import it, checked, written into the mod (one undo step; the view stands for every room it
names, --room picks the view of one room). --export writes the asset as the mod has it (a texture as .png or its
own format; a model as .glb): Change a character's look = swap its textures; Edit in Blender = --export, edit in
Blender, --file. Exit codes: 0 done (or planned), 1 usage or failure, 2 refused.
```

## texture-pack

```
usage: jadestudio-cli texture-pack <project> [--mod MOD] (--export DIR | --import DIR) [--room ROOM] [--match-size] [--dry-run]
Texture packs, as the Use a texture pack / Export textures tasks: --export writes every texture of ROOM (else of the
game) as <KEY>.png; --import swaps every texture a <KEY>.png / .dds / .tga / .bmp / .jpg of DIR names (any size),
all in one undo step. Exit codes: 0 done, 1 usage or failure, 2 nothing taken.
```

## rebind

```
usage: jadestudio-cli rebind <project> [--mod MOD] --room ROOM --object KEY --behaviour MODEL [--dry-run]
Points a placed object at another behaviour of its room, as the Inspector's Rebind does: its own values are kept
where the new behaviour has variables of those names. One undo step. Exit codes: 0, 1, 2 refused.
```

## variables

```
usage: jadestudio-cli variables <project> [--mod MOD] --room ROOM --objects KEY[,KEY...] (--list | --set NAME=VALUE...) [--dry-run]
The objects' own values, as Tweak a character's stats and the Variables editor's bulk edit set them: NAME as the
behaviour's variables view names it (or its hash in hex), VALUE a number or true / false (int, float and bool
variables). Every object of the list in one undo step. Exit codes: 0, 1, 2 refused.
```

## nav-paint

```
usage: jadestudio-cli nav-paint <project> [--mod MOD] --room ROOM --box X0,Y0,X1,Y1 --code walkable|blocked
                              [--grid KEY] [--z Z] [--dry-run]
Paints the enemy navigation grid as the level view's paint brush does: every sample whose square lies in the
box (world metres) becomes walkable or blocked. The grid: --grid, else the one under the box's centre nearest
in height to --z (default: the first). One undo step. Exit codes: 0, 1, 2 refused.
```

## import-behaviour

```
usage: jadestudio-cli import-behaviour <project> [--mod MOD] --from sot|ww|t2t --list ROOM
       jadestudio-cli import-behaviour <project> [--mod MOD] --from sot|ww|t2t --source ROOM:OBJECT --to ROOM:OBJECT[,...]
                                       [--dry-run]
Carries the behaviour (AI model, variables, scripts, tables) of OBJECT of another game's ROOM onto placed objects of
this project, as Prefabs > Other games > Behaviours does. Every builtin its scripts call is checked against this
game's builtin catalog (tools/bf/data/builtins-<game>.json): the same handler at the same id, or the same name with
the same stack traffic (translated); anything else, calls to other behaviours' functions and keys of the other
game the scripts name refuse it, listed. The other game's project: Settings or JADESTUDIO_<GAME>_PROJECT.
Exit codes: 0 done (or listed / planned), 1 usage or failure, 2 refused.
```

## import-character

```
usage: jadestudio-cli import-character <target-project> --source <other-project> --list [--search Q]
       jadestudio-cli import-character <target-project> --source <other-project> --character KEY [--room SOURCE_ROOM]
                          (--bases | --into ROOM[@X,Y,Z] [--base FAMILY|ROOM:KEY] [--clips N|all|I,J,..]
                          [--name NAME] [--carry-behaviour] [--mod MOD] [--dry-run])

Brings a whole character of another Prince of Persia game into ROOM of the target (a game room or a zone's
room): a variant of this game's closest enemy (its skeleton objects, AI and behaviour, cloned) wearing the
source's skinned meshes refitted onto that skeleton by bone name, the source's textures over the base's,
and clips of the source's bank retargeted onto it as new entries. One undo step.
  --list       the source's characters (its ACT_ worlds), with why one cannot come over (cached in the
               target's jadestudio/cache/other-games/).
  --bases      this game's enemies ranked as bases for the character (bones matched by name, hip height).
  --base       the base: an enemy family id (default: the closest) or ROOM:KEY of this project.
  --clips      the source bank entries carried: the first N with a clip (default 8), all, or a list.
  @X,Y,Z       where it stands (default: where the base stands).
  --carry-behaviour  the source's own behaviour instead of the base's clone, when it carries (every builtin its
               scripts call has a counterpart here: import-behaviour checks the same); else the base's clone
               stays and the refused calls are listed.
--dry-run: plan and print only. Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## particles

```
usage: jadestudio-cli particles <project> [ROOM:]KEY [--mod MOD] [--show] [--set FIELD=VALUE]... [--preview SECONDS]
       [--write-view FILE] [--dry-run]

A particle generator (class resource type 11; KEY: the generator's key, or a particle object's: its visual's
generator), as the particle editor shows it: every field by group with what the engine does with it (--show),
the numbers as the game uses them, and an approximate simulation (--preview: particles alive and how far they
reach after SECONDS). --set FIELD=VALUE changes a field (its name from --show: emissionRate=30, colourPrimaryStart=
#FF8040FF, birthShape=sphere) or a named flag (fadeOut=true); the generator is checked through bfcook's
particles-json converter and written into the mod (its view added when the mod has none), one undo step.
--write-view FILE saves the view (for create --what particle-effect --set generator=@FILE).
Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## effects

```
usage: jadestudio-cli effects <project> [--mod MOD] --post-effect [ROOM:]KEY [--set KEY=VALUE]... [--dry-run]
       jadestudio-cli effects <project> --other sot|ww|t2t [--search TEXT]
       jadestudio-cli effects <project> [--mod MOD] --import GAME:KEY [--slot N] --room ROOM --at X,Y,Z [--yaw D] [--set FIELD=VALUE]...
                                   [--dry-run]

--post-effect: an object's post-effect controllers (39 volume, 40 boundary, 41 distance; WW and T2T) described, and
changed as the Inspector's form changes them: tint=R,G,B (0..1) or none, scale=R,G,B,S or none, blur=N (0: none),
brightness=F or none, near=F far=F (distance); a boundary: axis=x|y|z, fadein=F, fadeout=F, hold=F, threshold=F.
Written through the object's object-json view (added when the mod has none), one undo step.
--other: the particle objects, lights, fogs and LOD selectors of another game's project (JADESTUDIO_SOT/WW/T2T_PROJECT
or the Studio's Settings), one per name. --import: one of them made in a room of this game by this game's own creator
(New particle effect with its generator carried over and its sprite painted over the look's; a light or fog
creator with its colour, range, cone, density; a LOD selector with its switch sizes, over this game's object and
meshes: --set object=KEY --set meshes=KEY,KEY, no --at), one undo step. --set fills any other field of the creator.
Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## object

```
usage: jadestudio-cli object <project> [--mod MOD] --move KEY[,KEY...] --to ROOM@X,Y,Z [--yaw DEGREES] [--dry-run]

Moves objects of room ROOM (a pack name, its stem or key; a zone's room too) as one: the first object's origin to
X,Y,Z, turned DEGREES about the vertical through it, the others keeping their places relative to it. Every object is
written through its object-json view in the active mod, as the level view's gizmo writes a move (the view added when
the mod has none), its stored bounds turned with it; one undo step. A placed Level kit item moves with kit --move.
Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

### jadestudio-cli object --structure --help

```
usage: jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --structure
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --add PART --from DONOR [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --remove PART [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --add-controller --from DONOR[:N] [--at I]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --add-controller --type T [--flags HEX]
                                       --fields JSON [--at I]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --remove-controller I [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --donors PART|controller
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --rename NAME [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --delete KEY[,KEY...] [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --reparent PARENT|none [--dry-run]
       jadestudio-cli object --parts

An object's structure, as the Inspector's Structure component edits it: parts (sections) and controllers added or
removed, so an object can gain animation, a behaviour (AI), collision or a controller it did not have. A part's
values are copied from DONOR, an object of the same room that has it (its keys then name resources the room
loads; copy an object into the room first to use its parts). Written through the object's object-json view (added
when the mod has none) with "structure": "edit", checked as the cook imports it; one undo step of the mod.
  --structure  the object's parts and controller chain
  --donors     the room's objects that have PART (controller: their typed controller nodes, DONOR:N)
  --at I       where the new node goes in the chain (default: the end, before a node kept as raw bytes)
  --rename     the object's name (".gao" kept), as the Inspector renames it
  --delete     the objects leave the room's object list (its objlist-json view), as the level view's Delete;
               other objects still naming them are listed
  --reparent   the object goes under PARENT (an object key of the same room; none: no parent) and keeps its
               place in the world, as the Outliner's drop and the Inspector's Parent do: its parent link, pair
               matrix and header matrix, the matrices worked out from the room's objects. Refused for an object
               stored without a parent section, a loop, a bone of a skeleton, or a parent the room reads after it
Exit codes: 0 written (or listed / planned), 1 usage or failure, 2 refused.
```

### jadestudio-cli object --collision --help

```
usage: jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --collision
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --collision --shape I
                                       [--move X,Y,Z] [--scale X,Y,Z] [--turn DEGREES] [--dry-run]
       jadestudio-cli object <project> [--mod MOD] --room ROOM --object KEY --collision --paint --at X,Y,Z
                                       --radius R --material TAG [--shape I] [--dry-run]

An object's collision, as the level view's Collision tools edit it (one undo step each, written through the
resource's collision-json / collision-glb view and checked as the cook imports it). Object-local metres, +Z up.
  --collision  alone: the object's collision shapes (index, kind, size, view) and a mesh's game materials
  --shape I    shape I moved by --move, scaled about its centre by --scale, turned about Z by --turn. A box
               stays aligned with the object (quarter turns only), a sphere scales evenly, a cylinder stands on Z
  --paint      the faces of the object's collision mesh (or mesh I) within --radius of --at get game material TAG
               (a key of the mesh's material table, listed by --collision)
Exit codes: 0 written (or listed / planned), 1 usage or failure, 2 refused.
```

## view

```
usage: jadestudio-cli view <project> [--mod MOD] <view file> [--get [POINTER]] [--set POINTER=JSON]... [--remove POINTER]...
                                     [--dry-run]
       jadestudio-cli view <project> [--mod MOD] KEY --converter C ...   (the first C view of resource KEY)

A view's fields, as the editors' forms show and set them (material, light / fog / LOD resource, collision shapes,
game-material table, any JSON view): --get prints the view (or the field at POINTER, e.g. /levels/0/uv); --set writes
a field (JSON value: 1.5, true, "0x0000FFFF", [1,2,3]; a pointer ending in /- appends to a list), --remove takes a
key or list item out. The result is imported through the view's converter as the cook will (refused with its
words), then written into the mod: one undo step. A view standing for several bins changes in all of them.
Exit codes: 0 done (or read / planned), 1 usage or failure, 2 refused.
```

## raw

```
usage: jadestudio-cli raw <project> [--mod MOD] --pack PACK --list
       jadestudio-cli raw <project> [--mod MOD] --pack PACK --block KEY[:N]|#I --extract FILE
       jadestudio-cli raw <project> [--mod MOD] --pack PACK --block KEY[:N]|#I --override FILE [--dry-run]
       jadestudio-cli raw <project> [--mod MOD] --pack PACK --block KEY[:N]|#I --revert [--dry-run]

The Raw editor's Export, Override and Revert: a block of a pack (a room's .wow pack name or stem, any pack's name
or key) as the mod has it, written to FILE; replaced by FILE's bytes in the active mod (the pack's pack.json and the
raw file go into the mod's overlay, as bfcook's Extract / Override does in a project); or put back. KEY:N is the
Nth block with that key, #I the pack's block I. The cook takes overridden bytes as they are. Refused: a block whose
view the mod edits (revert the view first), a pack the mod adds. One undo step each.
Exit codes: 0 done (or planned), 1 usage or failure, 2 refused.
```

## create

```
usage: jadestudio-cli create <project> --list [--category CATEGORY] [--all]
       jadestudio-cli create [<project>] --describe ID
       jadestudio-cli create <project> [--mod MOD] --what ID [--into ROOM@X,Y,Z [--local]] [--yaw DEGREES]
                             [--name NAME] [--set KEY=VALUE]... [--dry-run]

Makes a new, clean object or resource from a creator: its own values reset to its behaviour's defaults, nothing
linked to other objects, new keys (Create... in the Studio).
  --list       the creators of the project's game (--all: also those it refuses, with why); --category narrows it
  --describe   a creator's settings (--set keys, their kinds and defaults)
  --what       the creator id (from --list)
  --into       placed creators: the room (pack name, stem or key; a zone's room too) and the point (world metres;
               --local: relative to the zone's origin)
  --yaw        degrees about the vertical
  --name       the new object's or resource's name
  --set        a setting (repeat): numbers, X,Y,Z, R,G,B (0..1), true/false, keys (hex), choices, text
--mod: the mod worked in (default: the active one; a mod that does not exist is made, but not by a dry run).
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## kit

```
usage: jadestudio-cli kit <project> --list
       jadestudio-cli kit <project> [--mod MOD] --list-placed | --move ID --to ROOM@X,Y,Z [--yaw D] [--dry-run]
       jadestudio-cli kit <project> [--mod MOD] --place NAME --at ROOM@X,Y,Z [--yaw DEGREES] [--start zone|load]
                          [--range START,END] [--colour R,G,B]
                          [--facing +x|-x|+y|-y] [--wall +x|-x|+y|-y] [--approach M] [--retail-zones]
                          [--no-worlds] [--local] [--dry-run]

The Level kit: every parkour object, trap, mechanism (cranks, plates, lifts, bridges, switches), pickup (fountains,
save points, life upgrades, sand), light and camera of the game (kit --list; SoT, WW and T2T), placed so that it
works (its retail group copied with
new keys, values naming objects it leaves behind cleared, the trigger zones that start a trap brought with it,
the move kits the level needs added to its world list), into the workspace's active mod, as one undo step.
  --list     every item: id, name, category, what it shows
  --list-placed  the items this mod placed (groups the level view moves together): id, item, room, objects
  --move ID|NAME|KEY --to ROOM@X,Y,Z [--yaw D]  moves a placed item as one (every object of it, through its
             object-json view as the level view's gizmo writes it), its main object's origin offset kept; --yaw
             turns it to D degrees; one undo step
  --place    an item by id or name ("ww-swing-bar", "Swing bar"); --item ID works too
  --at       the room (pack name, stem or key; a zone's room too) and where the item's foot stands
  --yaw      turns the item about the vertical (degrees); traps turn to the corridor by themselves (below)
  --start    traps: zone (default: a trigger zone over the corridor around it starts it) or load (it runs
             from the level's load, no zone)
  --facing   traps: the way the Prince runs past it: +x, -x, +y or -y (default: the corridor's axis at the
             point, from the room's walls); blades, pillars and rollers turn across it, a spike line along it,
             a plate goes on the path before its spike tiles
  --wall     wall traps (wall saw, wall blades): the wall it goes against: +x, -x, +y, -y (default: the
             nearest); it stands as far from the wall as retail has it
  --approach traps: metres of the path before and after it its trigger zone covers (default 5); the zone spans
             the corridor from wall to wall. --retail-zones keeps retail's zones at their retail places
  traps stand as high above the floor at the point as retail has them (the point's z: the floor)
  --range    Lighting items: the light's range in metres (default per item, e.g. 1.5,9 for a wall torch)
  --colour   Lighting items: the light's colour, 0..1 per channel (default a warm orange); the light is an
             enabled omni of the main bank (it lights the Prince and enemies); bake the room's static
             geometry afterwards: jadestudio-cli light <project> --room ROOM
  --local    --at / --to are relative to the zone's origin (zone --move records it), as the Level kit's
             "From the zone's origin"; else world coordinates
  --no-worlds  the kits it needs are only noted, not added to the level's list
  --mod      another mod (created when missing): it becomes the workspace's active mod
Interactive objects (levers, doors, fountains...) take their own options: jadestudio-cli kit --help-interactive.
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

### jadestudio-cli kit --help-interactive

```
usage: jadestudio-cli kit <project> [--mod MOD] --item ID --at ROOM@X,Y,Z [--with X,Y,Z] [--yaw DEGREES]
                          [--hold SECONDS] [--delay SECONDS] [--name NAME] [--local] [--no-worlds] [--dry-run]
       jadestudio-cli kit <project> --list-interactive

Places an interactive object of the Level kit (Warrior Within) in room ROOM (a game room or a zone's, by pack
name, stem or key) so that it works: its retail group copied with new keys, the worlds and kits the level
needs added to its list (unless --no-worlds), the opener wired to its door by the behaviour templates.
  --item   lever-gate, plate-timed-gate, push-block, save-fountain, health-fountain, sand-vase,
           breakable-barrel, weapon-rack, artwork-chest, breakable-wall (--list-interactive says more)
  --at     where the main part stands (the lever, the plate, the fountain...)
  --with   where the partner stands (the gate of lever-gate / plate-timed-gate; default: beside the main
           part, where retail has it)
  --yaw    turns every part about the vertical (degrees)
  --hold   seconds the gate stays open once let go (plate-timed-gate: 2); --delay seconds before it opens
  --name   the name stem of the new objects (default KIT_<item>)
  --local  --at / --with are relative to the zone's origin (zone --move records it); else world coordinates
--dry-run: plan and print only. Exit codes: 0 written (or planned), 1 usage or failure, 2 refused.
```

## ui

```
usage: jadestudio-cli ui <project> [--mod MOD] <what> ...
  --fonts                                the game's fonts (FONTDESC), their atlases, and the fonts that are not data
  --font KEY [--glyph CODE [--rect U0,V0,U1,V1] [--spacing X,A]] [--from-ttf FILE [--size PX]] [--atlas OUT.png]
                                         a font's glyphs (listed), one glyph's rectangle / spacing, or the whole font
                                         redrawn from a TrueType file at a line height (default: the font's own);
                                         --atlas writes the atlas picture as the mod has it
  --pictures                             the menu pages' and the HUD's pictures (texture keys, views)
  --picture KEY --with FILE.png          a menu / HUD picture replaced (fitted to the texture's size)
  --captions [--language L] [--languages DIR] [--set N=TEXT | --revert N]
                                         save captions (SaveNames.Checkpoint.<n>, WW Chapter.<n>: N is 12 or Chapter.3)
  --special-load [--name LABEL=NEW | --hide LABEL | --restore LABEL]
                                         Special Load entries renamed / hidden through the menu text (needs an openjade
                                         that looks up SpecialLoad.<label> menu texts)
  --not-data                             menus, loading screens, unlocks, story flags: why they are not mod data
Writes are one undo step each. Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## menus

```
usage: jadestudio-cli menus <project> [--mod MOD] <what>
  --list                                    the mod's data for openjade's menus
  --font ROLE FILE.ttf [--size S]           a font for menu | tutorial | subtitles (size 0.25-4)
  --font-remove ROLE                        back to the player's / the game's font
  --loading FILE.png [--world KEY]          a loading picture: of one level's world list, else shown in turn
  --loading-remove (FILE.png | --world KEY) takes one out
  --special-load LABEL --world KEY          a Special Load entry of the mod starting that world list
  --special-load-remove LABEL
  --menu-entry main|extras map|page|url TARGET --label TEXT   a button (map: a Special Load label; page:
                                            special_load | extras | options; url: http(s)://...)
  --menu-entry-remove main|extras map|page|url TARGET --label TEXT
Needs an openjade that reads a mod's menu data (Mods/<game>/*.json). Writes are one undo step each.
Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## live

```
usage: jadestudio-cli live [--port N | --staging DIR | --game sot|ww|t2t] <command> [arguments]
The game: --port, else the live-link.txt that Play (the Play bar, or jadestudio-cli play --live-link) wrote into
the play-test folder (--staging, else %LOCALAPPDATA%/Jade Studio/playtest/<--game, default ww>).
  hello | state                    the game, paused / stopped, the Prince's place
  pause | resume                   the debug pause (the free camera while paused)
  teleport X Y Z [YAW]             the Prince (engine units, +Z up; yaw degrees from +X toward +Y)
  camera                           the drawn camera: position and forward
  camera-set X Y Z FX FY FZ        pause on the free camera there, looking along F
  selected | select KEY | object KEY   the in-game object inspector's pick; pick an object; an object's place
  get OBJECT VAR | set OBJECT VAR VALUE   a script variable (VAR: a name or 0x hash; vec3 as x,y,z)
  console "COMMAND"                a console command
  break FUNCTION [NODE] | unbreak FUNCTION [NODE] | breaks | break-clear   script breakpoints (name or id)
  lines <project> SCRIPT           the script's text (as the game runs it with the mod) with line numbers; '*' marks
                                   the lines a breakpoint can stop at (statements inside functions)
  break-line <project> SCRIPT LINE | unbreak-line <project> SCRIPT LINE   a breakpoint on a source line of 'lines'
  locals [LEVEL] [--project P]     at a breakpoint: the call's arguments and locals with their values (LEVEL: 0 the
                                   outermost call; default the innermost); --project names them from the script
  wait-stop [SECONDS] [--project P]   wait until the scripts stop, then print where (with --project: the
                                   script and its source line) and the stack
  continue | step | stack          at a breakpoint
  unlocks | unlock BIT | credits N the profile's unlock bits and sand credits
  events [SECONDS]                 print what the game says by itself (picks, stops) for a while
  reload <project> [play options]  hot reload: close the game, build the mod again and start it at the same
                                   level with the Prince where he stood (jadestudio-cli play ... --spawn X Y Z --live-link);
                                   openjade reads the mod's files once, at start, so a changed room is not swapped in
                                   while it runs
  fake [--port N] [--game G] [--seconds S]   a fake game answering the live link (tests; never the game)
Exit codes: 0 done, 1 usage or no game, 2 the game refused (its reason is printed).
```

## level-wide

```
usage: jadestudio-cli level-wide <project> [--mod MOD] --level LEVEL ...
  --mood PRESET|custom [--light R,G,B] [--fog R,G,B] [--fog-amount 0..1]
                         every light of the level's own rooms times the tint, every fog toward the colour
  --moods                the presets
  --roster FROM:TO       every spawner's enemies of family FROM become family TO (--enemy-families lists them)
  --enemy-families       the game's families, and those the level spawns
  --difficulty --health X --damage Y [--families a,b]
                         enemy templates' health and damage scaled (the level's families, --families, or all
                         without --level); game-wide for those enemies. The Two Thrones: --damage in whole
                         steps (1..4); its guards, sand guards, thralls, archers and the Brute keep theirs
  [--dry-run]            the plan only
Each is one undo step. Exit codes: 0 done, 1 usage or failure, 2 refused.
```

## simple

```
usage: jadestudio-cli simple <project> [--mod MOD] <task> ...   (the Simple-mode one-page tasks)
  sound BANK EVENT|r:ID FILE [--vorbis]     Replace a sound (sounds BANK [--find Q] lists a bank's events)
  video TARGET FILE                         Replace a video (CINE0010.bik, ARTWORKS/CHAR/PRINCE/CP01.BIK ...)
  text KEY TEXT [--tutorial] [--language L] Change a text (menu text; --tutorial: a tutorial hint)
  text-revert KEY [--tutorial]              the game's text again
  texts QUERY [--languages DIR]             menu texts and hints whose key or text holds QUERY
Each change is one undo step with a Changes row.
```

## cutscene

```
usage: jadestudio-cli cutscene <project> [--mod MOD] --room ROOM (--shot SHOT | --path KEYS [--ease E])...
                               (--line TEXT|SAMPLE|EVENT | --event HEX) [--line-start SECONDS]
                               [--on-start | --trigger X,Y,Z [--trigger-size W,D,H]] [--free-player]
                               [--look-at-player DX,DY,DZ] [--prince-offset DX,DY,DZ]
                               [--fast-forward seen|always|never] [--fov DEGREES] [--name NAME]
                               [--no-bank] [--no-worlds] [--subtitles FILE] [--print-path] [--dry-run]
       jadestudio-cli cutscene <project> [--mod MOD] --room ROOM --edit NAME|KEY [any option above] [--dry-run]
       jadestudio-cli cutscene <project> [--mod MOD] --room ROOM --show NAME|KEY [--print-path]
       jadestudio-cli cutscene <project> --lines [--search WORDS] [--subtitles FILE]
       jadestudio-cli cutscene <project> [--mod MOD] --list --room ROOM
       jadestudio-cli cutscene <project> [--mod MOD] --remove NAME|KEY --room ROOM [--dry-run]
       jadestudio-cli cutscene <project> --clips [--room ROOM]   the actors' clips (retail cinematics; with --room the
                               mod's new clips of that room's characters too)
       jadestudio-cli cutscene <project> --videos                the videos a video trigger can play
       jadestudio-cli cutscene <project> [--mod MOD] --room ROOM --video CODE [--vision] --trigger X,Y,Z
                               [--trigger-size W,D,H] [--name NAME] [--with FILE] [--dry-run]
       jadestudio-cli cutscene <project> [--mod MOD] --room ROOM --remove-video NAME|KEY [--dry-run]

An in-engine cutscene in which the Prince says a line: a copy of the game's own camera cut (camera, manager,
trigger box: WW 615_Forge_Pres_CAMCUT, SoT 1401_Cour_CAMCUT_Pres, T2T SE410_130_CAM). Its camera plays the
parts in the order given, with a cut between them (up to 8):
  --shot X,Y,Z@LX,LY,LZ[:S]   a still shot: the camera at X,Y,Z looking at LX,LY,LZ for S seconds (default 3)
  --path "X,Y,Z@LX,LY,LZ:T; X,Y,Z@prince:T; ..."  a camera glide through 2 to 64 keys on a smooth spline: each
               key's T is the time to the next key (the last key's: how long it holds at the end; default 2);
               a key looks at a point or at the Prince (@prince, or no @ at all)
  --ease       inout (default), in, out or linear: the timing of the --path just before it (given before any
               --path: every path's)
  --look-at-player DX,DY,DZ  every key of every path looks at the Prince, DX,DY,DZ above his feet
  --prince-offset DX,DY,DZ   where @prince keys look: this far above his feet (default 0,0,1.5). He stands
               at the level's start checkpoint (--on-start) or on the trigger box's floor, locked while it plays
The line's voice plays LINE-START seconds in (default 0.5; its subtitle shows while it plays) and the Prince is
locked (unless --free-player). The last part is lengthened so the line is said before the cut ends.
  --room       a game room or a zone's, by pack name, stem or key
  --line       a Prince line of the subtitles by its sample or event id (hex) or its exact text (see --lines)
  --event      any play event of the game's sound banks (hex)
  --on-start   (default) a trigger box at the level's start checkpoint: the Prince spawns in it, the cut plays
               when the level starts
  --trigger    the trigger box's centre instead (its floor 0.5 m below); --trigger-size its box (default 4,4,4 m)
  --fast-forward  seen (default, the game's rule: jump fast-forwards a cut seen before), always, never
  --fov        the camera's field of view (default the game's, 84 degrees)
  --name       the objects' name stem (default Intro)
  --no-bank    do not add the line's sound bank to the level's bank set (the line is silent without it)
  --print-path the camera every 0.1 s as the game will play it (time, position, the point it looks at)
  --edit       change a cutscene the mod added (its saved keys, line and options; any option given replaces
               that part; --shot/--path replace all the parts), one undo step, the same keys
  --show       a cutscene's parts and options as the options that make it again
  --lines      the Prince's lines with their events, banks and lengths (--search: words of the text)
  --list       the cutscenes of a room; --remove takes one the mod added out again (one undo step)
Dialogue and actors:
  --subtitle TEXT, --speaker NAME  the first line's subtitle written into the mod's English subtitles
  --then LINE@SECONDS   a later line by any speaker (sample or event id in hex, or its exact text; --lines
               --speaker all lists them), said SECONDS into the cut (its own voice key); repeat for more (up to 8);
               --then-subtitle TEXT / --then-speaker NAME give the last --then its subtitle
  --actor CLIP@X,Y,Z[:YAW]  an actor playing a cinematic's clip (CLIP: ROOM:KIT, or ROOM:KIT#ENTRY for a new clip the
               mod made on a character, see --clips), starting at X,Y,Z facing YAW degrees; --actor-no-head: the
               Prince's head (T2T: his face) keeps still. Up to 6. T2T: through a copy of 100_Quai's scripted event
  --actor-start SECONDS  (after an --actor) it starts that late: its clip held on its first pose until then
  --actor-end X,Y,Z[:YAW]  (after an --actor) where it stands when the cut ends (an end marker)
  --no-actors  (with --edit) the cutscene's actors taken out; --actor with --edit replaces them (one undo step)
Video triggers: the game's video player (WW 115_LowerGarden_TEC_VideoPlayer, SoT 1501_ReceptionHall_
VideoPlayer_Vision151, T2T 100_Quai_Video_P3C_010) and a trigger box: walking in plays video CODE (WW CINE/
CINE%04d.bik, T2T CINE/P3C_%03d.bik, SoT Cine_%03d.int; --vision: SoT Vis_%03d.int). --with FILE replaces that
video in the mod (the Videos panel's replacement) in the same undo step. --list shows a room's video triggers.
--mod: the mod worked in (default: the active mod). Exit codes: 0 written (or planned), 1 usage or failure,
2 refused.
```
