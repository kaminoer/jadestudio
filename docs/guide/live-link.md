# Live link: the running game

> This part needs an openjade built with the live link (`--live-link <port>`). The openjade side is compiled and
> unit-tested in a separate folder, **not deployed**, and never run in a real level. Until your openjade has it,
> the Live panel cannot connect.

## Start

The Play bar starts openjade with the live link (the Play bar's "..." options: "Live link: pick, teleport, camera,
variables and breakpoints", on by default) and writes `live-link.txt` into the play-test folder. The **Live panel**
(View > Panels > Live) opens and connects when Play starts the game.
CLI: `jadestudio-cli play <project> --live-link`, then `jadestudio-cli live [--port N | --staging DIR] <command>`.

## What it does

| Live panel | CLI (`live ...`) |
|---|---|
| Pause / resume (the free camera while paused) | `pause`, `resume`, `state` |
| Pick both ways: the in-game object inspector selects in the level view, and the reverse | `select`, `selected`, `object` |
| Teleport the Prince to the level view's point (or coordinates) | `teleport` |
| Camera game to view, view to game (the free camera), follow | `camera`, `camera-set` |
| Watched variables (by name or hash), read twice a second, set | `get`, `set` |
| Breakpoints by function name or id and node; the stop's stack by name; Continue / Step | `break`, `unbreak`, `breaks`, `break-clear`, `wait-stop`, `continue`, `step`, `stack` |
| Breakpoints on a line of a script's text (the script key and line, or **Show lines** and click a marked line); a stop shows its script and line | `lines`, `break-line`, `unbreak-line`, `wait-stop --project P` |
| At a stop: the call's arguments and locals with their values | `locals [LEVEL] [--project P]` |
| Unlocks and sand credits (the player's profile) | `unlocks`, `unlock`, `credits` |
| A console command | `console` |
| **Hot reload:** the mod built again, the game restarted at the same level and start, the Prince where he stood | `reload` |

`live fake` runs a fake game for trying the panel and the commands without openjade. `live events` prints what the
game sends (selected, stopped, resumed, bye). See `jadestudio-cli live help` for each command's arguments.

## Limits (refused in words)

- A changed room cannot be swapped into the running level, so Hot reload restarts the game. openjade can drop and
  load rooms while it runs, but it reads the mod's files once, when it starts (where each file sits, kept for the whole
  run), keeps a room that stays in the level loaded, and keeps the pieces other rooms share.
- Line breakpoints: the lines are those of the script's text as the script editor shows it (the mod's copy when the
  mod changed the script). Only lines inside a function can stop (a script's main part is not a called function);
  other lines are refused with the next line that can. A closing brace is refused (its block's first line holds the statement); some
  `return` lines are folded by the text into the line before and have no stop of their own (`live lines` marks the
  lines that can stop with `*`).
- Locals: the compiled script carries no names. Arguments are named from the function's first line; locals by their
  declarations when they match, else by their place ("local +0x8").
- Story flags have no table: they are script variables, watched and set here.
- Unlock bits are the player's profile, set live, not mod data.
