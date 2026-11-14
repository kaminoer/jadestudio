# Tutorial c: a cutscene with an actor and dialogue

You make a camera cut that plays when the Prince walks into a trigger box: a still shot and a glide that follows the
Prince, the Prince's line with its subtitle, a second line three seconds in, and a character playing a cinematic clip.
Works on SoT, WW and T2T.

Sample script: `samples/tutorial-c-cutscene.ps1` (see [Sample mods](../samples.md)).

## 1. A room to hold it

Any room works: a game room or a zone's. The sample makes a Blockout zone `SampleCut` (see
[Tutorial b](b-new-zone.md), step 1). Open the room in the level view.

## 2. Open the Cutscene task

**Studio:** Mod > Create new > Cutscene... It points at the level view's room. The camera you make is a copy of the
game's own camera cut (WW 615_Forge_Pres_CAMCUT, SoT 1401_Cour_CAMCUT_Pres, T2T SE410_130_CAM).

## 3. Shots

**Studio:** in the level view, move to where the camera should stand; **Add a shot** (**From the view camera** takes
the view's eye and where it looks; set its seconds), then **Add a path** and **Add key from the view camera** for two
or more keys, each looking at a point or at the Prince. The **Preview** section plays it in the level view.

**CLI** (world coordinates): `--shot X,Y,Z@LX,LY,LZ:SECONDS`, `--path "X,Y,Z@prince:SECONDS; X,Y,Z@prince:SECONDS"`.

## 4. Lines and subtitles

**Studio:** **The first line**: pick the Prince's line in the list (filter by speaker), type its subtitle. **More lines
(dialogue)**: right-click a line > **Add as a later line**, at 3 seconds, with its subtitle.

**CLI:**

```
jadestudio-cli cutscene <project> --lines
```

Each line shows its sample, event, bank, length and text. Use the event id with `--line` and `--then LINE@SECONDS`.

## 5. An actor

**Studio:** **Actors** > **Add actor**: pick a clip (the cinematic clips of the game, and the mod's new clips of the
room's characters), **At the view's point**, facing 90 degrees, **his head too** for the Prince. Optional: starts
after, **end marker** (**End at the view's point**).

**CLI:**

```
jadestudio-cli cutscene <project> --clips
```

A clip is `ROOM:KIT` (for example WW `010_DeckPart2:3100C2FF`, a pirate of the opening cinematic).

## 6. The start, then Create

**Studio:** under **Start**, **When the Prince walks into a box** (**Where the view looks** puts the box) or **When the
level starts**; read the **Review**, then **Create** (one undo step, a Changes row).

**CLI** (the whole cutscene in one command):

```
jadestudio-cli cutscene <project> --mod my_cut --room SampleCut --name SampleCutIntro
    --shot 1505,-1505,3@1500,-1500,1:3
    --path "1505,-1495,3@prince:2; 1495,-1495,3@prince:2"
    --line 00220055 --subtitle "This feels oddly familiar."
    --then 00220054@3 --then-subtitle "I'll need to start watching my back."
    --actor 010_DeckPart2:3100C2FF@1502,-1500,0:90
    --trigger 1501,-1499,0
```

(One line in a shell; the line ids are WW's: take yours from `--lines`.)

## 7. Check, edit, play

```
jadestudio-cli cutscene <project> --mod my_cut --room SampleCut --list
jadestudio-cli cutscene <project> --mod my_cut --room SampleCut --show SampleCutIntro_Cutscene
```

**Studio:** select the cutscene in the task's room list to **edit** it (parts, lines, actors; one undo step, the same
keys) or **Remove** it. `--edit NAME ...` and `--remove NAME` on the CLI. Then **Play** and walk into the box.

## What was checked

The sample made this cutscene (lines, subtitles, one actor) on copies of all three games.
