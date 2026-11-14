# Sound, video and text

## Sounds (the Sounds panel)

- Browse banks and events, play and export them, **Replace** a sample, **Add sound** (a new event), **Revert**.
  The quick way is Tasks > Replace a sound.
- **Zone banks:** which banks a level loads (its bank set). Add / remove a bank, "Same as level", remove the set.
  The level check names a sound whose bank the level does not load, with an Add-the-bank fix.
- **This mod's new banks:** a bank new to the game, built when the mod is built and registered in the game's sound
  catalog. Name length: SoT 11, WW 32, T2T 3 characters.
- **Sound of another game:** the samples an event of another game plays, decoded and added as new sounds of a
  bank of this game. A sound that picks at random among its samples, or plays them in a sequence, keeps doing so: a
  new sound of that kind plays the new samples (the bank is then shipped whole). Tick **Each sample as a sound of its
  own** (`sound import --each`) to get the samples only. Chained or switched events and choices of choices are
  refused in words; multitrack events are refused.
- **Music and ambience:** a level's WW music zones and ambience sectors, or T2T sound triggers, with their events.
  **Pick** another event (its bank joins the set) or **Replace with a file**. SoT refuses in words: it has none.
- WW: an `.ogg` is kept as a Vorbis sample (Add sound's codec "Vorbis", `--vorbis`). A WAV cannot become Vorbis
  (no encoder in the tree: refused in words). SoT and T2T refuse Vorbis.

CLI (into the mod): `sound replace <project> [--mod MOD] BANK EVENT FILE`, `sound add <project> BANK FILE`,
`sound revert <project> ID...`, `sound new-bank`, `sound banks`, `sound import --from GAME`, `sound music --level L`,
`sound zone <project> --mod MOD --level L [--add BANK] [--remove BANK]`, `simple <project> sound ...`.
`sound list|events|export <game folder>` read the game's own banks.


## Videos (the Videos panel)

Replace a game video (or a gallery still) with your file. CLI: `video <project> --list | --replace TARGET --with FILE
| --revert ID`, or `simple <project> video TARGET FILE`. A video can also be played by a trigger box:
see [Cutscenes](cutscenes.md).

## Text (the Text panel)

- Menu text, tutorial hints and subtitles (with a cue editor), archive text, a new language.
- Tasks > Change a text for one string. CLI: `simple <project> text KEY TEXT [--tutorial] [--language L]`,
  `simple <project> texts QUERY`, `text-revert KEY`.
- Cutscene subtitles are written into the mod's English subtitles by the Cutscene task.
- Create > Text: Menu text, Tutorial line, Subtitle line.

Text and video edits are shipped in the `.jtmod` and staged for Play (openjade reads `Languages/<Name>/<Kind>/mods/`).
