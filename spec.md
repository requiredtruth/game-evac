# ASHFALL · Evacuation Protocol — v0.3.0 specification

## Product and delivery

A playable 3D cooperative browser evacuation game for one to eight human players. Repository: requiredtruth/game-evac, branch main. One embedded HTML is released as game-evac.html alongside this spec.md, source.zip and SHA256SUMS.txt. Sources are src/shell.html, src/game.js and src/renderer.js. Build with node scripts/build.cjs; generated index.html must match sources. There are no runtime libraries, image downloads or server requirements for solo play.

This version is a substantial presentation and gameplay upgrade. It remains an indie procedural-art alpha, not a claim of AAA content, exact historical reconstruction, or universal network/device certification.

## Scenario and mission rules

The fictional town of Greyhaven is quarantined. Forty civilians are hidden in distinct seeded buildings. Operators wear hazmat suits and start in two four-seat, turret-equipped Hummers. Investigate homes, businesses and chapels, recruit civilians, escort or transport them to a central helicopter LZ, then evacuate the operators.

Start time is 06:00. The city is destroyed after 86,400 simulation seconds. Every operator and unrescued civilian left inside at that deadline dies, including vehicle occupants. Each civilian needs exactly 120 simulation seconds in the helicopter queue; only one civilian progresses at a time. The queue continues without an operator present but is vulnerable to enemies and air support. Success requires 40/40 civilians and every rostered operator to escape. Once no active operator remains, the mission ends with its actual results. No respawns or replacement civilians.

Clock modes: 60× default (24 real minutes; two real seconds per winch), or 1× (24 real hours; two real minutes per winch). Movement, weapon/reload delays, supply approach and airstrike warnings use real gameplay seconds. Fixed simulation step: 50 ms. Frame stalls are clamped to 100 ms; time is simulated, not a wall-clock countdown. Suspended/background browsers can slow the operation. No offline progression.

## World and historical accuracy

World size: 2,500 × 2,700 units. Ground x/y in state maps to rendered world x/z, with modeled height on world y. Roads, structures, foliage, vehicles and actors have actual perspective geometry and depth testing; a separate 2D tactical overview is optional.

Historical reference: City of Hudsonville approved 2012–2041 Downtown Development Authority Development and Tax Increment Financing Plan, Map 1, printed page 7 (PDF page 9), updated 5/3/12. Source: https://evogov.s3.amazonaws.com/media/80/media/49874.pdf

Downtown centerlines are manually traced and simplified from this source. The diagonal road/rail corridor, major avenues, cross streets and nearby courts retain relative positions. All in-game place names are fictional. **The complete municipal 2012 street network has not been verified.** Peripheral streets and all building footprints/occupants are synthesized. Exact citywide fidelity requires a dated citywide street/parcel dataset, digitization, scale validation and comparison with 2012 aerials. This limitation is preserved in the UI and release notes.

## Buildings and civilians

Unsearched buildings block movement and bullets. Interacting within 70 units of an exterior edge searches/opens a building, awards its one-time ammunition cache (25–54 rounds), and reveals its occupant if present. Houses, four fictional shop brands and two fictional chapel brands use distinct labels. Blue distress markers identify the nearest hidden civilian's building, providing a useful objective without revealing occupants outside it. Search is an immediate interaction, not a timed lockpick sequence.

Searched structures become traversable cutaway interiors with floors, low walls and simple furniture. There are no separate interior scenes, upper floors or structural destruction during normal play. Low cutaway walls are visual, not collision obstacles. Revealed civilians have 100 health, follow their recruiting operator, and navigate around intact structures using a host-side A* grid (24-unit cells, diagonals with corner checks). Path caches and a spatial collision index remain outside saves/network snapshots. Infected use the same obstacle routing. Grid planning is bounded; this is not a full navigation-mesh crowd simulation.

When the leader enters a vehicle and a following civilian is within 85 units, a free seat is used automatically. Seat counts include living operators and other civilians; no vehicle exceeds four occupants. Riding civilians share the vehicle position, are protected by the hull, and automatically join the helicopter queue when the vehicle reaches the LZ. If their leader exits nearby, they dismount and follow. A destroyed transport ejects and injures civilians for 30 health; survivors wait for help. If a leader dies or evacuates before extraction, following civilians wait for another operator to recruit them.

## Operators, loadouts and resources

| Role | Total rounds | Flares | Medkits | Spare parts |
|---|---:|---:|---:|---:|
| Rifleman | 180 | 2 | 2 | 0 |
| Medic | 120 | 1 | 6 | 0 |
| Engineer | 150 | 3 | 2 | 3 |

Health: 100. A medkit restores up to 60 health. Medic treatment chooses the most injured nearby living operator or revealed civilian within 100 units, including self. Other roles treat themselves. Engineers spend one spare part to restore up to 160 health to a nearby living damaged vehicle; destroyed vehicles cannot be repaired.

Rifle magazine: 30 rounds. Reserve is total remaining ammunition minus loaded rounds; reload does not create ammunition. Reload takes 1.8 real gameplay seconds, disallows shots, and automatically begins if the trigger is held on an empty magazine with reserves available. No ammunition is consumed during reload. Rifle damage: 40, interval 0.22 seconds, range 420 units. Mounted gun: 65 damage, 0.12-second interval, 560 range, directly consumes total ammunition without magazine reload. Intact building footprints block hitscan; the closest zombie before the wall takes damage. Friendly bullets do not hit civilians/operators. No vertical hitboxes, projectile ballistics or headshots.

Foot speed: 100 units/second. Shift sprint: 155 units/second while stamina exceeds five. Stamina capacity 100, drain 24/second, recovery 16/second when not sprinting; sprinting prevents shooting. Touch has normal movement and a separate look zone; no touch sprint control yet. Kill counts appear in the final report.

