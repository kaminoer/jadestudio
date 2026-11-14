# Textures, models and characters

## Textures

- **Texture editor:** double-click a texture in the Assets panel. Replace it with a picture, see it on the models
  that use it. A palette (`palette-png`) opens in the texture editor too.
- **Swap a texture** (task) and **Use a texture pack** (task): see [Swaps](swaps.md).
- **Create > Texture** makes a new texture (DXT1, DXT5 or ARGB32); "Largest side" has no limit by default.
- **Create > Texture of another kind:** palettized (format 1, its own 256-colour palette; all three games) or
  animated (type 9; SoT only: WW and T2T have no retail animated texture, so they refuse it in words). Format 12
  and types 4 / 5 are refused in words: no game uses them.
  CLI: `create <project> --what texture-kind --set kind=palettized|animated ...` (`create --describe texture-kind`).
- **No size limit:** openjade draws any size. Only the header's 16-bit size words bound a side.


## Materials

- A material (`material-json`) opens in a **Fields** form above its text: every field typed, colours as pickers.
- Create > Material / Multi-material make new ones.
- CLI: `view <project> <view file> --get [POINTER] | --set POINTER=JSON` sets any field as the forms do.

## Models

- **Swap a model** (task, `swap ... model`): a `.glb` in place of an object's model.
- **Edit in Blender** (on a model or a character part; the **Blender** panel): the model opens in Blender with the
  Studio's add-on loaded from its folder (nothing installed); each send from Blender comes back through the model
  swap's checks, one undo step per send. **End session...** closes the Blender started for it.
  CLI: `blender <project> --status | --start --key MESH [--launch] | --push | --pull [--send N] | --stop`.
  The command line's session works as the panel's: materials Blender renamed are matched to the model's slots, the
  pictures are the mod's copies of textures you changed, and a mesh shown through a LOD selector or by a character
  gets the pictures of the object that shows it.
- **Import geometry** (task; `import-geometry`): your own level geometry from glTF. See
  [Levels, rooms and zones](levels-rooms-zones.md).
- **LOD selector** (Create > Meshes & collision > LOD selector; `create --what lod-selector --set room=R
  --set object=KEY --set meshes=KEY,...`): an object shows another mesh far away. Refused in words for objects that
  already show a LOD set or selector. An object with baked vertex colours: the game gives every mesh of the selector
  the object's one colour list by vertex number, so a farther mesh is accepted when its vertices are the near mesh's
  first ones in the same order; a mesh with more vertices than colours is refused (they would draw black); any other
  is refused unless you tick **Use baked colours anyway** (`--set anyColours=true`). Another game's selector:
  Prefabs > Other games > Effects, LOD selectors (`effects --import sot:KEY --room R --set object=KEY --set
  meshes=KEY,...`): its switch sizes over this game's object and meshes.

## Characters

- **Change a character's look** (task): its textures, or a `.glb` rigged to the character's own skeleton.
- **New enemy variant** (task; `jadestudio-cli character`): a new enemy from one of the game's: its own skin,
  stats, cloned behaviour and animations, placed in a level or a zone's room (the needed world added to the level).
- **Skeleton view:** the animation editor's Skeleton tab shows the character's bones and (Mesh / Bones) its skinned
  meshes posed by any clip. CLI: `animation <project> pose [--mesh]`.
- **Characters of another game:** Prefabs > Other games > Characters. See [Cross-game import](cross-game.md).
- **Skinned meshes over 65,535 vertices** are written in the split form openjade reads (draws of at most 48
  palette records for DX9; the Vulkan path has no limit).

## Objects and their parts

- **Inspector:** transform, visual, light, particles, collision shapes, controllers, behaviour of the selection.
- **Inspector > Structure:** add or remove parts (visual, animation, parent, behaviour, script properties,
  collision, navigation links, sound roll-off...) and controllers, copied from a donor object of the same room.
  CLI: `object <project> --room R --object K --structure | --add PART --from DONOR | --remove PART |
  --add-controller --from DONOR[:N] | --remove-controller I`.
- **Rename, delete, re-parent:** the Outliner and the Inspector; CLI `object --rename NAME`, `object --delete KEY,...`,
  `object --reparent PARENT|none`.
- **Collision:** the level view toolbar's **Collision** box moves, turns or scales a shape; **Collision tools** has
  exact numbers and **Paint faces** (a game material of the mesh's table). Boxes turn in quarter turns only.
  CLI: `object --collision [--shape I --move X,Y,Z --scale X,Y,Z --turn D | --paint --at X,Y,Z --radius R --material TAG]`.
- **Game materials:** Create > Game-material table; Create > Game materials of another game (an entry is taken
  only when this game's own table uses every value it carries).
- **Raw editor** (Full mode): **Override with a file...** / **Revert override** for any block.
  CLI: `raw <project> --pack P --list | --block KEY --extract F | --override F | --revert`.
