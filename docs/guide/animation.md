# Animation

## The animation editor

Open any animation in the Assets panel. Tabs:

- **Skeleton:** the character's bones (and, with Mesh, its skinned meshes) posed by the clip; play, pause, step,
  speed, loop, root motion on or off, a timeline with the event keys. The bones are listed in their tree with their
  names and how many vertices each moves; the view shows the clip's pose, the **bind pose** (the meshes as stored,
  the bones where the skin holds them) or the **rest pose**. Pick a bone to see its weights painted on the mesh
  (blue: not moved by it, through green and yellow to red: moved by it alone).
- **Channels:** the clip's channels. **Matrix keys** can be set here (no retail clip of the three games has any).
- **Events:** the event keys (footsteps, sounds, events) as markers and a table. Move a key, copy it to the
  playhead, remove it, point it at another sound event, or **New key at the playhead** from the room's program list.
  A clip without an event channel refuses a new key in words.

CLI: `animation <project> show | pose | rig | events | event | event-add | programs | matrices | matrix`
(`jadestudio-cli animation --help`). `animation <project> rig ROOM CHARACTER [--bone B]` prints the rig: the bone
tree, or one bone's parent, children, weights and where it sits at rest and at bind.

**Tasks > Animation bank** also has **Rig...**: the same rig view for the chosen character, without a clip.

## Banks: which clips a character plays

**Tasks > Animation bank** lists a character's bank. Replace an entry with another clip of the same skeleton, or
add one. **New clip...** makes a new clip:

- from a glTF animation (Blender's animation export), or
- from another character's clip, of this game or another (retargeted by bone name: exact, else the 3ds Max Biped
  part after the rig's prefix; rest pose corrected; root motion scaled by hip height). Refused in words when fewer
  than half the bones match.

The new clip keeps the replaced clip's event keys, retimed. CLI:
`animation <project> new-clip [--from ROOM:KEY [--from-game sot|ww|t2t]] [--glb FILE] [--entry N] [--dry-run]`.
Create > Animation > Animation clip does the same from the Create page.

**Swap an animation** (task, `swap ... animation`) replaces a clip with a `.glb` animation on the same bones.

## Limits

- Object-driving channels are shown, not edited.
- Replacing an entry with a new clip can copy many blocks ahead in the room (its read order shifts); it loads in
  place but the room grows.
- A new event channel for a clip that has none is not made.
