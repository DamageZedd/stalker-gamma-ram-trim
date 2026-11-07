# RAM Trim

A small STALKER GAMMA / Anomaly 1.5.3 addon that logs Lua heap size around level changes and loads, and optionally runs a full garbage-collection pass at those points (and periodically during gameplay).

**It only affects Lua memory.** Most loading-screen RAM and load time is engine-side (textures, models, A-Life data) and is not touched by this mod. Use the log output to see whether Lua heap is a meaningful share of your spike. Untested in-game beyond a Lua 5.1 syntax check.

## What it does

- Logs `[ram_trim]` lines with the Lua heap before and after GC at: before level change, level change, actor destroy, game load.
- Logs the post-load baseline at first update.
- Optional periodic GC during gameplay (off by default; a full GC can cause a brief hitch).

## MCM options (page "RAM Trim")

- Run full GC at level changes and loads (default on)
- Also run GC periodically during gameplay (default off)
- Gameplay GC interval, 60-900 s (default 180)
- Log Lua heap size to the game log (default on)

## Install

Copy the `gamedata` folder into a new Mod Organizer 2 mod (for example `mods/RAM Trim`) and enable it. No other mod is overridden.

## Notes

- Callback names `on_before_level_changing`, `on_level_changing` and `on_game_load` are not verified against GAMMA; if one is wrong you will see no log line for that point.
- Requires MCM for the options page; defaults apply without it.
