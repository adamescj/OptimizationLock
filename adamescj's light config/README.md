# adamescj's Light Config

A visuals-first edit of [Sqooky's OptimizationLock](https://github.com/Sqooky/OptimizationLock) default config.
It keeps the cheap wins (CSM shadow cuts, particle/animation/audio tweaks, IK off, etc.) and puts back
the handful of settings that make the game look noticeably worse or hide gameplay information.

All of the actual optimization work is Sqooky's and the OptimizationLock contributors' — this is just a re-tune.

## What's different from Sqooky's default

| Setting | Sqooky's default | Light | Why |
|---|---|---|---|
| `r_aspectratio` | `2.9` | `2.2` (≈91° FOV) | Closer to stock FOV, less fisheye |
| `sc_fade_distance_scale_override` | `100` | game default | Blast vents / vent wind visible at range again |
| `sc_screen_size_lod_scale_override` | `0.55` | game default | No more low-poly heroes or "triangle" Sinner's lights |
| `r_size_cull_threshold` | `0.9` | game default | Trooper healthbars and boxes don't vanish at range |
| `r_farz` / `r_mapextents` | `7000` | game default | No building / player pop-in |
| `lb_enable_baked_shadows` | `false` | game default | Map isn't flat and washed out |
| `lb_enable_stationary_lights` | `false` | game default | Proper lighting |

Each changed line is marked in `gameinfo.gi` with `// [light: restored to default]`, so you can search for it
and flip any of them back if you want the FPS instead.

**Expect a few FPS less than Sqooky's default** — mostly from the lighting. If you need some back, re-enable
`lb_enable_stationary_lights "false"` and `lb_enable_baked_shadows "false"` first; they're the biggest cost.

## Install

Replace `steamapps/common/Deadlock/game/citadel/gameinfo.gi` with the `gameinfo.gi` in this folder
(back up the old one first). Or let the updater do it and keep it up to date:

```
python "auto updater/gameinfo_updater.py" update --config light
```

Using a mod loader that adds its own search path (e.g. Grimoire)? The updater keeps it for you. If you copy the
file by hand, re-add the loader's `Game` line above `Game "citadel/addons"` in `SearchPaths`.
