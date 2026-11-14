# Scripts and behaviours

An object's **behaviour** (its AI model) is its scripts, its function tables and its variables. Objects of the same
behaviour share the scripts; each has its own variable values.

## The script editor

Open a script from the Assets panel or the Inspector's Behaviour section. It compiles as you type (errors on their
line), completes names, shows signatures and hovers, goes to a definition, finds references, and has a Snippets strip.
When the game logs a script fault after Play, the Problems panel's **Open script at the fault** opens it at the line.

CLI: `check <project> [--mod MOD]` compiles every changed script as the cook will.

## Variables

The variables editor shows a behaviour's defaults and each object's own values; bulk edits over several objects;
**Add a variable** (appended, so old offsets stay valid). **Tweak a character's stats** (task) is the quick way.
CLI: `variables <project> --room R --objects K,... --list | --set NAME=VALUE...` (one undo step).

## Your own behaviour

- **Clone behaviour** (script editor, variables editor, Inspector): a copy of everything the behaviour owns alone,
  new keys, every function id renumbered, its calls rewritten. Copy / Duplicate offer "Clone behaviour" too.
- **Rebind:** point an object at another behaviour of the same room (`rebind <project> --room R --object KEY
  --behaviour MODEL`).
- **Add function**, **New script file**.
- **Create > Behaviour / Variable / Script function / Script file.**

## Add a behaviour (task)

Wires the game's own pieces with no new script, or adds a small script to a tutorial-trigger zone:

- "Wire" templates: a lever or plate opens a door, an area starts a spawner, go to a zone (WW / SoT ChangeMap), a
  checkpoint here, a timed trap.
- "Script" templates: a zone goes to a zone, opens a door, starts enemies or runs a trap.

Each template refuses in plain words what it cannot wire. CLI: `behaviour <project> --list | --template ID
--set FIELD=VALUE... [--dry-run]`. Trigger zones also have the actions arm-kill-zone / disarm-kill-zone.

## Behaviours of another game

Prefabs > Other games > **Behaviours** (`import-behaviour <project> --from GAME --list ROOM | --source ROOM:OBJ
--to ROOM:OBJ,...`): a behaviour's model, variables, scripts and tables carried onto objects of this game. Every
builtin call is checked against both games' catalogs (same handler: kept; same name and stack traffic: translated;
else refused and listed). Calls into other behaviours' functions are always refused: function ids differ per game.
