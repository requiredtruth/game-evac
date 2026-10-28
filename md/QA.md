# Quality checks

Run `node scripts/build.cjs --check` and `node tests/rules.cjs` before a release. Run Playwright's `node tests/browser.cjs` with installed Chromium for browser coverage. CI gates publication on all suites and stores screenshots for visual review.

| Area | Acceptance coverage |
|---|---|
| Mission | 40 seeded civilians, exact sequential 120-second winch, deadline deaths and all-operator victory |
| Rescue | Building search/loot, following, transport, capacity and LZ disembarkation |
| Navigation | A* route reaches a target around an intact structure |
| Combat | Wall occlusion, magazines, reload timing/accounting, night speed and air support |
| Roles | Spare-part repairs and medic teammate treatment |
| Persistence | Old-save migration, malformed coordinates rejected, browser save and export/import implementation |
| 3D | Real frames, GL errors, desktop/on-foot/portrait/landscape screenshots |
| UI | Deployment, tutorial dismissal, camera, map and quality settings |
| Co-op | Host plus seven genuine local WebRTC browser peers, loadouts, inputs and admission cap |

Physical Android/iOS devices, arbitrary WAN NATs, full 24-hour endurance, latency prediction, accessibility and the complete historical city map remain outside current certification. Quality settings provide a performance fallback; they do not constitute measured low-end-device guarantees.
