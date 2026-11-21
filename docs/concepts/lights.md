Jade Studio has its own baker. It computes lighting the same way the engine does and writes the results into the room's data. You can also bring in lighting you made yourself, for example in Blender.

## How the engine lights things

The game has two kinds of objects, and the bake only touches the first:

- **Baked statics.** These are visuals with draw flag `0x40` ("baked vertex colors"). The engine draws their stored per-vertex colours and adds no lighting at run time. The colour multiplies the texture ×2, so 0.5 shows the texture as painted and 1.0 is twice as bright.
- **Run-time lit objects.** This means the Prince, enemies, traps and anything animated or scripted. They get the ambient colour plus up to 4 nearby lights each frame (3 in Sands of Time). The Prince takes his ambient colour from the visual he's standing on. The bake never writes these objects.

The baker uses the engine's own formula: `ambient + Σ light colour × N·L × linear falloff between the light's start and end range`. Baked walls and the lit Prince therefore match when they stand side by side.

## Built-in tools

| Tool | Where | What it does |
|---|---|---|
| **Light the room** | Room menu / `light --room` | Bakes vertex colours from the room's enabled omni, spot and directional lights |
| `--shadows` | | A ray from each vertex to each light, tested against the room's collision |
| `--mesh-shadows` | | Visible meshes without collision (tree crowns, banners) also cast shadows |
| `--occlusion` | | Darker corners, from short hemisphere rays |
| `--light-maps` | | GPU-baked shadow textures on floors (D3D11 compute with a BVH, 4×4 supersampling per texel, soft sun edges). Falls back to the CPU if there's no D3D11 device |
| `--match` | | Bakes every baked visual the mod added and links run-time objects (flames, weapons) to the room's ambient/fog pair, so statics and dynamics share one ambient |
| **Bake for this object** | Inspector / `vertex-colors --bake` | Bakes one object. |
| **Light editing** | Inspector / `light --set` | Colour, range, kind, cone, enabled, main bank, channel, shadow projector, per-pixel bank |
| **Live-only lights** | `--live-only` | A fill or key light for characters that the bake ignores |
| **Room mood** | `mood` | Ambient, fog, key light and post effect in one step, from presets measured on retail rooms (or copied from any retail room), then a bake |
| **Report / survey** | `light --report`, `--survey` | What each object is actually drawn with, and how retail rooms are lit |

## Bringing your own lighting

- **Importing geometry with `--lighting vertex`:** the glTF's `COLOR_0` becomes the baked colour. A lightmap named in a node's or material's `extras.lightmap` is used through `TEXCOORD_1`. `--lighting quick` gives a flat ambient instead.
- **Vertex lighting round trip:** Export writes a `.glb` with the colours and lightmap UVs. Paint or bake them in Blender, then import. Only colours and UVs come back, so the vertices must stay where they are.
- Vertex alpha (for blend overlays) survives re-bakes.

## What it can't do

- **Direct light only.** There's no bounce or radiosity. The flat ambient colour (times the corner occlusion) stands in for bounced light. For real GI, bake it in Blender and import it.
- **Shadow detail depends on triangle size.** Vertex colours can't show a shadow smaller than the mesh's triangles. Use `--subdivide` when importing, or light maps.
- **Light maps are limited:**
  - They only go on floors (60% or more of the area facing up), in rooms the mod made.
  - The projection is top-down, so walls get none.
  - They're shadow-only: the engine draws colour × (1 − alpha) and doesn't add any colour.
  - They're large. At 0.05 m per texel, 7 floors came to about 25 MB of textures.
- **Only the mod's own statics by default.** Retail objects keep their baked colours unless you bake them one at a time, and placed prefabs need `--prefabs`.
- **Skipped objects:** LOD sets and selectors, skinned meshes, skies, and objects that store no colours. Each skip is listed with a reason.
- **Not live in the game.** Rebake after moving a light. The level view does preview it.
- **Character shadows** come from shadow-projector lights (Warrior Within and The Two Thrones only), and only land on visuals flagged as receivers. Every receiver near a projector is redrawn for each shadow map, every frame, so keep them to a region (`--receive-region`).
- **Normal-mapped materials** only take per-pixel-bank lights plus the ambient. A room without a per-pixel light leaves them in flat ambient light.
- **Sands of Time** has no post-effect volumes or shadow projectors.
- **Copying lights and fog from another game** only partly works.

The practical workflow:
1. Place your lights.
2. Use Room mood or a `--fog-from` room for the ambient and fog.
3. Run **Light the room** with `--match --shadows --occlusion`, plus `--light-maps` on big open floors.
4. Bake hero pieces in Blender if you want bounced light.