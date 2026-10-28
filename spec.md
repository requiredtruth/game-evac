# EVAC: ASHFALL — specification v0.2.0

## Delivery and status

Repository: requiredtruth/game-evac. Branch: main. Release series: v0.2.0. Deliverables: game-evac.html (the standalone index.html renamed for download), spec.md, source.zip, SHA256SUMS.txt. This is a playable 3D alpha. No external art or runtime JavaScript dependencies. Desktop and mobile WebGL perspective client; direct WebRTC co-op. All unspecified design decisions below are implementation defaults.

## Scenario and completion

1–8 human operators enter fictional Greyhaven in hazmat suits. Forty civilians are deterministically placed inside different houses, shops and chapels. Find and escort them to a single central helicopter landing zone. Each civilian occupies the helicopter queue for exactly 120 simulation seconds, sequentially. Start time is 06:00. The city is destroyed after 86,400 simulation seconds. Unextracted operators and remaining civilians die at that deadline, including occupants of vehicles. Success requires all 40 civilians and every rostered operator to escape. An early departure or squad death ends the mission when no active operators remain, with the actual rescue total. No respawns, friendly player bullet damage or scoring bonuses. Airstrike and explosion damage affects civilians and operators.

Clock choices: default 60× time (24 real minutes), or 1× (24 real hours). The deadline and civilian winch duration always share the same simulation clock. Movement, combat cooldowns, helicopter approach delays and airstrike warning use real elapsed gameplay seconds. Fixed simulation step: 50 ms. Long browser stalls are clamped to 100 ms per frame: this is a simulation clock, not a wall-clock deadline. Background or suspended browsers can slow the operation. No offline progression.

## World and historical fidelity

Primary reference: City of Hudsonville, approved 2012–2041 Downtown Development Authority Development and Tax Increment Financing Plan, Map 1, printed page 7 (PDF page 9), map last updated 5/3/12.

Source URL: https://evogov.s3.amazonaws.com/media/80/media/49874.pdf

World coordinates are a 2,500 × 2,700 unit rectangle with north at the top. Centerlines are manually traced, scaled and simplified from the historical map; no imagery is embedded. The road/rail diagonal crosses southwest to northeast. The creek, downtown cross streets, avenues, and east-side courts follow visible relative positions. All rendered names are fictional. The main diagonal is Cinder Drive; major avenues are Iron and Westwatch; the city is Greyhaven; the chapel is Beacon Chapel; shops are Morrow Market. Source names appear only in research documentation.

**Unfulfilled historical precision requirement:** this alpha is not the complete city layout as it existed in 2012. The available verified map covers downtown and adjacent neighborhoods. Peripheral streets and buildings are approximations. Building footprints are seeded synthetic placements, not 2012 parcels. To claim exact full-city fidelity, obtain a licensed or public-domain citywide 2012 road/parcel dataset, digitize all streets at a known scale, compare intersections and removed/new streets with 2012 aerials, and replace synthetic footprints. Do not market this alpha as an exact 2012 replica.

## Survivors, buildings and supplies

Buildings begin solid and unsearched. Interact within 70 units of an exterior edge to open/search; searched footprints become traversable open-roof 3D interiors, gain a check mark, and cannot be looted again. There are no separate interior scenes or floors. Each building contains 25–54 rounds. Forty seeded buildings each contain one civilian. Search recruits its occupant; civilian movement starts at the searching operator's location to avoid trapping them inside walls. Following civilians travel on foot, follow faster when separated, and queue automatically within 105 units of the LZ. If their leader dies or evacuates before they reach the zone, civilians wait for another operator to recruit them. They have 100 health and can die. There is no survivor vehicle seating or pathfinding navigation mesh; escort them around unsearched buildings. Dead civilians cannot be replaced.

## Loadouts

| Role | Rounds | Red flares | Medkits | Specialty |
|---|---:|---:|---:|---|
| Rifleman | 180 | 2 | 2 | Largest ammunition reserve |
| Medic | 120 | 1 | 6 | Extra self-healing kits |
| Engineer | 150 | 3 | 2 | Repair a nearby living vehicle for 25 ammo salvage |

Operators have 100 health. A medkit restores up to 60 health, never above 100. Engineer repairs restore up to 160 vehicle health. Ammunition is a shared abstract rifle/turret ammunition type. No reload/magazine mechanic; spent rounds decrement reserve immediately. Rifle shots deal 40 damage at 0.22-second intervals with 420-unit range. Turret shots deal 65 damage at 0.12-second intervals and 560-unit range. Shots use aim-directed hitscan against the nearest zombie on the ray. Visual tracers show shots. Buildings do not block shots in this alpha.

## Vehicles and combat

Two Hummers start near the LZ with four seats, 450 health and a mounted machine gun. E enters/exits. The first occupant drives; other occupants ride, with Hummer occupants able to select the turret. T switches driver/turret position, handing other occupants the passenger/turret role when a driver takes control. Vehicles stop without driver input. Civilian cars have four seats, 190 health and no turret. Twenty total vehicles. Speed: 260 units/second; operator speed: 100. Vehicle and foot movement slide along building obstacles and remain within the world bounds. Foot exits search eight nearby positions for safety.

Zombies have 60 health. Day speed: 36 units/second; night speed: 70. Day detection: 370 units; night: 800. Night is 20:00–05:59. Zombies pursue the nearest active operator or revealed civilian. Attacks occur once per real second, causing 5 day / 9 night person damage, or 7 day / 12 night vehicle damage. Around 15% are orange explosive infected. Vehicle impacts kill zombies; ordinary impacts cost 2 vehicle health, explosive impacts cause a 90-unit-radius, 90-damage explosion. Other explosive deaths cause an 85-unit, 65-damage blast. Destroyed vehicles eject and injure occupants for 30 damage on simulation update. Roughly 100 zombies are maintained; dead enemies slowly replenish. Zombies use direct pursuit with obstacle sliding, not full route planning.

