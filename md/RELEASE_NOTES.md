# EVAC: ASHFALL — 3D alpha

Download **game-evac.html** and open it in a modern desktop or mobile browser. **spec.md** documents all rules, defaults and limitations; **source.zip** contains the source and tests.

- **3D conversion:** third-person camera, camera-relative movement, ground-ray aiming, Q/Z or right-drag rotation, mobile LOOK buttons, lighting and fog. WebGL required.
- Solo and up to eight real human operators through direct WebRTC invitation exchange.
- Forty hidden civilians, a 24-hour simulation clock, sequential two-game-minute helicopter evacuation and the city destruction deadline.
- Three loadouts, mobile controls, two turret-equipped Hummers, civilian cars, explosive infected, night aggression, loot, airstrikes, supply helicopters and local solo saves.
- Default 24-minute game or real-time 24-hour mode.

Historical map limitation: the downtown road geometry is manually traced from an approved map dated May 3, 2012. This is **not an exact full-city reconstruction**: peripheral blocks and all buildings are synthesized. All in-game place names are fictional. Gameplay now uses a true WebGL 3D perspective world with modeled characters, vehicles, buildings, helicopters and cutaway interiors; M opens the 2D tactical map. Direct-pursuit AI and ground-plane movement remain.

Co-op needs exchanged invitation/answer text and a reachable host. Some internet networks need a user-provided TURN relay. No managed matchmaking, host migration, reconnection identity or survivor car seating. Keep the host tab active. Save/resume is host-only and resumes solo. Full details and remaining work are in spec.md.
