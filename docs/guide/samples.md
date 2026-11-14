# Sample mods

Five PowerShell scripts in `tools/jadestudio/samples/` build each tutorial's mod with `jadestudio-cli`, step by step,
and print what each step did. Read them next to the tutorials: each step names its Studio menu path.

| Script | Tutorial | Mod it makes |
|---|---|---|
| `samples/tutorial-a-texture-sound.ps1` | [a: a texture and a sound](tutorials/a-texture-and-sound.md) | sample_texture_sound |
| `samples/tutorial-b-zone.ps1` | [b: a new zone](tutorials/b-new-zone.md) | sample_zone (zone SampleZone) |
| `samples/tutorial-c-cutscene.ps1` | [c: a cutscene](tutorials/c-cutscene.md) | sample_cutscene (zone SampleCut) |
| `samples/tutorial-d-boss-arena.ps1` | [d: a boss arena](tutorials/d-boss-arena.md) | sample_boss (zone SampleArena) |
| `samples/tutorial-e-other-game-character.ps1` | [e: a character of another game](tutorials/e-character-from-another-game.md) | sample_guest (zone SampleGuest) |
| `samples/run-samples.ps1` | all five on copies of unpacked games, one game at a time | - |
| `samples/SampleLib.ps1` | the shared helpers | - |

## Run one

Work on a **copy** of an unpacked game:

```
robocopy "D:\Jade Studio\WW" "D:\Jade Studio\WW-try" /MIR
powershell -File samples\tutorial-b-zone.ps1 -Project "D:\Jade Studio\WW-try"
powershell -File samples\tutorial-e-other-game-character.ps1 -Project "D:\Jade Studio\WW-try" -Source "D:\Jade Studio\T2T"
```

Options: `-Mod NAME`, `-Cli PATH` (default: the newest `jadestudio-cli.exe` of a `build*` folder, or
`JADESTUDIO_CLI`), `-Report steps.json`; tutorial a takes `-TextureKey`, `-Bank`, `-Event`; b `-Family`; c `-Line`,
`-Then`, `-Actor`; d `-Boss`; e `-Character`. Then open the copy in the Studio: the mods are there, each step one row
in the Changes panel, and Play starts them.

Each step ends as **done**, **refused** (the Studio said why, in words; the script goes on), **failed**,
**problems** (it ran, a check listed problems) or **skipped** (nothing to do on this game).

## Run all of them

```
powershell -File samples\run-samples.ps1 -Fixtures <folder with ww, sot, t2t> -Scratch <work folder> [-Games ww,sot,t2t]
```

It copies each game, runs the five scripts, deletes the copy, and prints the steps that did not run through.

## Results on the three games (2026-11-14)

`run-samples.ps1` on copies of the three unpacked games, with that day's jadestudio-cli.
Every step of the five tutorials ran through on SoT, WW and T2T: **no step refused, none failed**.

| Tutorial | SoT | WW | T2T |
|---|---|---|---|
| a texture + sound | 10 of 10 (texture 13008340 of 0101_Entree, bank prince) | 10 of 10 (020_Beach_Skydome 01004F3F, bank prince) | 10 of 10 (texture 0A001ACB of 100_Quai, bank 003) |
| b new zone | 22 of 22: column, ladder 8 m, omni light, checkpoint, Sand Forgeron spawner, grid, network, root, kill zone, exit last (0101_Entree), a light placed `--local` after it, mod check clean | 22 of 22: swing bar, ladder 6 m, wall torch, Disciple spawner, ..., exit last (055_ExitTo045), mod check clean | 22 of 22: column, ladder 6 m, omni light, Sand Garde spawner, ..., exit last (100_Quai portal: the room and its origin moved), a light placed `--local` after it lands in the room, mod check clean |
| c cutscene | 9 of 9 (two lines, one actor) | 9 of 9 | 9 of 9 (actor through the copied scripted event) |
| d boss arena | 11 of 11 (Vizier) | 11 of 11 (Shahdee, Sacrifice Room) | 11 of 11 (Twins) |
| e other game's character | 10 of 10 (WW ACT_Enemies_Mahasti) | 10 of 10 (T2T ACT_Enemy_Mahasti) | 10 of 10 (SoT SandGuard, imported as ACT_SandGuard_02) |

What did not go cleanly in an earlier run, and how it was fixed (tutorial b reran on all three games on 2026-11-14,
one game at a time, every step done):

- **b, all three games:** after the patrol network the mod check (`check`, Play's check) listed
  `resources/ROOT/Bin/<zone>_wow_*/world/<key>.json: the game has no such file any more`. The Level kit placements
  had edited the zone room's world and object list through views; the network writes those blocks with the views'
  content taken in and drops the views' records, but left their files. Now the files go too (Undo puts them back).
- **b, WW:** `zone --connect` after the patrol network was refused ("the zone's room would ask for keys none of its
  level's worlds holds": textures such as AA00042D, 0F0444xx). The network did not add them: the wall torch's materials
  (from 045_GueriteINT) name textures 045's other rooms hold, and retail 045_GueriteINT asks for the same ones (some,
  like 0F04448E, no world holds at all). Connect read the room without the mod's views, so before the network it never
  saw the torch. Connect now refuses only keys it makes the room ask for and names the ones the room asked for before
  in a note.
- **b, T2T:** Connect moved the zone's room to the portal's doorway but left the zone's recorded origin
  (zones.json) at 1500, -1500, 0. The origin now moves with the room in the same undo step (also when the zone is made
  with its connection), so `--local` and "From the zone's origin" find the room afterwards.

The sample now uses one order on every game: the zone, its contents, the exit last, then a light placed `--local`.
- **e, all three games:** the source character's own behaviour did not carry (its scripts call other behaviours'
  functions, whose ids differ per game): the base's behaviour runs, as the Studio says.
