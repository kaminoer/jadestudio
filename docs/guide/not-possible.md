# What is not possible, and why

The Studio refuses these in words. Each has a reason in the games' data or in openjade; none is a missing button.

## Levels and story

- **A story-variant world of your own (WW).** A variant (an 0xEE world) is picked by the story script's condition;
  no script sets a condition your mod could own, so new variant keys are not made. Retail variants can be added or
  removed.
- **Farah co-op (SoT).** The Farah levers take their event only from the area managers, by hard-coded ids and story
  case bodies; no data-only route sets it. The snipe, hole, teleport and goal-zone items are the data route.
- **T2T Farah as a companion.** Her T2T object is a cinematic body with no follow or shoot behaviour.
- **SoT navigation path costs (0x10 / 0x18).** No rule in the room's collision explains where retail puts them.
- **A Special Load entry starting at a chosen checkpoint**, **new menu pages**: openjade's own C++ and the frontend
  script's slot table.
- **T2T upgrade statues other than 810's.** Create > Story set offers 810_LU_STATUE's statue with its sounds; the
  Level kit's statue item copies from one room, so its sound trigger is cleared there.
- **A dream portal back into your zone (SoT).** Its way back names retail's room.

## Assets

- **LOD sets** (type 4): openjade has no draw callback for them.
- **Another game's LOD selector with its meshes:** a selector chooses between meshes of its own room, so its switch
  sizes come over (Other games > Effects) and the meshes are this game's.
- **Texture format 12 and types 4 / 5:** no game uses them. **Animated textures on WW / T2T:** no retail one.
- **WAV to Vorbis (WW):** there is no Vorbis encoder in the tree (openjade's FFmpeg decodes only). An `.ogg` is kept.
- **Chained or switched sound events, and choices of choices,** on import from another game: refused in words (a
  random pick or a sequence over plain samples is kept); import them with each sample as a sound of its own.
- **Multitrack sound events** from another game.
- **Object-driving animation channels** are shown, not edited; **a new event channel** for a clip that has none.
- **Post-effect blocks of an existing volume:** the blocks are fixed in its view (the converter refuses a mask change);
  the form edits their values. Choose the blocks when you make a volume.
- **Collision boxes turned by other than quarter turns:** a box is stored as corners.
- **Fonts outside the archive** (SoT POPData.bf, T2T POP3_54.MFT) and openjade's TrueType settings: openjade reads
  them, not the game archive. openjade's mod data can add a TrueType font for a role instead.
- **HUD layout:** it is GameInterface scripts (use the script editor).

## Behaviours and characters

- **Calls into other behaviours' functions across games:** function ids are minted per game (WW 5083 GetLife is
  SoT's Farah_5083).
- **The source character's own behaviour** when it calls such functions (all the retail characters tried do): the
  base's clone runs, the refused calls are listed.
- **Enemy difficulty for some T2T enemies:** the sand guards, dark warriors, guards, sand gate guards, thralls and
  archers take their health and the damage of their blows from numbers in their behaviour's script, the Brute's
  script sets its health itself, and where the Illusion's blows take their damage is not known: these are left as
  they are, and the plan says so. T2T damage goes up in whole steps (x1 to x4).

## Live link

- **Swapping a changed room into the running level:** openjade reads the mod's files once, when it starts, and keeps
  a room that stays in the level loaded (Hot reload restarts the game).
- **A breakpoint on a line of a script's main part:** openjade stops only inside called functions.
- **Story flags as a table:** there is none; they are script variables (watch and set them live).

## Kept as retail on purpose

- A boss arena leaves out WW's GP710 story manager, SoT's sand vision and monster-room portal, and the T2T Vizier's
  teleport; a change of level after a fight is always left out.
- Throw targets for bosses: no boss has such data in the trilogy.
- Power altars (WW): none exist in the data; the powers come with the story (the player start sets them; the time
  portal's fountain grants them).

## Also not built

- **Throw targets, the rotating claw as its own item** (it is the blade pillars, roller and axe pillars), and
  **chain walls started by a cinematic** (640_Courtyard01's; the standalone one is in the kit).
