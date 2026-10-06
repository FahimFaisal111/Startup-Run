# Startup Run 3D: Handoff Notes

Written by Claude (session 1). Next AI: read this, then `index.html`. Keep the single-file rule until the game is stable (spec section 1).

## Status: playable v0.3 (not browser-tested by the author, so test first)
Flow: name modal -> NYC window cutscene (typewriter subtitle, janitor sweeping) -> "Start Running" -> camera fly to third-person -> endless 3-lane runner -> game over -> Top-10 leaderboard -> "New Startup".

## Implemented (per spec)
- Three.js r128 via CDN, MeshToonMaterial plus black back-face outlines (`outline()` helper).
- Comic UI: thick panel border, Bangers/Inter fonts, speech-box subtitle with the mandatory intro text.
- Controls: Left/Right or A/D, plus touch swipe. 3 lanes.
- Economy: $50,000 start, burn $1,000/s growing +100% per minute (`CFG.burnGrowthPerMin`), game over at balance <= 0.
- Speed: +2% every 10 s survived.
- Gates (5 in `GATES`): Get Funding (+$25k), Go Viral (+$15k, burn x0.9), Server Crash (-$20k), Tax Audit (-$35k), Hire Expert (-$10k, +$500/s). Spawn in pairs of two lanes; third lane empty. A pair is never debuff+debuff. Gates are ALL visually identical (no color/info hints).
- HUD: balance, `SURVIVAL TIME: MM:SS`, burn rate, speed or income.
- Leaderboard: localStorage key `startupRun3D_top10`, objects `{companyName, survivalTime, survivalSeconds, finalBalance, ts}`, sorted descending, capped at 10.

## Code map (MODULE markers -> future files)
| Marker in index.html | Future file |
|---|---|
| MODULE 1 state, CFG, GATES | js/gameplay.js (+ config) |
| MODULE 2 renderer, lights, player, track | js/scene.js, js/runner.js (player) |
| Intro room, window, `CAM_WINDOW` | js/cutscene.js |
| MODULE 3 cutscene and camera transition | js/cutscene.js |
| MODULE 4 gate spawning | js/runner.js |
| MODULE 5 collisions and economy | js/gameplay.js |
| MODULE 6 leaderboard | js/leaderboard.js |
| MAIN LOOP + INPUT | js/main.js |

## Session 2 changes (v0.2), user requests
1. **Main menu**: Start Game -> name modal -> cutscene; High Scores (reads `startupRun3D_top10`, empty state message); game over has "Main Menu". Phases: `menu | name | cutscene | transition | play | over`.
2. **Human character**: `makeHuman()` builds torso, head, hair, eyes, arms, legs and shoes with pivot limbs. Used by the player (`P`, run cycle) and the janitor (`J`, sweeping with a broom).
3. **Environment**: textured road scrolled with the player, sidewalks, curbs, windowed buildings with water towers, lamp posts, trees, clouds and a sun. Props recycle every 480 units in `updateWorld()`.
4. **Gates**: simple titles only. No color coding and no effect text on the gate; the effect shows in a white toast after passing. Uniform blue frame + cream sign (`makeGate`, `labelPlane`).
5. **Cinematic intro**: brick NYC facade, fire escape, whiteboard "STARTUP PLAN", posters, plant, lamp, rug, bean bag, pizza, dust motes, sunbeam, warm light, dawn sky, black letterbox bars, fade-from-black, slow 9 s camera dolly-in with sway. The menu reuses the same view. The camera hands over from the window to the runner view in `beginRun()`.

## Session 3 changes (v0.3), user requests
1. **Controls (Subway-Surfers style)**: Left/Right or A/D = lane change (smooth, with body bank + yaw). **Up/W/Space = jump** (gravity 34, jump velocity 12). **Down/S = slide** (0.7 s crouch; also fast-falls in the air). Touch: swipe left/right/up/down. Run cycle has bigger arm/leg swing, forward lean, tucked-arm jump pose. Code: `onKey()`, play branch of the main loop, `G.vy/air/slide`.
2. **Character physique**: `makeHuman()` rewritten with ellipsoid chest/waist/hips, tapered round limbs, ball joints, oval head, hair cap, round shoes, rounded backpack. Still toon-shaded and outlined. Used by player `P`, janitor `J` and the police officer.
3. **Obstacles** (MODULE 4b, spawn one row 30 units after every gate pair via `spawnObstacle`):
   - `car`: incoming traffic moving +z (12 u/s), single lane, cannot be jumped. Hit = -$15,000 + camera shake.
   - `block`: striped road barrier + cones, **jump over**. Hit = -$8,000.
   - `ditch`: dark pit with yellow edges, **jump over**. Hit = -$10,000 + 1 s slowdown (40% speed).
   - `police`: patrol car with flashing lights + officer with raised hand, blocks 2 adjacent lanes (one lane free). Hit = **player is held still for `CFG.policeHold` seconds (default 4)**. Balance keeps burning and the survival timer keeps running. A banner shows the countdown. The user said "few minutes"; this is a placeholder, so tune `CFG.policeHold`.
   - Collision logic: `checkObstacles()` (lane proximity + height check vs `player.position.y`).
4. **Environment realism (PARTIAL)**: muted real-world building palette, grainy asphalt with tire-wear strips, concrete sidewalks, hydrants and trash bins. Gate and obstacle visuals are unchanged in style. The user wants MORE realism; see TODO.

## Known gaps / TODO (suggested order)
1. **Playtest v0.3 in a browser** (only syntax-checked with `node --check`, never run). Check: jump/slide feel, obstacle fairness (obstacle row can appear in the same lane as a gate), car speed, police hold length, performance.
2. **Environment realism (main open request)**: enable shadow maps (directional light shadows, `castShadow` on player/cars/buildings); swap `MeshToonMaterial` for `MeshStandardMaterial` on world objects (keep toon on characters?) or decide on a consistent style; sidewalk tile texture and crosswalks; building facades with balconies/awnings/shop fronts; parked cars and pedestrians; oncoming-traffic lanes beyond the curb; road reflections and puddles; sky gradient with sun glow; optional time-of-day shift as the run goes on.
3. **Character polish**: face details, clothes variety, jointed knees and elbows, real slide animation (currently squash + lean), landing squash, idle breathing during police stop.
4. Obstacle variety: low barriers to slide under, moving trains or trucks, coins. Spawn rules so obstacles do not stack with gates or make an unavoidable hit when balance is low.
5. Sound, pause key, mobile on-screen buttons, leaderboard on the start screen (exists in High Scores).
6. Perf: many outlined meshes; consider merging static props or dropping outlines on distant objects.
7. Only after stability: split files per spec section 9 plus new modules: `character.js` (makeHuman, player), `environment.js` (world), `obstacles.js` (MODULE 4b), `cutscene.js`.

## Handoff prompt
Use the template from the master spec section 2 and paste the current `index.html` into the `[INSERT EXISTING INDEX.HTML HERE]` slot.
