# Cutscenes

**Mod > Create new > Cutscene...** (the Cutscene task) makes an in-engine camera cut: a copy of the game's own
(WW 615_Forge_Pres_CAMCUT, SoT 1401_Cour_CAMCUT_Pres, T2T SE410_130_CAM) with its own camera animation.

## What it holds

- **Shots and paths:** still shots (a point and where it looks, for some seconds) and glides through 2 to 64 keys
  on a smooth spline; a key can look at the Prince. Up to 8 parts, cut between them. Preview it in the level view.
- **Lines:** the Prince's line (a sample or event of the subtitles, or its text), its subtitle and speaker;
  **More lines**: later lines of any speaker, each at its own time. Subtitles go into the mod's English subtitles.
- **Actors:** a character playing a retail cinematic's clip (the Prince with his head, Farah, Kaileena, the Dahaka,
  cinematic bodies...), placed at a point and facing; **starts after** (its clip held on its first pose until then),
  **end marker** (where it stands when the cut ends). A new clip your mod made on a character is offered as
  `ROOM:KIT#ENTRY`. T2T actors go through a copy of 100_Quai's scripted event.
- **Start:** when the level starts (at its start checkpoint) or a **trigger box**.
- **Timeline:** camera parts, lines and actors as tracks.
- **Video trigger:** a trigger box that plays a game video (optionally your replacement file in the same step).

Edit a made cutscene (its parts, lines, actors: one undo step, the same keys) or Remove it (with its kits, parts,
end markers).

## CLI

```
jadestudio-cli cutscene <project> --mod MOD --room ROOM --shot X,Y,Z@LX,LY,LZ[:S] --path "X,Y,Z@prince:T; ..."
    --line TEXT|ID [--subtitle TEXT] [--then LINE@S --then-subtitle TEXT] [--actor ROOM:KIT@X,Y,Z[:YAW]
    [--actor-start S] [--actor-end X,Y,Z[:YAW]]] [--trigger X,Y,Z | --on-start] [--name NAME]
jadestudio-cli cutscene <project> --lines [--search WORDS] | --clips [--room ROOM] | --videos
jadestudio-cli cutscene <project> --mod MOD --room ROOM --list | --show NAME | --edit NAME ... | --remove NAME
jadestudio-cli cutscene <project> --mod MOD --room ROOM --video CODE --trigger X,Y,Z [--with FILE]
```

The line's sound bank is added to the level's bank set (unless `--no-bank`: then the line is silent).

## Limits

- Retail SoT and T2T camera cuts never say a line: the Studio writes the voice channel in WW's shape for them.
- T2T scripted events: props' events, effects and sounds beyond actors are not built.
