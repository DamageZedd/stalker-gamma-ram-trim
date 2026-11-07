# RAM Trim

A small STALKER GAMMA / Anomaly 1.5.3 addon for reducing and measuring memory use: Lua garbage-collection passes and tuning, a Lua heap report, and opt-in engine memory settings.

Untested in-game beyond a Lua 5.1 syntax check.

## Features

### Lua GC passes
- Logs `[ram_trim]` lines with the Lua heap before and after a full GC at: before level change, level change, actor destroy, game load. Logs the post-load baseline at first update.
- Optional periodic full GC during gameplay (off by default; a full GC can cause a brief hitch).

### Lua GC tuning (on by default)
Lowers the collector's pause so collection starts at 1.5x live heap instead of 2x, keeping the Lua heap smaller. The work stays incremental, so no hitch. Unticking restores Lua defaults (200 / 200).

### Heap report (off by default)
Logs the 15 largest script modules by estimated Lua memory at first update and before level change, to find addons holding lots of memory. Estimates only; the scan is capped at 3 million entries. May add a short delay to loads.

### Engine memory toggles (off by default)
These affect engine RAM, which is where most of the usage is.
- **Clear models on level unload**: sets `r__clear_models_on_unload on`. Frees cached models at level change; loads may be slightly longer.
- **Do not keep textures in RAM**: sets `r__no_ram_textures on`. Avoids a system-RAM copy of textures already on the GPU. Restart the game for full effect.

Unticking reverts a setting only if the mod turned it on in the current session, so values you set yourself are never overridden. The engine may save these values to `user.ltx`, so they can persist after the mod is disabled.

## MCM options (page "RAM Trim")

| Option | Default |
|---|---|
| Run full GC at level changes and loads | on |
| Also run GC periodically during gameplay | off |
| Gameplay GC interval (60-900 s) | 180 |
| Tune Lua garbage collector | on |
| GC pause (100-200) | 150 |
| GC step multiplier (100-400) | 200 |
| Engine: clear models on level unload | off |
| Engine: do not keep textures in RAM | off |
| Log heap report at load and level change | off |
| Log Lua heap size to the game log | on |

## Install

Copy the `gamedata` folder into a new Mod Organizer 2 mod (for example `mods/RAM Trim`) and enable it. No other mod is overridden.

## Notes

- Callback names `on_before_level_changing`, `on_level_changing`, `on_game_load` and `on_option_change` are not verified against GAMMA. If one is wrong, that point produces no log line, or option changes apply at the next load.
- Requires MCM for the options page; defaults apply without it.
