# ASHFALL · Evacuation Protocol

A standalone **3D cooperative evacuation game** for 1–8 players. Find forty civilians in a quarantined town, bring them home, and get your squad out before containment destroys the city.

Download **game-evac.html** from [Releases](https://github.com/requiredtruth/game-evac/releases/latest) and open it in a modern browser with WebGL. The whole game is embedded in one file; solo play needs no server or network connection.

## The operation

Choose Rifleman, Medic, or Engineer. Deploy in two turret-equipped Hummers. Follow blue distress markers to homes, shops and chapels. Exit, search, and escort occupants to the marked helicopter LZ. Civilians can also board available vehicle seats. Each survivor takes exactly two **game minutes** to evacuate, sequentially. The default session compresses 24 game hours into 24 real minutes; a true 24-hour mode is available.

Explosive infected damage vehicles when hit. Night brings faster, more aggressive infected. Find ammunition, call supply helicopters, use red flares for airstrikes, and watch your magazine, health, stamina and vehicle hull. Every operator must evacuate before the deadline. A clean victory requires all forty civilians and the whole squad.

## Controls

| Action | Desktop | Touch |
|---|---|---|
| Move / drive | WASD or arrows | Left thumbstick |
| Aim / fire | Mouse / hold left click | Tap world / FIRE |
| Rotate camera | Right-drag or Q / Z | Right drag zone / VIEW buttons |
| Camera distance | Mouse wheel | Follow camera |
| Interact / enter / exit / search | E | Contextual gold button |
| Driver / Hummer turret | T | TURRET |
| Sprint on foot | Shift | Desktop sprint only |
| Reload | R | RELOAD |
| Red-flare airstrike | F | AIRSTRIKE |
| Supply drop | C | SUPPLY |
| Heal / medic treatment | H | HEAL |
| Engineer repair | B | REPAIR |
| Operator extraction at LZ | X | EXTRACT |
| Tactical map | M | MAP |
| Settings / save / networking | Escape | Top-right menu |

Medics heal the most injured nearby operator or revealed civilian. Engineers carry three spare parts; each repairs 160 vehicle health. Supply crates replenish ammunition, medkits and flares, plus an engineer spare part. Rifle magazines hold thirty rounds; reloads take 1.8 real gameplay seconds. Mounted guns draw directly from ammunition reserves. Intact buildings block bullets.

## Squad connection

Host deploys, opens the operations menu, and creates an invitation. A teammate chooses their loadout, selects JOIN SQUAD, pastes the invitation, and creates an answer. The host pastes and accepts that answer. Repeat with a fresh invitation per teammate, up to seven guests.

This is direct WebRTC, with no hosted lobby or managed relay. Some internet NAT/firewall combinations require a user-supplied TURN relay. Keep the host tab active. No host migration or reconnect identity recovery is included. Disconnected operators remain vulnerable. Solo pauses in the menu; connected co-op continues.

## Presentation and settings

Procedural 3D models, rounded characters, cylindrical wheels, architectural details, trees, perimeter lights, a forward base, warm directional lighting, real shadow maps, distance haze, procedural sky/clouds, ambient wind, gunfire, footsteps and engine sound. No downloaded art or external game engine. Balanced and High include shadows when the browser supports depth textures; Performance disables shadows and reduces resolution. Portrait and landscape layouts include radar, objectives, contextual interactions, squad status and a rescue debrief. Audio starts after deployment/join and can be muted.

## Saves

Host auto-saves about once per real minute and may save manually. Export/import JSON mission files in the operations menu. v0.1/v0.2 saves migrate to the new magazine/stamina/repair fields. Resume/import is solo, retaining the original host; live network connections cannot be restored.

## Historical map accuracy

Downtown roads are manually traced from the approved Hudsonville **2012–2041 DDA plan**, Map 1, dated May 3, 2012. All gameplay place names are fictional. **This is not a verified full-city 2012 reconstruction**: peripheral roads and building footprints remain approximations. See [spec.md](spec.md).

## Source and validation

No dependency installation is needed to build or run rule tests:

```sh
node scripts/build.cjs
node scripts/build.cjs --check
node tests/rules.cjs
```

Edit `src/shell.html`, `src/game.js`, and `src/renderer.js`, then rebuild `index.html`. Browser tests use Playwright with Chromium: `node tests/browser.cjs`. CI verifies source/build consistency, game rules, desktop/mobile 3D rendering, settings, saving, and eight real local WebRTC connections before publishing HTML, spec, source ZIP and checksums. See [md/CHANGELOG.md](md/CHANGELOG.md) and [md/QA.md](md/QA.md).
