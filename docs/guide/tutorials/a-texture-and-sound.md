# Tutorial a: replace a texture and a sound

You make a mod that changes one texture and one sound, check it, and build a `.jtmod`. About ten minutes. Works on
SoT, WW and T2T.

Sample script: `samples/tutorial-a-texture-sound.ps1` does every step below with `jadestudio-cli` (see
[Sample mods](../samples.md)).

## Before you start

- A workspace: an unpacked game (File > Unpack a game..., or Open project...). See [Getting started](../getting-started.md).
- A picture (PNG, TGA, BMP, JPEG or DDS; any size) and a sound (WAV, OGG or FLAC).

## 1. Make the mod

**Studio:** Mod > New mod... Name it `my_first_mod`, your name as author, template Empty. It becomes the active mod.

**CLI:**

```
jadestudio-cli new-mod <project> my_first_mod --author "Your name"
```

## 2. Find the texture

**Studio:** Ctrl+T and type part of its name (WW names its textures: try `skydome`), or click a model in the level
view and follow its material to the texture in the Inspector. In Simple mode you see game names; View > Full mode
shows keys.

**CLI:**

```
jadestudio-cli index <project> --search skydome
```

A texture line looks like `01004F3F Texture 020_Beach_Skydome`. The 8-digit key is what the CLI needs. SoT and
T2T name few textures: open a room in the level view and pick there, or take a texture's file name in
`<project>/resources/ROOT/Bin/<room>_wow_*/textures/` (the sample does this).

## 3. Export the game's picture (optional)

**Studio:** Tasks > Swap a texture, pick the texture, **Export** (or Build > Export textures...).

**CLI:**

```
jadestudio-cli swap <project> --mod my_first_mod texture --key 01004F3F --export game.png
```

Paint over it in any image editor.

## 4. Swap the texture

**Studio:** Tasks > Swap a texture: the texture, then your picture (or drop the file on the window), then **Swap**.
If the texture is used in several places, choose **Everywhere**, **This level** or **This room**. The models that use it
show the new picture.

**CLI:**

```
jadestudio-cli swap <project> --mod my_first_mod texture --key 01004F3F --file my-texture.png
```

There is no size limit; `--match-size` resizes your picture to the texture's size.

## 5. Replace a sound

**Studio:** Tasks > Replace a sound: pick a bank (SoT and WW: `prince`; T2T banks have 3-character names such as
`003`), an event (play it to hear it), then your file. Or the Sounds panel: select the event, **Replace**.

**CLI:**

```
jadestudio-cli simple <project> sounds prince
jadestudio-cli simple <project> --mod my_first_mod sound prince 0x00030100 my-sound.wav
```

`simple sounds BANK` lists the events and how many samples each plays (0x00030100 is one of SoT's `prince`
events: take yours from the list). WW: add `--vorbis` to keep an `.ogg` as Vorbis.

## 6. Check and play

**Studio:** Play > Check the mod now. The Problems panel lists anything wrong. Then **Play** (F5): openjade starts
at the Play bar's level and checkpoint; walk to where the texture shows, do what plays the sound.

**CLI:**

```
jadestudio-cli check <project> --mod my_first_mod
jadestudio-cli play <project> --level <a level that shows it>
```

Each change is one row in the Changes panel; Revert takes it out. Edit > Undo works too.

## 7. Build and share

**Studio:** Build > Build and share...: fill in the card (title, version, description, picture), run the checks,
**Build .jtmod...**.

**CLI:**

```
jadestudio-cli mod <project> --mod my_first_mod --properties --title "My first mod" --version 1.0
jadestudio-cli use-mod <project> my_first_mod
jadestudio-cli jtmod <project> my_first_mod.jtmod
```

## What was checked

The sample ran on copies of all three games (see [Sample mods](../samples.md)).
