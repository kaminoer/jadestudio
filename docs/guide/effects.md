# Effects: particles, lights, fog, post effects

## Particles

- **Particle editor:** every particle generator opens as a form (fields named from the engine code that reads
  them, flags as checkboxes, colours as pickers) with a live preview drawn as dots in the generator's colours.
  Save asks where (Everywhere / This level / This room). **New effect from this** starts a new one.
- **Create > New particle effect:** a new generator from its numbers, on a fire, smoke, sparks or dust look of the
  game (all three games). **Create > Particle emitter** places one.

CLI: `particles <project> [ROOM:]KEY --show | --set FIELD=VALUE | --preview S | --write-view FILE`.

The preview is approximate: no sprite, no children, no planes.

## Lights and fog

- The Inspector edits a light (colour, range, cone, kind, main bank) and a fog.
- Create > Omni / Spot / Directional / Ambient light, Fog; Level kit > Lighting (torches, braziers, lamps: a light
  that lights the Prince and enemies, with its flame).
- **Light the room** (level view: Room > Light the room...) bakes the room's static geometry from its lights;
  "Match the room's other objects" bakes every mod object and moves the fog to cover the room.
- **Change a level's mood** (task): every light tinted, fogs toward a colour.

CLI: `light <project> --room ROOM [--match] [--shadows]` (bake), `light <project> --room ROOM --set KEY --range A,B
--colour R,G,B` (one light), `level-wide --level L --mood PRESET`.

## Post effects (WW, T2T)

- The Inspector's Controllers section has a form for post-effect controllers: tint, colour scale, blur,
  brightness, distance, boundary envelopes. The blocks a volume has are fixed when it is made: the form edits their
  values only.
- **Create > Post-effect volume:** a box with tint, brightness, blur.
- SoT refuses in words: its archive has no post-effect controller.

CLI: `effects <project> --post-effect [ROOM:]KEY [--set tint=R,G,B --set brightness=F ...]`.

## Effects of another game

Prefabs > Other games > **Effects**: the particle objects, lights and fogs of SoT / WW / T2T. Place opens Create
with this game's own creator (lights and fog with the source's values; particles carried field by field, the
sprite painted over the look's). CLI: `effects <project> --other GAME [--search T]`, `effects <project> --import
GAME:KEY --room ROOM --at X,Y,Z`.
