# Apex: Harran — design brief

Paste this into your coding agent (Claude Code or Codex) together with the original Melty Publish prompt. The Melty token lives in that prompt, so it is deliberately not repeated here. The design questions are already answered, so the agent should skip the interview and start from "Look before designing".

## One sentence
Play as an Apex Legend in Dying Light's Harran, with Apex abilities and weapons, solo.

## Games
| Game | Slug | Role | What it brings |
|---|---|---|---|
| Dying Light | dying-light | primary (host) | The world, the infected, parkour, the player |
| Apex Legends | apex-legends | companion | Legends, abilities, weapons: read from the player's installed copy via `{game:apex-legends}`, never shipped, never launched, never modded |

Do not use role "secondary" and do not ship look-alike stand-ins. Apex content must come from the player's own install.

## Player experience
1. Launch via Melty, and a **Legend select** screen appears (2 Legends in v1).
2. Pick a Legend and spawn straight into Harran.
3. Use the Legend's passive, tactical and ultimate abilities and 2 Apex guns against the infected.

## Scope
- **Solo only.** Do not set `recipe.multiplayer`.
- **Version 1 (Legend-heavy):** 2 Legends with the full ability set (passive, tactical, ultimate), plus 2 guns.
- **Out of scope:** co-op, extra Legends, shields and armor tiers, ring and arena mode.

## Draft design sheets (source of truth; refine after reading the games)

### legends.json
| id | name | passive | tactical | ultimate | notes |
|---|---|---|---|---|---|
| wraith | Wraith | Voices from the Void (danger warning) | Into the Void (brief invulnerable phase) | Dimensional Rift (linked portals) | Suggested; maps well to parkour movement |
| pathfinder | Pathfinder | Insider Knowledge (scan beacons) | Grappling Hook | Zipline Gun | Suggested; grapple interacts with Dying Light's climbing |

Columns to fill per row: Apex asset source path under `{game:apex-legends}`, Dying Light hook used, cooldown, duration, range, UI icon, sound, verified in game (checkbox).

### weapons.json
| id | name | damage | clip | fire rate | ammo | hook into Dying Light |
|---|---|---|---|---|---|---|
| wingman | Wingman | TBD | TBD | TBD | TBD | TBD |
| r301 | R-301 Carbine | TBD | TBD | TBD | TBD | TBD |

### systems.json (hooks into the host game)
Legend select UI, spawn handler, ability input bindings, cooldown/HUD, weapon pickup/equip, damage to infected, reading Apex assets at runtime.

## Order of work
1. Connect to Melty. Call `search_mashups` (dying-light + apex-legends), `mashup_info` on the closest launchable one, and `game_info` on both games.
2. Find both install folders. Set up universal-modder and identify Dying Light's engine and modding route.
3. Check Apex's installed files: can the abilities, models, animations and sounds be read at runtime? If not, build the closest version that works (Dying Light's own character with the Legends' abilities) and say so.
4. Preflight the sheets (unfilled cells, unresolved references), then build and test in the running game.
5. Run `inspect_package`, `validate_recipe` and `one_click_check`. Dying Light has no loader that Melty installs itself, so bundle the loader if its license allows. If a player would have to install anything by hand, tell the user before building.
6. Draft the listing (title, tagline, description), ask the user for credits, license and remix choice, capture a real screenshot of the running build, then submit. Ask before calling `publish`.

## Known risks
- Melty may not publish this unless the loader gets bundled and the release passes `one_click_check`.
- Apex's Legend models and animations may not be readable from its install, so v1 may fall back to Dying Light's character with Apex abilities.
- Apex has Easy Anti-Cheat, but as a companion its files are only read, so there is no account risk per `game_info`. Never work around anti-cheat.
