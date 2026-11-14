# Jade Studio user guide

Jade Studio makes mods for the three Prince of Persia games of the Jade engine: The Sands of Time (SoT), Warrior
Within (WW) and The Two Thrones (T2T). You test them in openjade and share them as a `.jtmod`.

This guide is for every modder: from a first texture swap to a whole new level. Each page says how to do a thing
in the Studio (the menus and panels) and with `jadestudio-cli`, the command line that does the same.

In the Studio: **Help > User guide** (F1) opens this guide; **Help > Tutorials** lists the tutorials.
On the command line: `jadestudio-cli help guide [PAGE]` and `jadestudio-cli help tutorial [NAME]` print the pages as
plain text (tables in columns); `--markdown` prints them as written.

## Start here

- [Getting started: workspace, mods, Play, Build and share](getting-started.md)

## What you can change

- [Swaps: the quick tasks](swaps.md)
- [Textures, models and characters](textures-models-characters.md)
- [Animation](animation.md)
- [Sound, video and text](sound-video-text.md)
- [Levels, rooms and zones](levels-rooms-zones.md)
- [The Level kit and Create](level-kit-and-create.md)
- [Enemies and navigation](enemies-navigation.md)
- [Bosses and story sets](bosses-story-sets.md)
- [Cutscenes](cutscenes.md)
- [Effects: particles, lights, fog, post effects](effects.md)
- [Scripts and behaviours](scripts-behaviours.md)
- [UI and progression: fonts, menus, saves](ui-progression.md)
- [Cross-game import](cross-game.md)
- [Mod lifecycle and sharing](mod-lifecycle.md)
- [Live link: the running game](live-link.md)

## Reference

- [What each game supports, and why](per-game.md)
- [What is not possible, and why](not-possible.md)
- [CLI reference](cli.md) (made from the CLI's own help)
- [Sample mods](samples.md)

## Tutorials

- [Tutorial a: replace a texture and a sound](tutorials/a-texture-and-sound.md)
- [Tutorial b: a new zone with parkour, a checkpoint, enemies and an exit](tutorials/b-new-zone.md)
- [Tutorial c: a cutscene with an actor and dialogue](tutorials/c-cutscene.md)
- [Tutorial d: a boss arena](tutorials/d-boss-arena.md)
- [Tutorial e: bring a character from another game](tutorials/e-character-from-another-game.md)

## Words used in this guide

| Word | Meaning |
|---|---|
| Workspace | One unpacked game (a bfcook project folder). The Studio never changes its files. |
| Mod | Your changes, kept apart from the game in `<project>/jadestudio/mods/<mod>/`. |
| Level | What the game loads at once (a WOL). It lists rooms and technical worlds. |
| Room | One world pack (a WOW) of a level: objects, lights, collision. |
| Zone | A new level of your mod, made with New zone. |
| Technical world | A world every level loads: the Prince, the camera, enemy templates, move kits. |
| Key | The 8-digit hex name of an object or resource (shown in Full mode). |
| One undo step | Every action can be undone with Edit > Undo, also after a restart. |