## Vehicles and infected

Two Hummers: 450 health, four seats, mounted guns. E enters/exits; T selects driver/turret mode. The first operator takes the driver role, later operators ride. Taking the driver role demotes other operators to passenger/turret roles. Civilian cars: 190 health, four seats, no gun. Twenty vehicles total. Vehicles accelerate and coast toward input direction, with smoothed heading changes; maximum target speed 260 units/second. This is arcade ground-plane handling, not suspension or rigid-body vehicle physics. Collision slides against intact buildings. Operator exits search eight nearby safe positions. Destroyed vehicles eject/injure operators for 30 health.

Infected: 60 health. Day speed 36, detection range 370; night speed 70, detection range 800. Night is 20:00–05:59. They pursue the nearest active operator or exposed civilian and route around obstacles. One attack per real second: 5 person / 7 vehicle damage in daylight, 9 person / 12 vehicle at night. Approximately 15% are orange explosive infected. Moving vehicle impacts kill zombies; ordinary impacts cost two hull health. Explosive runovers cause 90-damage blasts within 90 units. Other explosive deaths cause 65 damage within 85 units. Explosions harm vehicles, exposed operators and revealed civilians. Around 90 infected start outside the immediate LZ safety radius; deaths gradually replenish toward 100. AI has no hearing propagation or strategic flanking.

## Air support

Red flare: consumes a flare and lands 200 units toward the aim point. A red 150-unit warning ring lasts six real gameplay seconds, then a strike deals 240 area damage. Friendly/civilian blast damage applies. Supply helicopter: ten real gameplay seconds to approach, a crate with 160 rounds, two medkits, one flare, and one engineer spare part when collected by an engineer. Collection range: 65 units. Per-operator resupply cooldown: fifteen simulation minutes. Helicopters have modeled hulls, tails, spinning rotors, skids and an evacuation cable. No player-controlled aircraft.

## Presentation, interaction and performance

Perspective follow camera; right-drag or Q/Z rotates, mouse wheel adjusts distance. Mobile left thumbstick, world aim/fire, right drag-to-look zone, view buttons and contextual actions. Camera-relative movement and screen-to-ground aiming. No pointer lock required. Desktop, portrait and landscape layouts are supported. Fullscreen is user-invoked when available.

Interface: deployment screen, mission briefing, role and clock selectors, persistent current objective, projected distress/LZ waypoint, local tactical radar, squad status, hull/health/stamina/ammo readouts, hit confirmations, damage vignette, event feed, first-run hint and after-action report. Initial tutorial can be dismissed and remembered. Controls are DOM buttons; world visuals are not accessible through a nonvisual screen reader. No controller bindings.

Art: procedural rounded character meshes with articulated limbs and equipment, cylindrical tires/hubs, vehicle bodywork/glazing/lights, porches, trim, chimneys, shop awnings, chapel crosses, fences, foliage, forward-base tent/barriers/crates and lamp posts. Procedural directional/hemisphere-style shading, fine world-surface grain, tone mapping, color correction, gradient sky/clouds, fog, real depth-texture shadow mapping with four-tap filtering where supported. Cutaway interiors remain stylized. No externally licensed/downloaded artwork.

Graphics settings: Balanced and High use shadows when WEBGL_depth_texture exists; Performance disables shadows and lowers resolution. Rendering is capped near 30 FPS; pixel ratio caps are 1.15 Balanced, 1.5 High, 0.8 Performance. Static geometry is cached and dynamic geometry is streamed in batches. Resize occurs only when viewport changes. Distant actors are culled. Render work is larger than the prior block prototype; low-end physical-device performance is not yet certified.

Audio is locally synthesized through Web Audio: ambient wind, engine hum, footsteps, gunfire, explosion and feedback tones. Starts after a deployment/join gesture; mute preference persists locally. No voice dialogue, licensed music, audio assets or microphone permission.

## Co-op, persistence and validation

The host owns all rules and state. Seven guests connect through reliable ordered WebRTC data channels using manually exchanged base64 SDP invitation/answer text, one invitation per guest. Guests select their own loadouts. Host validates finite/clamped movement/aim, action names, resources, seat capacity and mission state. Inputs are sent up to 20 Hz; shared snapshots up to 10 Hz, skipping congested channels. New combat effects are propagated to clients. No client prediction or interpolation; high latency can still feel uneven.

STUN address discovery is configured; optional user TURN URL/credentials support restrictive networks. No hosted matchmaking, managed TURN, host migration or reconnect identity is supplied. Keep the host tab active. Disconnected operators stop after input timeout, remain vulnerable and count toward the result. Solo pauses in the operations menu; connected co-op continues. Up to eight rostered operators, including dead/disconnected operators; no slot replacement during a mission.

Host localStorage saves: manual and approximately once per real minute. JSON export/import from the operations menu, with size/shape/coordinate checks. Save version 2 migrates version 1 magazine/stamina/parts fields. Resume/import returns to solo and keeps the original host; network connections and guests are omitted. Browser storage restrictions can prevent persistence, so exported files are useful. Imported missions are not a competitive/anti-cheat boundary.

Automated rule tests exercise deterministic population, building search, winch timing/order, destruction, extraction, loadouts/repairs, damage/support, night speed, survivor transport/capacity, obstacle routing, bullet occlusion, reload accounting, medic treatment, stamina and save migration/rejection. Browser checks cover actual WebGL frames and errors, deployment, camera, on-foot view, map, save/settings, portrait/landscape controls and eight local human browser peers. GPU drawing is suspended for already-verified peers during the network phase to bound software-rendered CI. These tests do not establish WAN NAT connectivity, sustained 24-hour sessions, physical Android/iOS performance, accessibility certification or production anti-cheat.
