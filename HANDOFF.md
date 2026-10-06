# Startup Run 3D: Master Architecture & Handoff Notes

## Status: Fully Implemented v1.0 (Production Playable)
Flow: Main Menu (`Start Game`, `How To Play`, `High Scores`) -> Startup Naming Modal -> NYC Apartment Office Cutscene (Founder working at desk with laptop, janitor sweeping, typewriter subtitle dialogue) -> "Start Running 🚀" -> Camera transition to third-person -> Endless 3-lane 3D runner -> Game Over -> Top-10 LocalStorage Leaderboard -> "Run Again" / "Main Menu".

---

## 1. Features Implemented (Per Master Spec & User Vision)

### A. Narrative & Beginning Story (Stage 1)
- **Startup Office Cutscene**: A detailed NYC loft room with brick exterior, fire escape, warm morning sunlight, dust motes, pizza boxes, whiteboard with the Startup Roadmap, and two 3D characters:
  - **Founder Character**: Sitting at the desk typing on his glowing laptop with coffee cup, preparing to launch.
  - **Janitor Character**: Sweeping the floor with a broom.
- **Mandatory Intro Dialogue**: 
  > *"Ah, beautiful morning for the start of your startup business, hope you can manage to keep your business running as there is tons of competition."*
- Dynamic company badge (`[COMPANY NAME] HQ • DAY 1`) and animated "Start Running 🚀" button.

### B. Core Running & Financial Burn Rate (Stage 2)
- **3-Lane Track**: Left, Center, Right (`-3.2`, `0`, `3.2`).
- **Controls (Subway-Surfers Style)**:
  - `A` / `D` or `Left` / `Right`: Smooth lane switching with bank & yaw tilt.
  - `W` / `Up` / `Space`: Jump physics (gravity 34, initial jump velocity 12.8) to jump roadblocks and ditches.
  - `S` / `Down`: Slide (`CFG.slideTime` = 0.4s, eased in/out) to slide under overhead scaffolds, and fast-fall from mid-air.
  - Touch: Full swipe gesture support (left, right, up, down) + on-screen mobile buttons.
  - `P` / `Esc`: Pause menu modal.
- **Financial Burn Rate Engine**:
  - Initial Capital: **$50,000** starting balance.
  - Passive Burn Rate: **-$1,000/s** base burn, compounding +100% per minute survived (`CFG.burnGrowthPerMin`).
  - Speed Acceleration: +2% forward speed every 10 seconds survived.
  - Game Over Trigger: Account balance reaches $0 or negative (Bankrupt!).

### C. Decision Gate Matrix (UPDATED v1.1)
Gates spawn in pairs of 2 lanes (1 open lane). Pairs never offer double debuffs.
**All gates now look identical** (blue frame, navy sign, white title only). No green/red/gold color coding, no BUFF/DEBUFF pill, no effect numbers, no sparkles/sirens/crown. The player must judge from the NAME. The effect is only revealed after passing (toast + screen flash, unchanged). `def.type/color/badge` still exist in `GATES` but are only used for the pair rule and effects, never for gate visuals. Gate list unchanged (Angel Seed, Product Goes Viral, Popularity Surge, AWS Outage, Tax Audit, Rent Overdue, Debt Repayment, Hire Executive, Hire Tech Team).

### D. 3D Obstacles
- **Incoming Traffic Cars**: Colored cars / taxis driving toward player (`+z`). Can be vaulted over with a timed jump (`W`/`Up`/`Space`) or dodged by switching lanes. Ground hit: `-$15,000` + camera shake.
- **Roadblock Barriers**: Orange/white striped barriers with traffic cones. Jump over (`W`/`Up`/`Space`). Hit: `-$8,000`.
- **Road Ditches / Potholes**: Dark road hazards with caution curbs. Jump over. Hit: `-$10,000` + speed slowdown.
- **Overhead Scaffolding / Cable Banners**: Low hanging overhead obstacle. Slide underneath (`S`/`Down`). Hit: `-$12,000` head bonk.
- **Police Checkpoint**: Patrol car with flashing lightbar + officer with raised hand blocking 2 lanes. If hit, founder is held for 4 seconds while the survival timer and burn rate keep ticking!

### E. Audio System (Pure Web Audio API)
Zero external audio dependencies, 100% self-contained in `index.html`:
- Upbeat financial cash chime on Buff gate pickup.
- Deep descending buzz on Debuff hit.
- Harmonious two-tone chime on Hiring staff.
- Pitch sweep on Jump.
- Low swoosh on Slide.
- Impact rumble on Obstacle crash.
- Alternating siren pulse on Police stop.
- Melodic descending game over arpeggio on Bankruptcy.
- HUD `SOUND: ON / OFF` toggle button.

### F. Environment & Atmosphere
- Three.js PCF Soft Shadow Maps enabled with dynamic player & building shadows.
- 3-Lane asphalt road with tire wear paths, crosswalk stripes, and textured concrete tile sidewalks.
- City skyscrapers with randomized lighted office windows and startup billboards ("PIVOT TO AI 🚀", "SCALE FAST $$$").
- Dynamic time-of-day atmospheric transition: Morning Blue -> Golden Afternoon -> Twilight Neon Purple.

### G. Leaderboard & UI
- Top-10 historical leaderboard saved in `localStorage` (`startupRun3D_top10`).
- Comic typography (Google Fonts Bangers & Inter) with bold borders and comic-card popups.
- Single-file execution rule strictly maintained in `index.html`.