## Air support

A red flare consumes one flare and lands 200 units along the aim direction. A 150-unit red warning ring persists for six real seconds, then the airstrike deals 240 damage inside the ring to zombies, vehicles, exposed operators and revealed civilians. Flares cannot be thrown without stock. Helicopter supply call: 10 real seconds to approach, one crate containing 160 rounds, two medkits and one flare. Collect within 65 units. Each operator has a 15-game-minute resupply cooldown. Helicopter and rotor are procedural silhouettes.

## Multiplayer architecture and networking limits

A single host owns the state and simulates all rules. Up to seven joining browser peers connect via reliable ordered WebRTC data channels. Signaling is manual base64 JSON SDP exchange with ICE gathering. Each host invitation is for one teammate; create the next invitation after accepting the prior answer. Each joiner selects their own loadout. Host validates finite input coordinates, clamps movement, restricts action names, and checks in-game resources. Client input packets are sent up to 20 times/second. State snapshots are sent up to 10 times/second; congested channels skip snapshots. Clients render shared mission state without prediction. All players share rescue counts, clock, enemies, vehicles, buildings, drops and damage. There is no multiplayer AI replacing absent humans.

Google STUN is configured for address discovery. Optional user-supplied TURN URL/username/password supports restrictive networks. Relay credentials remain in page memory and are not put in save data or invitation text. No managed relay or lobby service is included. Network reachability across arbitrary consumer NATs is not guaranteed without TURN. Input timeout: two real simulation seconds. Disconnected operators stop, remain vulnerable, and still count toward final results. No reconnect identity recovery or host migration. Seven remote peers plus host is the supported cap; the eighth remote admission is rejected.

## UI, persistence and accessibility

Responsive full-screen WebGL world with Canvas HUD/map overlay, high-contrast tactical HUD, loadout and pace selectors, mission briefing, visible button equivalents for keyboard actions, mobile thumbstick and world aiming, full-map toggle, visible supply and airstrike warnings, game-over result. Menu and network text are DOM elements; combat graphics are WebGL and map graphics are Canvas and do not support nonvisual screen-reader play. No audio, account, analytics or paid content. No third-party art is used. No gamepad support.

Solo operations menu pauses the mission; connected co-op continues. Host may save JSON to localStorage, including clock, buildings, civilians, inventories and enemies. Resume restores only original host operator and continues as solo; clients are omitted. No file export, cloud save or backward save migration. Save compatibility version: 1. Closing/reloading without saving discards the mission. Modern Chrome, Edge, Firefox and Safari with WebGL/Canvas/WebRTC are the target; real-device/iOS coverage remains unverified.

## Validation and known limitations

Actual embedded simulation code is tested by `node tests/rules.cjs`: deterministic 40-person placement, search/loot, sequential 120-second extraction, destruction deadline, operator extraction, win condition, vehicle entry/turret/exit, airstrike, supply cooldown/collection, repair and night speed. Browser smoke tests cover deployment, desktop/mobile rendering, local save and real WebRTC host/client state propagation. Automated local peers cannot prove connectivity through unrelated internet NATs or sustained 24-hour play. GitHub release workflow runs the rule and browser suites before publishing assets.

Remaining scope: complete citywide 2012 map verification; richer interiors, route-finding and art; host migration/reconnection; managed relay/matchmaking; survivor transport in cars; long-session and physical-device performance testing. These limitations must remain explicit in release notes.

## 3D rendering and camera (v0.2.0)

Default gameplay uses a perspective third-person camera in a genuinely three-dimensional WebGL scene. World state x/y positions become world x/z ground coordinates; modeled height is world y. Buildings have elevated walls, gabled roofs, windows and doors, with raised chapel crosses and shop awnings. Searched buildings reveal floors and low cutaway walls with simple interior props so operators can enter and see inside. Hazmat operators, civilians and infected are articulated box meshes; Hummers/cars have bodywork, windows, wheels and elevated turrets. Helicopters have hulls, tails, animated rotors and landing skids. Road surfaces, sidewalks, rail corridor, yard trees, LZ markings, crates, explosions and tracers are 3D meshes. Art is deliberately low-poly and procedural. World geometry uses depth testing, directional diffuse light, ambient light, day/night tint and distance fog. No external engine or asset CDN is needed.

Camera follows behind and above the operator/vehicle. Right-drag or Q/Z rotates it; mouse wheel adjusts distance (130–440 world units). Mobile LOOK buttons rotate in 22.5-degree increments. WASD/thumbstick movement is relative to camera yaw. A screen-to-ground perspective ray determines the aiming point; map view pauses firing. M toggles the original 2D tactical overview without changing the mission state. WebGL failure displays a visible requirement/error message; there is no automatic 2D gameplay fallback.

Static world meshes are cached and rebuilt when building search state changes; dynamic actors share a streamed vertex buffer. Rendering caps at approximately 30 frames/second and pixel ratio 1.25 to reduce mobile GPU load, while simulation remains fixed-step. Distant actors are culled. Browser smoke tests require actual WebGL frames and zero GL errors, capture 3D desktop/mobile screenshots, exercise camera controls, and retain all eight-human co-op checks. Art, world movement and combat remain constrained to the ground plane: no jumping, multistory navigation, slopes, ragdolls or vehicle suspension physics. Building bullet occlusion, full pathfinding and citywide historical fidelity remain the limitations stated above.
