# EVAC: ASHFALL

A self-contained **3D** cooperative zombie evacuation game for desktop and mobile browsers. Open **index.html** (or the release's **game-evac.html**) in a modern browser. No build step or game server is required for solo play.

**Mission:** find 40 civilians in Greyhaven, escort them to the helicopter LZ, and evacuate your operators before the 24-hour destruction deadline. Select a rifleman, medic or engineer loadout. Two Hummers, civilian vehicles, turrets, explosive infected, night aggression, airstrikes and supply drops are playable.

The default view is a real-time WebGL perspective world with modeled buildings, vehicles, hazmat operators, infected, helicopters and fog. A 2D tactical map is available with M. WebGL support is required. All renderer code is embedded in the HTML.

## Controls

| Action | Desktop | Touch |
|---|---|---|
| Move / drive | WASD or arrows | Left thumbstick |
| Rotate camera | Right mouse drag or Q / Z | LOOK buttons |
| Camera zoom | Mouse wheel | Fixed follow distance |
| Aim / fire | Mouse / hold left click | Tap or hold world; FIRE button |
| Search, recruit, enter or exit | E | INTERACT |
| Toggle Hummer driver / turret | T | TURRET |
| Throw red airstrike flare | F | AIRSTRIKE |
| Supply helicopter | C | SUPPLIES |
| Heal / engineer repair | H / R | HEAL / REPAIR |
| Evacuate operator at LZ | X | EXTRACT |
| Full map | M | MAP |
| Pause / networking / save | Escape | Top-right menu |

Stand beside an unsearched building and interact. Search opens its interior, collects ammunition and recruits any occupants. Blue survivors follow on foot; drive slowly enough to keep them safe. Green checked roofs have been searched. Civilians reaching the green LZ enter a sequential helicopter queue: exactly 120 **game seconds** per person. Winching continues without an operator present; the LZ is not invulnerable. Exit the vehicle before evacuating yourself.

The default 24-minute session uses a clock 60 times real time. A real-time 24-hour option is available. At default speed each survivor takes two real seconds; in real-time mode each takes two real minutes. Airstrike warning and supply flight delays use real seconds in both modes.

## Co-op: 1–8 real players

1. Host deploys, opens the menu and selects **HOST: CREATE INVITATION**.
2. Host sends the outgoing text to one teammate using their own messaging app.
3. Teammate chooses a loadout, selects **JOIN CO-OP**, pastes the invitation, and selects **JOIN: CREATE ANSWER**.
4. Teammate sends their outgoing answer back. Host pastes it and selects **HOST: ACCEPT ANSWER**.
5. Repeat with a fresh invitation for each teammate. The host simulates all mission rules and sends shared state ten times per second.

WebRTC requires a modern browser. STUN is used for public address discovery; restrictive NAT/firewalls may require your own TURN relay URL and credentials in the operations panel. There is no hosted matchmaking, host migration, reconnect identity recovery or automatic relay service. Prefer keeping the host visible; browsers may throttle background tabs. Solo pauses in the operations menu; connected co-op continues. Disconnected operators remain vulnerable in the city. A saved mission resumes as solo with the original host; saves do not preserve network connections.

## Historical map status

Downtown centerline geometry was manually traced and scaled from the city-approved **2012–2041 DDA plan**, Map 1, printed page 7, dated May 3, 2012. The diagonal road/rail corridor, major north-south avenues, central boulevard, and intersecting neighborhood streets follow that source. All gameplay street, city, church and business labels are fictional.

**This release is an alpha, not a verified reconstruction of the entire 2012 city.** The source covers downtown and adjacent blocks, not all municipal streets. Peripheral streets, building footprints, interiors and occupant distribution are synthesized for gameplay. See [spec.md](spec.md) for the exact scope and remaining historical verification work.

## Validation / release

Run `node tests/rules.cjs`. Browser and WebRTC smoke tests: install `playwright` with Chromium and run `node tests/browser.cjs`. GitHub Actions validates both suites and publishes `game-evac.html`, `spec.md`, `source.zip`, and `SHA256SUMS.txt` as release assets. See [md/PROMPT.md](md/PROMPT.md) for the original request.
