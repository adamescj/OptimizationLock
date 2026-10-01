# OptimizationLock — adamescj's fork

Performance configs (`gameinfo.gi`) for **Deadlock**, plus a lighter, visuals-first variant and a few fixes.

> **Credit where it's due:** this is a fork of [**Sqooky's OptimizationLock**](https://github.com/Sqooky/OptimizationLock).
> Nearly everything here — the configs, the research, the convar documentation — is the work of Sqooky and the
> OptimizationLock community (boot, Kaizuchaneru, Piggy, Jasper, Abdalla, Artemon121, Kunet, Maihdenless, the translators,
> and many more). The full original README, with every contributor, translator and donor, is kept in
> [ORIGINAL_README.md](ORIGINAL_README.md).
>
> If this helps you, please support the original author: **[ko-fi.com/sqooky](https://ko-fi.com/sqooky)** ·
> [OptimizationLock Discord](https://discord.gg/EF3Jq57jQv)

---

## What this fork adds

- **[A light config](adamescj's%20light%20config)** — Sqooky's default with the ugliest cuts reverted: normal lighting,
  no pop-in, proper LODs, vents visible at range, and a saner ~91° FOV.
- **Updater keeps your mod loader.** `auto updater/gameinfo_updater.py` used to throw away any extra search paths
  (e.g. Grimoire's `citadel/grimoire`) on every update. It now carries them over. There's also a `--config light` shortcut.
- **Duplicate convar cleanup.** Several configs set the same convar twice, and only the *last* line counts — so editing
  the first one silently did nothing. The shadowed copies are now commented out and tagged
  `// [duplicate - overridden by the later ...]`. Nothing about how the configs behave changed.
  - Heads-up: in Sqooky's default, `sc_layer_batch_threshold_fullsort` is documented as `120`, but the stock block later sets `20`,
    and `snd_ui_positional "false"` is likewise overridden by a later `"1"`. That was already the case before; it's just visible now.
- **Fixed a broken file.** `Sqooky's .gi/addons/pak54_dir.vpk` was a symlink to a path on the original author's PC. It's now the real
  Blur Disabler addon.
- Small housekeeping: removed a stray editor backup, ignored `*~`/`__pycache__`, filled in [Screenshots.md](Screenshots.md).

---

## Quick start

1. **Pick a config** from the table below.
2. **Back up** your current `gameinfo.gi`.
3. **Replace** it with the one you picked:

| OS | Location |
|---|---|
| Windows | `C:\Program Files (x86)\Steam\steamapps\common\Deadlock\game\citadel\gameinfo.gi` |
| Linux | `~/.steam/steam/steamapps/common/Deadlock/game/citadel/gameinfo.gi` |

> Deadlock **overwrites `gameinfo.gi` on every major update**. Either re-copy it after patches, or use the
> [auto updater](auto%20updater) which re-downloads the config and re-applies your personal tweaks for you:
>
> ```
> python "auto updater/gameinfo_updater.py" update --config light
> ```

Video tutorial for the manual install (from the original project): https://youtu.be/TbjLbQVN2kE

---

## Configs

| Config | Best for | Look | Screenshots |
|---|---|---|---|
| [**adamescj's Light**](adamescj's%20light%20config/gameinfo.gi) | Decent PCs that want more FPS without the game looking worse | ★★★★☆ | — |
| [**Sqooky's default**](Sqooky's%20.gi/gameinfo.gi) | Most people. The original recommended config | ★★★☆☆ | [view](Sqooky's%20.gi/screenshots) |
| [Sqooky's Max FPS (`test_cfg`)](test_cfg/gameinfo.gi) | Max FPS. Experimental, lightly documented | ★★☆☆☆ | — |
| [Kaizuchaneru's Minimum Spec](kaizuchanerus%20minimum%20spec/gameinfo.gi) | Bad hardware. FPS above everything ([extreme low](kaizuchanerus%20minimum%20spec/gameinfoextremelow.gi) too) | ★☆☆☆☆ | [view](kaizuchanerus%20minimum%20spec/screenshots) |
| [Boot's Max FPS](boot's%20maxium%20fps%20config/gameinfo.gi) | Archived — no longer maintained | ★★☆☆☆ | [view](boot's%20maxium%20fps%20config/screenshots) |
| [Piggy's config](piggy's%20config%20%28comparatively%20outdated%29) | Archived — outdated | — | — |
| [Clean / stock](clean%20gameinfo.gi/gameinfo.gi) | **No performance changes.** Use it to repair a broken file | ★★★★★ | [view](clean%20gameinfo.gi/screenshots) |

Every config already supports mods (`citadel/addons`).

**Reference files:** [convars.txt](convars.txt) (every convar in the game), [cvarlist.txt](cvarlist.txt),
[cvars_we_can_modify.txt](cvars_we_can_modify.txt), [launch_options.txt](launch_options.txt),
[base_convars.txt](Sqooky's%20.gi/base_convars.txt) (just the convars Sqooky's default changes, to add by hand).

**Optional addons:** [Various Addons Relating to Performance](Various%20Addons%20Relating%20to%20Performance)
(blur disabler, optimized soul container, Sinner light fix, Vindicta scope downscale).

---

## Field of view

FOV is set with `r_aspectratio` (not `citadel_camera_hero_fov`). Higher = wider. Values are approximate:

| `r_aspectratio` | ≈ FOV |
|---|---|
| `1.75` | 80° |
| `2.15` | 90° |
| `2.2` | 91° — *light config* |
| `2.49` | 100° |
| `2.9` | ≈110° — *Sqooky's default* |

To use the game's own FOV, comment the line out.

---

## Editing a config

- **Find a setting:** open `gameinfo.gi` in a text editor and `Ctrl+F` the convar name.
- **Restore a setting to the game default:** comment it out by putting `//` at the start of the line.
- **Add a convar by hand:** search for `ConVars`, then paste it on a new line after the `{`.
- **A convar appears twice?** Only the last one counts. That's why the duplicates are tagged in this fork.

---

## FAQ / troubleshooting

| Problem | Fix |
|---|---|
| The config "broke" after a patch | The update overwrote `gameinfo.gi`. Re-copy it, or run the updater. |
| `FATAL ERROR: Unable to find child '...' in layout file 'panorama\layout\...'` | **This isn't the config.** A UI/HUD mod in `citadel/addons` is overriding a Panorama layout that a game update changed. Disable your UI mods one at a time (or move their `.vpk` out of `addons/`) until it stops, then update or remove that mod. |
| Mods stopped loading after updating | Your mod loader's search path got dropped. Re-add its `Game` line above `Game "citadel/addons"`, or use the updater, which keeps it. |
| Characters are dark in the shop / end screen portraits | `lb_enable_dynamic_lights` → `true` |
| Can't see heroes in the shop / end screen (boot's/Kaiz's) | Comment out `citadel_portrait_world_renderer_off` or set it to `false` |
| Buildings popping in and out | Comment out `r_farz` and `r_mapextents` *(already done in light)* |
| Can't see blast vent wind at range | Comment out `sc_fade_distance_scale_override` *(already done in light)* |
| Can't see boxes or trooper healthbars far away | `r_size_cull_threshold "0.7"`, or comment it and `sc_fade_distance_scale_override` out |
| Holes in Victor and Paige / Sinner's lights are little triangles | Comment out `sc_screen_size_lod_scale_override` *(already done in light)* |
| Can't see the Doorman ult indicator | `cl_ragdoll_limit "-1"` |
| Can't see Lash's ground slam (boot's/Kaiz's) | Comment out `r_drawdecals` or set it to `true` |
| Can't read in-world text (soul pickups, buffs) | Comment out or raise `citadel_in_world_item_panel_dpi` |
| Camera dips when Rem/Venator aim down sights | `citadel_camera_use_vmdl_flatten_vertical "true"` |
| Rainbow puddles, rank display, statues or urn / weird Billy skin (Kaiz's) | `r_citadel_npr_force_solid_outline "false"` |
| Clothes don't move sometimes (Kaiz's) | `cloth_update "1"` |
| McGinnis' wall turns into a tombstone for a second | Comment out `CMTAtlasHeight` and `CMTAtlasWidth` under `SceneEffects` |
| Upscaler looks blurry | In-game, use the Quality preset, turn motion blur off, and try a little sharpening (`r_citadel_fsr2_sharpness` in `cfg/video.txt` for FSR). None of the configs change upscaling. |

---

## Translations (original project)

[Español](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_spanish.md) ·
[Русский](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_russian.md) ·
[Português](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_portuguese.md) ·
[Български](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_bulgarian.md) ·
[Italiano](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_italian.md) ·
[Français](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_french.md) ·
[中文](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_chinese.md) ·
[Українська](https://github.com/Sqooky/OptimizationLock/blob/main/translations/README_ukrainian.md)

These translate the original README, not this fork's additions.

---

## Credits

- **[Sqooky](https://github.com/Sqooky)** — creator and maintainer of OptimizationLock and its default config. [ko-fi](https://ko-fi.com/sqooky)
- **boot**, **Kaizuchaneru**, **Piggy** — their configs, included here
- **Maihdenless** — started the original OptimisationLock and its Discord
- **Jasper**, **Abdalla**, **Artemon121**, **Kunet**, **Kin** and many others — research, tooling, benchmarks
- The translators and donors listed in [ORIGINAL_README.md](ORIGINAL_README.md) and at the top of each `gameinfo.gi`

Fork, light config and fixes by **[adamescj](https://github.com/adamescj)**.

## License

Same as the original project: see [LICENSE](LICENSE).