---
## Latest session (v1.1) changes
1. Gates: single neutral style (see C).
2. Slide: shortened 0.75s -> 0.4s (`CFG.slideTime`); fast ease-in and quick stand-up (`slideK`).
3. Jump/slide motion reworked (main loop "v1.1 Run / jump / slide animation"): jump has lead leg forward + trailing leg tucked, arms up, stretch on ascent, apex hang-time (lower gravity near apex, stronger when falling), landing squash (`G.land`). Slide now leans the body back with legs thrust forward and the body lowered, instead of squashing the model. Running bends knees and elbows.
4. `makeHuman()` limbs rebuilt: ellipsoid upper/lower segments with elbow and knee pivots (`armX.fore`, `legX.shin`) instead of cylinders. Torso/head unchanged. The player animation uses the new pivots; the janitor, founder and cop still animate only the main pivots (they work, but could also use `.fore/.shin`).

## Open TODO (not done yet; suggested order)
1. **Playtest v1.1** in a browser (only `node --check` syntax-checked). Check slide timing vs scaffold obstacles, the new jump feel, and that the slide pose looks right from the chase camera.
2. **Character realism still wanted**: faces (eyes/brows/mouth), ears, hands with fingers, hair shapes, clothing details (hoodie, sneakers with soles, backpack straps), cloth-like shading. Consider `MeshStandardMaterial` or custom gradient toon maps; keep outlines thin. Recommended style target: Subway Surfers (chunky, saturated, smooth). A 2D switch was discussed but NOT approved; do not convert to 2D without the user's confirmation.
3. **Cinematic cutscene (NOT done)**: realistic character poses and animation (founder typing with finger/arm motion, janitor walk cycle that turns at the ends), camera moves (dolly + crane + rack-focus feel), depth of field or vignette, volumetric light shafts, better prop models (laptop, mug, desk lamp, shelves, books, plants), subtle film grain/color grading overlay in CSS, ambient sound.
4. Object polish for obstacles/props (cars with windows/grille, realistic barriers, detailed buildings).
5. Split into multiple files only after stability (spec section 9).

---
## Session v1.2 changes (model polish)
1. `makeHuman()` rebuilt again: smooth LatheGeometry torso, tapered capsule limbs (`cap()`) that overlap ball joints so there are no cylinder seams, elbow/knee pivots kept (`.fore`, `.shin`), real hands (palm + 4 fingers + thumb), face (eyes with pupils, brows, nose, mouth, ears), hair cap + fringe, shoe with sole. Feet now sit on y=0 (old version sank ~0.28). Interface unchanged, so player, janitor, founder and cop all get the new body automatically.
2. `makeCar()` rebuilt: lower body, hood/trunk curves, tinted glass cabin, roof, bumpers, grille, headlights/taillights, mirrors, wheels with hubcaps; police car has a black door band + the same flashing light bar (`userData.lights` unchanged).
3. NOT changed: cutscene props (desk, laptop, whiteboard, sofa), road barriers, ditches, buildings, gates. Cutscene animation and camera are still the old ones. See Open TODO above.
4. Not visually verified. Check hands/face from the cutscene camera, the player back view, and that the founder's seated pose still looks right with the new limb lengths (limbs are slightly longer than in v1.1).

---
## Session v2.0: modern look overhaul (PARTIAL, see what is NOT done)
Backup of the pre-overhaul file: not kept in outputs (ask the user to keep their own copy of v1.2).
**Done**
1. Outlines removed: `outline()` is now a no-op (old calls kept so nothing breaks).
2. PBR: `toon()` now returns `MeshStandardMaterial` (roughness .5, metalness .1; the 0x66e0ff color gets an emissive glow for monitors). Every other `MeshToonMaterial` was converted to `MeshStandardMaterial`.
3. Rendering: ACES filmic tone mapping, `FogExp2` (density .0075; day/night code still calls `scene.fog.color.set`, `scene.fog.far = 220` is now a harmless no-op), hemisphere light 0.7, warm sun 1.5, 2048 soft shadow map, brighter loft light.
4. Gates: still ONE color/style for every gate (spec rule from the user). Now a brushed-metal gantry with neon cyan edge strips and a translucent holographic title panel (Inter font, no effect info).
5. Loft: procedural wood plank floor with soft reflections.
6. Camera: runner transition now uses smootherstep and takes 3.0 s (was smoothstep 2.2 s). Lane change smoothing slightly softer. Run cycle has hip/shoulder sway.
7. UI: all `Bangers` fonts replaced with Inter (CSS and canvas text). A glassmorphism override block was appended at the end of `<style>` (blurred translucent cards, gradient buttons, rounded HUD pills, no thick black borders). The old comic rules are still above it and are overridden with `!important`; delete them when cleaning up.
**NOT done (open TODO, in order)**
- Sky gradient dome (scene.background is still a flat Color because the day/night code calls `.set()`; replace with a gradient sky mesh that follows the player and tints with time of day).
- Street lamps with real glow/light cones (bulbs are still plain emissive spheres; avoid many PointLights, use additive cone meshes or a few lights pooled near the player to prevent shader recompiles).
- Detailed skyscrapers (glossy glass curtain walls, roof structures, billboard frames), volumetric tree canopies, more street props.
- Cars: add metallic paint (`metalness .6`, `roughness .25`) and clearcoat via `MeshPhysicalMaterial`; roadblocks/scaffolds with trusses; police LED light bar detail.
- Loft: detailed modern desk setup, furniture, window framing, ambient particles polish; founder and janitor animation and camera moves (see earlier TODO).
- Character: sneaker detail, hair styles; the model is smooth and jointed but not at Subway Surfers asset quality (code-only limit).
- Playtest everything: this version was only `node --check` syntax-tested. Look for too-dark or too-bright lighting after the switch to PBR and tone mapping, and tune light intensities (`hemiLight`, `sun`, room `PointLight`).
