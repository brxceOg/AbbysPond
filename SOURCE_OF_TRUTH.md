# Abby's Pond — Source of Truth
Version: 1.0
Status: Canonical design and implementation contract before gameplay coding
Repository: https://github.com/brxceOg/AbbysPond
Primary target: iPhone Safari, with responsive support for desktop browsers
Product intent: A cozy, cute, polished 2D pixel-art idle/incremental duck-collecting game made as a personal gift for Abby.

This is the canonical reference for game design, presentation, architecture, and delivery. Before changing an established decision, update this document first. If implementation and this document disagree, treat this document as authoritative until deliberately revised.

## 1. Core fantasy and design pillars

The player tends a beautiful pond, collects increasingly special ducks, watches each duck paddle around the water, and earns money from the flock. The pond should feel like a tiny living place worth opening just to watch. Beneath the cozy surface, a light auto-battler loop lets ducks protect their home from realistic duck predators.

Pillars:
1. **Cozy and affectionate:** soft colors, gentle motion, rounded UI, warm details, and a thoughtful romantic tone without overwhelming the play area.
2. **A pond that feels alive:** the pond is the main play space; ducks visibly swim, bob, turn, idle, and react. Reeds, lily pads, flowers, ripples, reflections, and flowy shoreline grass make the scene feel hand-crafted.
3. **Every duck matters:** every owned duck has its own identity, rarity, income contribution, and combat contribution. Collecting is meaningful rather than just increasing a single number.
4. **Idle progression with choices:** money grows over time, upgrades improve efficiency and luck, rolls unlock new ducks, and equipment/duck upgrades let the player shape their flock.
5. **Predators make ecological sense:** enemies are animals that plausibly threaten ducks, with stronger threats introduced as the flock and its capabilities progress.
6. **Mobile-first comfort:** usable with one thumb on iPhone Safari, with safe-area padding, legible text, generous touch targets, and no essential hover interactions.
7. **Finished, reliable implementation:** no placeholder code, fake buttons, unfinished systems, or knowingly broken mechanics in committed work.

## 2. Main gameplay loop

1. Ducks swim around the pond and generate money individually over time.
2. The player spends money on general upgrades, duck collection rolls, duck-specific improvements, and equipment.
3. The collection grows in variety, rarity, income, and defensive strength.
4. Predators occasionally invade the pond. The player's ducks automatically defend it.
5. Successful defense grants a reward appropriate to the threat; losing a fight has a fair, non-destructive consequence and must not delete ducks or erase earned progression.
6. Stronger flock progression raises the threat level over time, introducing tougher but ecologically appropriate predators.
7. The player returns later to find offline earnings, calculated safely and transparently.

The loop should be understandable within the first minute, with new systems introduced progressively rather than dumping every menu and mechanic on the player at once.

## 3. Pond and visual presentation

- The pond occupies the dominant play area and is the visual heart of the game.
- Use a cohesive 2D pixel-art aesthetic with a deliberate palette, readable silhouettes, consistent pixel scale, and subtle animation.
- Water should feel gently animated through layered color, drifting highlights, small ripples, and duck wake trails; effects must remain performant on mobile.
- Shoreline and surrounding details should include naturally varied, flowy grass, reeds, cattails, small flowers, stones, lily pads, and occasional decorative surprises. Avoid clutter that competes with ducks.
- Ducks should visibly float/swim around the pond rather than act as static icons. Give them gentle bobbing, turning, movement variation, and appropriate reactions to events.
- Keep important UI overlaid on the scene in calm, organized panels with clear spacing and hierarchy. Avoid covering the pond with permanent opaque UI.
- Use a custom-feeling type pairing: a friendly display face for headings and a highly legible face for numbers/body text. Bundle or load fonts robustly; provide fallbacks and avoid layout shifts.
- Visual tone: tender, playful, storybook-like, polished, and cozy. The affectionate identity should feel sincere rather than overly saccharine.
- **Chosen art direction — Romantic cottagecore:** use a warm, gentle palette of blush pink, cream, soft sage, muted rose, and pond blue. Pair cozy floral details and rounded, lovingly crafted UI with lush, detailed pixel-art nature. Keep the pond readable and the overall presentation elegant rather than cluttered.
- **Chosen romantic approach — hidden personal surprises:** include small, discoverable affectionate details for Abby, such as rare heart-shaped ripples, tiny paired flowers, a tucked-away note or keepsake, or a special duck-related surprise. These should be optional discoveries woven naturally into the world, not constant large heart graphics or intrusive pop-ups. Keep the exact surprises tasteful and delightful; don't include private personal information unless explicitly supplied for the game.
- Use reduced-motion preferences where practical and avoid rapid flashing or unnecessarily intense effects.

## 4. Core screens and menus

### Pond / Home
- Main animated pond scene.
- Current money and income rate.
- Duck count / collection progress.
- Clear access to the main menus.
- A compact event/status indicator for predator encounters.
- Controls must not obscure the main flock or overlap on iPhone.

### General Upgrades
A dedicated menu for economy and account-wide stats, including:
- **Income / efficiency:** increases earnings or improves economic scaling.
- **Luck:** improves roll odds for higher rarity ducks, with understandable probabilities and diminishing or capped effects where needed.
- **Defense / strength:** improves the flock's ability to repel predators.
- **Collection / roll improvements:** reduces roll costs or improves collection convenience as balance allows.
- **Offline earnings:** increases the portion or duration of idle earnings collected while away.

Every upgrade needs a clear effect, current level, next-level benefit, and cost. Purchase controls must validate affordability and update state immediately and consistently.

### Duck Upgrades & Equipment
A separate menu focused on individual ducks and the flock:
- Select an owned duck to inspect it.
- Display its name, species/variant, rarity, income, combat stats, and equipped items.
- Allow duck-specific upgrades such as income, health, attack, or defense where supported by the duck's role.
- Equip and remove valid items with clear slot compatibility and stat changes.
- Show equipped effects clearly; do not silently replace valuable equipment.
- Duck upgrades and equipment must persist in the save.

### Inventory
A separate inventory menu:
- Lists owned equipment, consumables (if introduced), and collectible items.
- Shows item rarity, effects, quantity, and whether/how it can be equipped or used.
- Clearly distinguishes an item in inventory from an item currently equipped.
- No dead-end items or UI controls that imply unsupported functionality.

### Duck Collection / Roll
- A dedicated, readable roll/recruitment interaction to unlock ducks.
- Show odds before the roll; luck upgrades may change odds but must not make displayed odds misleading.
- Each roll resolves to a real duck with defined stats, income, rarity, and appearance.
- Duplicate results must have a defined outcome before duplicate mechanics are implemented.
- Animation can add delight but must remain skippable/brief and must not delay access to the result.

### Predator encounter
- A readable, short event presentation that does not permanently cover the pond.
- Predators enter the pond/shore in a way appropriate to the species.
- Ducks automatically fight using their stats and equipment.
- Show enough feedback to understand the outcome and rewards without requiring manual combat.
- Encounters must not cause data loss or trap the player in a modal.

## 5. Ducks, rarity, and rolling

- Ducks are distinct collectible entities, not only a count.
- Each duck has a stable unique instance ID, a species/variant ID, rarity, income value, combat stats, upgrade levels, and equipment slots/loadout.
- Species/variants should have distinct silhouettes and palettes that remain readable at small iPhone screen sizes.
- Rarity names, colors, roll weights, base stats, and income values must be data-driven rather than scattered across UI code.
- The roll system must use a reliable weighted-random implementation and testable odds.
- Luck changes probabilities according to one documented formula. It must not make rare outcomes guaranteed unless a deliberate endgame mechanic says so.
- Avoid presenting random outcomes as guaranteed. Show the chance for each rarity and, where practical, a pity/anti-streak rule if one is introduced.
- Balance the economy so new ducks feel exciting while older ducks remain useful through upgrades, equipment, or synergy.

Initial rarity ladder (proposed starting point; tune only with a documented balance change):
- Common
- Uncommon
- Rare
- Epic
- Legendary
- Mythic

Initial species and exact odds are content decisions to finalize alongside the first playable implementation; do not ship fake or unimplemented entries in a roll table.

## 6. Predator ecology and progression

Predators must be plausible threats to ducks and scale with player progression. Candidate roster:
- **Snapping turtle:** an early, pond-native threat; dangerous at close range.
- **Water snake:** a stealthy, quick threat around reeds and shallow edges.
- **Large fish:** only use if the game includes a plausible vulnerability such as ducklings; otherwise choose a better-fitting threat.
- **Raccoon:** raids the shoreline and may attempt to snatch food or pressure the flock.
- **Fox:** a stronger land-based threat approaching from the bank.
- **Bird of prey (such as a hawk):** an aerial threat with a brief telegraphed dive.
- **Coyote:** a rare, high-tier shoreline threat, introduced only at high progression.

These are candidates, not a requirement to implement all at once. Each predator needs a clear behavior, stats, encounter weight, unlock threshold, and reward/penalty. Avoid nonsensical matchups or enemies that appear before the player can reasonably handle them.

Encounter rules:
- Encounters are random but governed by documented probability and a cooldown/anti-back-to-back rule so they feel exciting, not oppressive.
- Threat strength scales from a defined progression measure, such as total flock power and/or account progression, not simply duck count alone.
- Communicate the danger level before or as an encounter begins.
- A defeat must be fair: no permanent loss of collected ducks, currency, or equipment.
- Reward success with money and/or defined materials; keep rewards proportionate to risk.
- Combat resolves automatically using transparent, consistent rules. No placeholder combat outcomes.

## 7. Economy, idle time, and saves

- All ducks contribute income individually; total income is the sum of current ducks' effective income after upgrades/equipment.
- Use a consistent currency model and display both current balance and earnings rate.
- Costs and earnings must be data-driven and balanceable.
- Offline earnings use elapsed time since the last valid save, with a defined maximum offline duration and a clear summary when returning.
- Clamp elapsed time and validate persisted data to avoid absurd gains from clock changes or malformed saves.
- Save automatically after meaningful changes and periodically while active, using browser storage suitable for GitHub Pages and Safari.
- Include a deliberate reset-save flow with confirmation; never reset progression accidentally.
- Save schema must be versioned. Future changes need safe migration/defaulting for older saves.
- Handle missing, malformed, or inaccessible storage gracefully with a useful message rather than crashing.
- The game must be fully playable without an account or server. Do not introduce paid mechanics, tracking, or network-dependent core gameplay.

## 8. Mobile and accessibility requirements

- iPhone Safari is the primary acceptance target.
- Responsive layout must account for iOS safe areas and both portrait and landscape where practical.
- Use touch targets of comfortable size, adequate contrast, readable text, and clear pressed/selected states.
- No critical control may rely on hover, keyboard-only input, or precision tapping.
- Avoid page scrolling, zoom traps, accidental text selection, and overscroll where the game surface is intended to be app-like; do not disable normal browser behavior more broadly than necessary.
- Respect safe-area insets and dynamic viewport changes.
- Keep animation and particle counts efficient for mobile performance.
- Check that menus fit small screens and can be opened, scrolled, and dismissed reliably.

## 9. Technical direction

- Static browser game that can be hosted on GitHub Pages.
- No backend required for core play, saves, rolls, combat, or offline earnings.
- Keep gameplay data, simulation, rendering, and UI concerns separated enough to make systems testable and expandable.
- Prefer a lightweight implementation that is easy to maintain and fast on iPhone Safari. Choose the final rendering/framework approach after inspecting repository needs; avoid unnecessary dependencies.
- Use deterministic/testable functions for economy calculations, weighted rolls, upgrade pricing, encounter scaling, combat resolution, and save migration.
- Keep content definitions (ducks, rarities, equipment, upgrades, predators) in structured data.
- Do not rely on remote images or fonts for core functionality unless local/system fallbacks are complete.
- Do not commit secrets, private tokens, or personal information about Abby.
- Do not mix prototype experiments or unused alternate implementations into production paths.

## 10. Quality bar and definition of done

A change is not complete merely because code was written. Before committing:
- Check syntax/build output and fix all errors.
- Test the actual game flow affected by the change.
- Verify the main pond still renders, ducks move, income accrues, and menus open correctly.
- Verify saves persist after refresh and offline earnings are sane when relevant.
- Verify layout and controls on iPhone-sized viewports and Safari-specific behavior where testable.
- Verify no existing mechanic regressed.
- Ensure all visible buttons and menu entries perform a real, finished action.
- Ensure no placeholder code, TODO-driven unfinished feature, fake data path, or inert control is introduced.
- Make sure content, odds, stat effects, costs, and displayed descriptions agree.
- Distinguish what was tested directly from what could not be tested in the available environment.

## 11. GitHub workflow and release contract

- Repository: brxceOg/AbbysPond.
- This document is the canonical source of truth and should be updated before deliberate design changes are implemented.
- Work incrementally, with focused changes and a clear commit message.
- Preserve established behavior unless a design change is explicitly agreed upon.
- Every commit that changes the game should be followed by a playable GitHub Pages URL and a concise explanation of what changed and what was verified.
- Do not claim a build is live or playable until GitHub Pages deployment has been checked. If Pages is not configured yet, configure/verify it before promising a live URL; until then, report the actual status.
- Never invent a playable URL, commit hash, test result, or deployment status.
- Keep the project separate from Idle Mining. Decisions and code from that game do not automatically apply here.

## 12. Initial implementation milestones

1. **Foundation:** inspect the existing repository, establish the static app structure, responsive scene, save schema, and visual style; get a genuinely playable pond online.
2. **Duck economy:** add moving ducks, individual income, currency display, and persistence.
3. **Collection:** add the roll system, rarity odds, collection UI, and initial fully implemented duck roster.
4. **Progression menus:** implement general upgrades, duck upgrades/equipment, and inventory as separate functional menus.
5. **Predator encounters:** implement a small, complete initial predator roster and automatic combat; test fairness and scaling.
6. **Polish and balance:** enrich the pixel-art pond, add sound/motion settings if included, tune the economy, test offline earnings, and validate iPhone Safari.
7. **Release verification:** check the live GitHub Pages build, persistence after refresh, mobile layout, and all core flows before calling the milestone done.

Each milestone should be fully implemented and verified to its stated scope before moving on. Do not create fake placeholders for future milestones.

## 13. Open balance/design decisions

These details can be finalized during design and early implementation, but must be explicitly decided and documented before their affected systems ship:
- Initial duck roster, species variants, base stats, rarity weights, and exact roll costs.
- Exact formula for luck and probability redistribution.
- Equipment slot types, item roster, and duplicate-duck outcome.
- Encounter chance/cooldown, threat scaling formula, combat timing, and reward tables.
- Offline earnings cap and rate.
- Whether audio is included in the first release; if included, provide mute controls and avoid requiring sound.
- Exact font choices and asset sourcing/licensing.

No open decision authorizes placeholder code. A system should not ship until its required decisions are resolved and its implementation is complete.


## 11. Duck collection wheel (2026-10-15)

The duck invitation interaction uses a cozy animated prize wheel to reveal the result. The wheel shows every currently collectible duck, with slice sizes proportional to the actual per-duck probability derived from the existing rarity weights and the number of ducks in each rarity. The displayed rarity odds remain visible before spinning. Resolve the winning duck with the existing weighted rarity roll and within-rarity species selection before the animation starts; the wheel must land on that already-determined result and never alter odds or award a different duck. Deduct the invitation cost once, prevent duplicate spins while an animation is active, and award the existing new-duck or duplicate compensation only after the wheel finishes. Keep the wheel responsive on iPhone Safari, provide a reduced-motion-friendly shorter reveal, and make the final result clear. Preserve all existing economy, save, collection, and luck-upgrade behavior.


## 12. Handcrafted cottagecore interface (2026-10-15)

The UI should feel like a lovingly illustrated storybook game rather than a generic dashboard. Use warm paper and botanical tones, organic asymmetrical borders, gentle layered shadows, subtle paper/fabric textures, and a clear type hierarchy with Lora for storybook headings, Nunito for legible body/UI copy, and Caveat as a handwritten accent. Home-screen navigation should express hierarchy: a prominent full-width duck invitation action, with upgrades, flock care, and keepsakes as smaller secondary destinations. Keep important mobile touch targets at least 44px tall, preserve safe-area padding and comfortable scrolling, maintain high contrast and readable type, and never rely on hover. Menus should feel like pages in the same world, not a stack of identical technical panels. Keep the duck wheel, mechanics, accessibility preferences, and saved progress unchanged during visual refreshes.

## 14. World-integrated UI panels (2026-10-15)

Menus and text containers must look like physical storybook objects that belong in Abby's pond, not default web cards layered over a game. Use inset paper frames, fine dashed stitching, warm wood/paper edges, small botanical flourishes, restrained fabric-like header treatments, and layered pressed-paper shadows. The home navigation should feel like a little signboard or collection of crafted keepsakes; the modal should read as a framed storybook page; upgrades, duck details, collection entries, rarity odds, and settings should each have intentional visual hierarchy rather than all sharing one generic box style. Decorative details must remain behind content and never reduce tap targets, legibility, scrollability, or accessibility. Keep all existing mechanics, save data, odds, and controls intact.

## 15. Full-bleed pond with floating controls (2026-10-15)

The animated pond is the full-screen home scene, not a small framed box. Let the water, shoreline, and ducks fill the entire viewport behind the interface. Keep the title/settings and compact economy/flock stats grouped at the top; keep duck invitation and secondary navigation in a calm, translucent bottom dock; place the affectionate pond interaction in the top controls. UI must stay readable with soft storybook-paper surfaces, but remain visually light enough that the pond is always visible around and between controls. Preserve touch access, safe-area spacing, duck tapping/ripples, the hidden pond secret, and all existing gameplay logic and saved progress. On short iPhone screens, compress the overlay spacing instead of shrinking the pond or forcing page scrolling.

## 16. Species-specific duck anatomy and silhouette pass (2026-10-15)

Duck identity must be expressed through the duck's actual silhouette and feather structure, not only palette, face details, or decorative symbols. The roster should have species-specific combinations of body proportions, tail silhouettes, coat/feather markings, head-feather formations, and wing shape. Every duck should remain recognizable at normal pond scale and while swimming in either direction.

- Preserve the cozy, rounded, hand-drawn pixel-art/storybook style; anatomy differences should feel soft and natural rather than spiky, mechanical, or pasted on.
- Give every species a deliberate design across five channels: body proportions, tail silhouette, feather/coat pattern, head formation, and wing silhouette/feather layering. A species may share one subtle trait with another, but the overall combination must remain distinct.
- Make Meadow the clean, classic reference duck; Puddle compact and speckled; Clover leafy and gently scalloped; Bluebell elegant and tapered; Peaches round and plush with its curled forelock; Lavender soft and layered; Moonbeam sleek and crescent-feathered; Rosie rounded with petal-like feather edges; Sunpetal bright with a fan-like wing/tail; Stardust long and airy with small constellation flecks; Heartsong soft and heartlike with layered wing feathers; Wish graceful with a lifted, wish-star crest and elegant wing tips.
- Use the existing canvas renderer and species IDs; do not introduce external placeholder sprites or dependencies. Keep species anatomy data-driven and rendering helpers readable.
- Keep wing motion gentle and species-appropriate, with no frantic flapping. Ensure every shape is drawn within the duck's visual layer and does not break eye, bill, head-decoration, wake, or body-marking legibility.
- Do not change species IDs, rarity, stats, income, collection odds, roll outcomes, save schema, movement paths, collision avoidance, predator logic, or UI/gameplay. Treat this as a presentation-only change.
- Before calling this complete, syntax-check the full script and inspect all twelve species at actual pond scale and mirrored swimming directions; check mobile performance and verify the pond and saves still work.

### Follow-up: stronger body silhouette contrast (2026-10-15)

The first anatomy pass was too subtle at the live pond scale. Body proportions now use a clearly wider range: Puddle is compact and nearly round; Peaches is short and plush; Rosie is compact with a rounded body; Bluebell is distinctly long and low; Moonbeam is sleek and elongated; Wish is narrow and graceful; Stardust is long with a slightly fuller profile; Heartsong is fuller and rounded; Clover and Lavender have medium-full bodies; Sunpetal is long-medium and broad enough to support its fan identity; Meadow remains the balanced baseline.

Tail placement is derived from each body's width so tails stay attached to both stubby and elongated bodies. Keep body/head proportions and tail attachment data-driven, preserve existing markings and wing designs, and do not change movement, collisions, species stats, economy, progression, saves, or any other gameplay. Syntax validation alone does not prove visual quality: the live pond still needs a visual pass on mobile across all twelve species and both swimming directions.

### Follow-up: distinct species build archetypes (2026-10-15)

The previous changes still read too much like one generic duck with decorations. The renderer now gives species separate contour profiles rather than relying on ellipses alone: compact round Puddle; plush high-backed Peaches; scalloped Clover; streamlined, long-low Bluebell; soft layered Lavender; sleek Moonbeam; tapered Wish; petal-edged Rosie; deep-bodied Heartsong; broad Sunpetal; airy long Stardust; and balanced Meadow. Species also have more distinct neck profiles, more separated head proportions, species-scaled wing silhouettes, and different bill profiles. Existing species IDs, palettes, body markings, gameplay, movement, collision avoidance, economy, and save data remain unchanged.

Validation so far: the inline JavaScript passes syntax compilation. This is not a substitute for visual testing; the latest build still needs inspection in the live pond on mobile at normal scale, including ducks swimming both directions, to catch contour overlaps and verify that each archetype reads distinctly.
### Follow-up: species-specific head and face identities (2026-10-15)

The next renderer pass moves species identity into the face itself instead of relying on color and head accessories alone. Heads now use individual contour paths with different crown, forehead, cheek, and nape shapes. Eye treatments are more varied (including Clover's curved closed eye, Bluebell's focused eye, and distinct softer, brighter, or more angular eye styling across other species), while bills vary in length, width, taper, and color by species. Subtle cheek accents and brow/face markings further distinguish several types. This remains a presentation-only pass: species IDs, palette definitions, stats, income, odds, progression, movement, collision avoidance, predator logic, and save data are unchanged.

Validation: full inline JavaScript syntax compilation passed. The live pond still needs a visual check on mobile at normal scale and while ducks swim in both directions; syntax validation does not verify final appearance or deployment status.
### Follow-up: animated ripples around swimming ducks (2026-10-15)

Added subtle, continuously animated concentric surface ripples around every swimming duck, with slightly offset ring timing and soft broken highlights. Ripples render behind the duck sprites so the duck bodies interrupt the lines naturally at the waterline; the existing close wake lines and tap/heart/predator ripple effects remain intact. The effect is presentation-only and does not alter swimming paths, collision avoidance, economy, progression, or saves.

Validation: inline JavaScript syntax compilation passed. Visual verification at live mobile pond scale and deployment status still need confirmation.

### Follow-up: slow wave motion in pond surface texture (2026-10-15)

The small decorative surface strokes across the pond now drift subtly in a slow, overlapping wave pattern. Each mark keeps its stable, deterministic placement and original short, broken shape, while its position, angle, and brightness shift gently over time so the texture reads as flowing water rather than debris sitting on the surface. The texture remains clipped to the pond and avoids long rushing streaks. This is presentation-only; duck movement, duck ripples, gameplay, progression, and saves are unchanged.

Validation: the full inline JavaScript syntax compilation passed. The effect still needs visual confirmation in the live mobile build; deployment status has not been independently verified.

### Follow-up: more visible slow drift in surface texture marks (2026-10-15)

Clarified and strengthened the animation specifically for the short decorative texture strokes distributed across the pond surface, separate from the duck wake/ripple effects. Each mark now has a more noticeable but still gentle lateral drift, a slower vertical wave, a small rotation change, and soft brightness breathing. Marks remain short and broken, with no long rushing wave streaks. Presentation-only; gameplay and saves are unchanged.

Validation: full inline JavaScript syntax compilation passed. Live mobile visual verification and deployment status still need confirmation.

### Follow-up: gently drifting lily pads (2026-10-15)

Lily pads now have a more clearly visible, enlarged buoyant drift path intended to read at normal mobile screen scale. Each pad moves independently with layered horizontal and vertical motion and a more noticeable gentle rotation; the movement remains slow and organic rather than circular or synchronized. Existing pad shapes, veins, flowers, and pond clipping remain unchanged. Presentation-only; gameplay, duck movement, economy, and saves are unchanged.

Validation: full inline JavaScript syntax compilation passed. Live mobile visual verification and deployment status still need confirmation.

### Follow-up: lily pad water-contact shadows (2026-10-15)

Added a soft, offset translucent shadow beneath each drifting lily pad, plus a slightly darker underside/contact edge to give the pads a sense of thickness and help them feel seated on the water rather than pasted on top. The shadow follows each pad's position while retaining a small offset. Presentation-only; gameplay and save data are unchanged. Inline JavaScript syntax compilation passed; live mobile visual verification and deployment status have not been independently confirmed.

## 18. Lily pad shadows follow the original pad (2026-10-15)

Preserve the established lily pad artwork and silhouette; do not redraw or reshape the pad to create a shadow. Each pad keeps its original layered elliptical body and colors. A soft translucent shadow is built from overlapping ellipses, offset consistently toward the lower-right to imply light from the upper-left. The shadow shares the pad's local position and rotation so it travels with each pad's independent drift, with only subtle offset variation to keep the cast shadow feeling natural. This is visual-only. JavaScript syntax validation passed; live mobile rendering and Pages deployment were not independently verified.

## 19. Living pond atmosphere and woodland banks (2026-10-16)

The pond scene should feel gently alive without becoming visually cluttered. Keep a small number of trees anchored along the banks, with trunks rooted and leafy crowns swaying subtly from a breeze. Avoid evenly tiled foliage or a continuous wall of plants around the canvas edges; use sparse, naturally grouped shoreline plants and stones with breathing room. Add tiny water striders that travel in looping paths on the pond surface and leave faint broken ripples, plus a few winged insects that bob and loop above the water with quick wing flickers. These are decorative animation only and must not change duck, predator, or economy mechanics. JavaScript syntax validation passed; live visual appearance and Pages deployment were not independently verified.

## 20. Water strider paddling animation (2026-10-16)

The surface water striders keep their tiny body and looping movement. Their middle pair of legs use alternating fore-and-aft oar strokes: the tips sweep along the bug's length while the legs stay extended sideways, with a subtle outward push and recovery rather than a circular or helicopter-like motion. Scale the stroke amplitude and cadence from the strider's instantaneous path speed so paddling softens as it slows and settles toward a resting pose near a pause/turn. Keep the strider's heading steady between turns. Animate rotation only when its travel direction crosses into a new heading sector, easing smoothly through each turn and holding the new orientation afterward rather than constantly rotating along the whole path. All six legs must visibly originate at the body edge so no leg looks detached. Front and rear legs remain lightly splayed for balance. Keep the motion small and readable at mobile scale; this is cosmetic only. JavaScript syntax validation passed; live rendering and deployment were not independently verified.

## 21. Keep water strider legs level during turns (2026-10-16)

During heading changes, rotate only the tiny strider body; keep its legs and paddle strokes in the pond's horizontal screen plane so the propulsion limbs do not spin with the body. Preserve alternating fore-and-aft paddle strokes and the faint ripple effect. This is decorative animation only. Inline JavaScript syntax compilation passed; live rendering and Pages deployment were not independently verified.

## 22. Separate flying insect and water strider paths from ducks (2026-10-16)

Render the tiny winged pond insects after the duck sprites so they visually sit above the ducks instead of appearing to collide with or pass behind them. Water striders use a soft repulsion zone around current duck positions, bending their rendered paths away before the sprites overlap. Keep the strider's horizontal leg/paddle animation and turn-only body rotation. These are cosmetic avoidance/layering changes; duck mechanics and economy are unchanged. Inline JavaScript syntax compilation passed; live rendering and Pages deployment were not independently verified.

## 23. Give water striders real duck-avoidance turns (2026-10-16)

Replaced the previous position-offset repulsion, which made striders appear to slide sideways around ducks, with persistent per-strider movement state. When a duck enters the close-approach zone, the strider turns its heading away and then swims along that heading under its own forward motion. Near the pond boundary it turns back inward. The horizontal paddle legs and faint ripple effect remain. Inline JavaScript syntax compilation passed; live rendering and Pages deployment were not independently verified.

## 24. Correct flying insect draw order (2026-10-16)

The winged pond insects must be drawn after the complete duck-sprite loop in the active pond renderer. Drawing them earlier places them underneath later duck paint operations, regardless of their flight path, making them appear to collide with or pass behind ducks. Keep their looping flight and wing flick animation unchanged; this is a render-order correction only and does not alter duck or insect movement. Inline JavaScript syntax compilation passed; live visual rendering and GitHub Pages deployment were not independently verified.

## 25. Water striders hop onto lily pads (2026-10-16)

Water striders now check for a nearby lily pad while swimming. When one is close enough and the strider is not actively avoiding a duck, it performs a short arcing jump onto the pad, settles there for a randomized brief rest, then jumps back into the pond and resumes its usual forward swimming, paddling, boundary steering, and duck avoidance. The resting strider tracks the pad's current drift position so it stays aboard while the pad moves. A soft water/pad contact shadow helps the jump read as a hop rather than a teleport. This is cosmetic animation only. Inline JavaScript syntax compilation passed; live visual behavior and GitHub Pages deployment were not independently verified.

## 26. Duck-driven lily pad displacement (2026-10-16)

Lily pads now respond to nearby duck body positions with a gradual, damped push away from the duck rather than snapping away. Each pad retains its own small displacement and velocity while the existing gentle drift remains active. The pad position is constrained to an inner ellipse of the pond and outward velocity is damped at the boundary, preventing pads from being pushed outside the water. If a duck approaches within the resting strider's nearby-collision range, the strider immediately begins its existing jump-off animation, aims away from the duck, and returns to normal swimming with a brief boarding cooldown. Inline JavaScript syntax compilation passed; live visual behavior and Pages deployment were not independently verified.


## Lily pad edge clearance update
Lily pad boundary handling constrains the full visible sprite footprint (leaf, shadow, and flowers) inside the water ellipse rather than only keeping the pad center inside. The existing edge response projects the pad inward and damps/reflexes outward momentum, allowing it to bounce or settle along the edge without its art being clipped by the pond's water boundary.


## Pond shoreline sprite clipping correction
The water-texture clipping region is now closed immediately after drawing the water surface marks. Lily pads, ducks, water striders (including their legs and jump arcs), and flying insects render without being cut off by the pond ellipse. Water striders retain their existing shoreline-avoidance steering; water texture itself remains clipped to the pond.


## Water strider animation boundary safeguard
After every water-strider movement or animation update, each strider is constrained to an inner safe-water ellipse with room for its legs and jump arc. If an animation reaches the boundary, its position is projected back inside and an outward heading is redirected inward. Takeoff targets are adjusted to avoid carrying a strider outside the pond.


## Lily pad single-occupancy reservations (2026-10-16)

A water strider reserves its selected lily pad as soon as its landing animation begins. Other striders skip pads reserved by a different strider during landing, resting, or takeoff, and search for another unoccupied nearby pad instead. The reservation is released when the strider finishes jumping off and returns to swimming, preventing two striders from landing on or resting on the same pad at once. Inline JavaScript syntax compilation passed; live animation and GitHub Pages deployment were not independently verified.


## Lily pad collision for swimming water striders (2026-10-16)

Swimming striders now treat lily pad leaves as solid obstacles instead of gliding through them. If a pad is available and the strider is ready to board, it reserves that pad and enters the existing landing hop. If the pad is occupied or boarding is on cooldown, the strider is kept outside the pad's collision radius and steered along the edge, preserving the single-strider reservation rule. Inline JavaScript syntax compilation passed; live animation and GitHub Pages deployment were not independently verified.


## Shoreline-aware water strider takeoff (2026-10-16)

When a strider jumps off a lily pad near the pond edge, its takeoff direction is checked against the pond's safe-water ellipse. If the intended hop would head outward or cross the safe boundary, the strider instead aims along the local inward shoreline normal and uses a safe landing target. The takeoff target is no longer rewritten mid-animation by the boundary safeguard, avoiding abrupt target changes and visible teleporting. Inline JavaScript syntax compilation passed; live animation and GitHub Pages deployment were not independently verified.


## 17. Pond insect feeding and water strider life cycle (2026-10-16)

- Four small flying pond insects animate over the water. When a duck approaches an insect, there is a 38% chance of a catch; failed attempts do not remove the insect.
- A successful catch removes the insect briefly and plays a small sparkle burst plus water ripple at the catch point. Each insect respawns after a randomized 5–11.5 second delay, keeping the pond's insect population replenished.
- Water striders can court when two mature striders are within close range. Courtship is probabilistic, uses a cooldown to avoid constant repeated events, includes a short wing-touch animation, and displays a floating heart when a baby is produced.
- Successful courtship creates a juvenile strider at the pair's midpoint. Juveniles begin at reduced visual scale and gradually grow to adult size over approximately 45 seconds. They cannot mate until mature.
- The strider population is now dynamic rather than limited to the original three. Keep the rendering and movement lightweight for mobile, and preserve lily-pad reservations, shoreline safeguards, duck behavior, and existing pond visuals.
- Verification note for this implementation: repository edits were committed, but browser-based interaction and visual regression testing still need to be performed before the feature can be considered fully verified.


## 18. Duck insect-eating animations (2026-10-16)

- Successful flying-insect catches now stretch the duck's neck and bill upward and slightly forward in the air, rather than widening the head; successful water-strider catches trigger a head-and-bill dip toward the water.
- A small cream-colored “nom nom nom” speech bubble appears briefly above the duck only after a successful catch.
- Ducks can catch nearby water striders probabilistically. A caught strider briefly disappears and then returns to the pond, preserving the ongoing ambient strider population rather than permanently reducing it.
- These are presentation and ambient-life changes only; existing duck collection, economy, save data, and insect respawn behavior remain unchanged.
- Verification note: the source was committed to GitHub, but browser-based visual testing and live Pages deployment have not been independently verified.


## 19. Duck growth from successful insect catches (2026-10-16)

- Each duck tracks its own successful insect meals in the existing duck save object, so growth persists across reloads without changing the save version or disrupting older saves.
- Every successful catch of a flying insect or water strider adds one meal and increases that duck's rendered size by 2.2%, up to 16 meals (a maximum of about 35% larger than its original size). Failed catch attempts do not increase size.
- Growth scales the duck sprite as a whole, keeping its species-specific body, head, tail, feet, and markings in proportion. The existing feeding animations, speech bubble, catch odds, insect respawns, strider respawns, movement, and economy remain intact.
- Validation: inline JavaScript syntax compilation passed if confirmed by the implementation check; live visual testing and GitHub Pages deployment should still be verified in the browser.


## 20. Duck growth feedback animation (2026-10-16)

- Every successful flying-insect or water-strider meal now triggers a green outlined up-arrow beside the duck's head. It floats upward and fades away over about one second.
- The duck also performs a quick whole-body squash-and-stretch pop during its existing 720 ms feeding animation, emphasizing the size increase without distorting its species-specific proportions.
- The arrow and pop reuse the existing feeding events; growth caps, save persistence, catch chances, insect respawns, speech bubbles, and other gameplay remain unchanged.
- Validation: inline JavaScript syntax compilation passed after resolving a local variable-name collision; live browser visual testing and GitHub Pages deployment have not been independently verified.


## 21. Duck growth feedback render-order fix (2026-10-16)

- Code inspection found that the growth arrow was drawn after the duck sprites, but **not at the end of the active pond renderer**. Shoreline foliage, ripples, and the predator encounter sprite were painted afterward, so those later layers could cover the feedback. The arrow now renders as the final scene layer, above all pond artwork and encounters; it is no longer dependent on where the duck sits relative to foreground decoration.
- Replaced the thin outlined-only arrow with a larger filled bright-green arrow, dark outline, pale inner highlight, and soft shadow. It floats farther upward and fades over 1.25 seconds.
- Increased the duck's feeding squash/stretch pulse so the growth response is more noticeable while preserving the duck's species-specific sprite and movement.
- The effect still triggers only on a successful fly or water-strider catch; catch probabilities and growth progression are unchanged.
- Inline JavaScript syntax compilation passed. GitHub Pages publication and live browser behavior have not been independently verified.

## 22. Queued, fading HUD event messages (2026-10-16)

- The status message below the money and flock cards now shows one event at a time. Each message fades in, remains readable for about 3.2 seconds, fades out, and then the next queued message appears.
- Messages generated close together are queued instead of overwriting or stacking on top of one another. Repeated consecutive messages are deduplicated, and the local-storage warning is only queued once if browser storage is unavailable.
- The initial peaceful-pond message uses the same lifecycle. All event-message producers route through the queue; the separate bottom toast notifications keep their existing behavior.
- Inline JavaScript syntax compilation passed. GitHub Pages publication and live browser timing have not been independently verified.


## 23. Cozy pond-first interface redesign (2026-10-16)

- Refined the full-screen pond UI toward a soft storybook/cottagecore look: a compact floating title plaque with separate round utility buttons, two small stacked status lozenges on phones, and a warm translucent bottom dock with a prominent “Find a duck” action and three clearly labeled secondary actions.
- Removed several ornamental pseudo-elements from the active HUD and action controls so they no longer compete with pond life. Reduced hard-edged/card-like visual weight, retained cream paper, warm wood, sage, and rose tones, and used softer organic corner shapes and subtle pressed states.
- Preserved the full-viewport pond canvas, all existing controls/IDs, event-message queue/fade behavior, modal layouts, game mechanics, and save data. Responsive rules adapt the HUD and controls for narrow phones, short screens, and wider displays.
- Inline JavaScript syntax compilation passed. This is a CSS presentation change; live visual inspection on device and GitHub Pages deployment have not been independently verified.


## 24. Sketch-led pond interface layout (2026-10-16)

- Reworked the main HUD to follow the user's sketch: Abby's Pond title centered at the top, money and flock statistics at opposite upper corners, and four separate utility/action buttons docked along the left and right edges.
- “Find a duck” and “Pond upgrades” sit on the left; “Your flock” and “Keepsakes” sit on the right. Buttons use compact, touch-friendly organic plaques with clear labels and simple icon medallions. The center of the screen remains open so the pond and ducks stay the visual focus.
- Removed the grouped bottom action tray for the main game screen while preserving all action button IDs/data-menu attributes, existing menus, settings, love action, queued event messaging, gameplay, and save data. Responsive rules shrink side buttons on small/short displays and expand them on larger screens.
- Inline JavaScript syntax compilation passed. Live device layout and GitHub Pages deployment have not been independently verified.


## 25. Header control spacing fix (2026-10-16)

- Reserved a dedicated centered title lane between the upper-left heart control and upper-right settings control. The title scales down and wraps only as needed on narrow screens; the buttons remain independently anchored at the corners with their own tap targets.
- Kept the sketch-led side-button layout and all game behavior unchanged.
- Inline JavaScript syntax compilation passed. Live device visual verification has not been independently performed.


## 26. Removed redundant heart control (2026-10-16)

- Removed the heart button from the header because tapping the pond already triggers the same interaction. Removed its click listener as well, avoiding a duplicate path for the same action.
- Recentered the title within the remaining header space while keeping the settings button in the upper-right. All pond-tap effects and hidden tap-secret behavior remain intact.
- Inline JavaScript syntax compilation passed. Live browser/device verification has not been independently performed.


## 27. Four-button bottom dock (2026-10-16)

- Moved Find a Duck, Pond Upgrades, Your Flock, and Keepsakes into one evenly spaced horizontal row at the bottom of the game screen, using responsive button widths and safe-area-aware bottom spacing.
- The row sits below the central pond viewing area with a small gap; event messages are positioned just above it. Buttons retain their original IDs and menu actions.
- Inline JavaScript syntax compilation passed. Live visual/device verification has not been independently performed.


## 15. Nature scrapbook UI direction (2026-10-16, Session 5)

The active visual direction is **nature scrapbook with balanced texture and an illustrated storybook UI around the pixel-art pond**. This is a deliberate refinement of the existing cottagecore direction, not a replacement of the pond's pixel-art rendering.

- Treat the home screen like a page from a personal nature journal: warm uncoated paper, gentle paper grain, pressed-flower/botanical accents, hand-inked dividers, small taped or stitched details, and illustrated labels.
- Use texture in moderation. The UI should feel tactile at a glance without making text, stats, touch targets, or the pond harder to read.
- Avoid repeating the same card shape and border treatment everywhere. Give the title, currency/duck stats, primary duck invitation, secondary navigation, and menus different but related visual treatments.
- Use illustrated storybook ornament around the pixel-art pond; do not repaint or replace the game world with vector illustrations or photographic textures.
- Keep the pond as the main visual focus. Decorative marks must be subtle, never animate distractingly, and must not cover ducks or essential gameplay.
- Keep the primary duck invitation visually distinct from secondary actions. Preserve all button actions and menu behavior.
- Rework the existing CSS coherently with a final, maintainable override layer rather than adding another pile of conflicting style experiments. Remove obsolete styling only when it can be done safely without altering gameplay or selectors.
- Preserve the existing font pairing (Lora for storybook headings, Nunito for readable interface text, Caveat for handwritten accents), with robust fallbacks.
- Preserve mobile-first layout, iOS safe-area handling, minimum touch target sizes, reduced-motion support, modal scrolling, save data, economy, duck wheel odds, and all game mechanics.
- Validation must include JavaScript syntax checking and inspection of the final home-screen/menu markup and responsive CSS. A real iPhone Safari visual check remains necessary if no live device/browser is available.


## 28. Hand-drawn torn-paper action buttons (2026-10-16)

- Replaced the generic symbol badges on all four main actions with individual inline hand-drawn SVG illustrations, each designed for its purpose: a magnifying glass framing a duckling for Find a Duck, a watering can and sprout for Pond Upgrades, three duck friends for Your Flock, and a pressed flower/feather journal page for Keepsakes.
- Restyled each control as a separate irregular torn-paper scrap, with restrained paper grain, faint ruled lines, dashed hand-inked detail, slight individual rotations, and distinct natural paper tints. The labels remain readable and the four-button bottom dock remains intact.
- Kept each existing `data-menu` value (roll, upgrades, flock, inventory), action hierarchy, game logic, and save data unchanged. Drawings are inline SVG and require no external image requests or assets.
- Added responsive sizing for narrow phones and short screens, moved the event slip above the paper controls, and respected reduced-motion preferences.
- Validation: JavaScript syntax compilation passed; verified all four menu actions remain present, all four SVG drawings are present, style/script tags are balanced, and CSS braces balance. Live iPhone Safari visual verification has not been performed.


## 29. Header and HUD torn-paper consistency (2026-10-16)

- Matched the Abby's Pond title plaque, settings control, Pond Pennies note, and Your Flock note to the same irregular torn-paper silhouette, ruled paper texture, and drop-shadow treatment used by the four illustrated action scraps.
- Removed their prior rounded card borders and glass-like backgrounds in favor of consistent paper colors, hand-inked dashed accents, and softly irregular edges.
- Preserved the title, settings interaction, live currency/income and flock/collection fields, responsive layout, gameplay, and save data.
- Validation: JavaScript syntax compilation passed; expected menu action attributes remain intact and CSS braces balance. Live iPhone Safari visual verification has not been performed.

## 30. Independent settings corner (2026-10-16)
- Moved the settings control outside the clipped title-paper header so its own shape cannot be cut off by the title's torn-paper silhouette.
- Anchored settings independently in the top-right safe-area corner with a dedicated torn-paper surface, matching the scrapbook styling while preserving its existing ID and settings-menu behavior.
- Kept the title centered and separate, and preserved gameplay, save data, HUD, and bottom navigation.

## 31. Event messages below the HUD (2026-10-16)
- Repositioned the event/status note to a centered lane directly below the Pond Pennies and Your Flock paper scraps, rather than near the bottom navigation.
- Styled it as a compact matching paper note and added shorter-screen responsive positions to keep the message clear of the HUD and controls.
- Preserved the event queue, fade behavior, status semantics, gameplay, and save data.

## 32. Live animal encounters, duck health, and rarity-based attack (2026-10-16)

- Predator encounters are active real-time battles, not a preselected win/loss. The visitor loops quickly around the pond, reverses course on duck contact, and shows its own health bar.
- Each owned duck receives a persistent, randomly rolled base attack stat when its combat data is first initialized: Common 1–10, Uncommon 10–20, Rare 20–35, Epic 35–50, Legendary 50–70, and Mythic 70–100. Level and equipped attack bonuses are added to that base roll.
- Duck health uses each species' existing base-health value, with level and equipped health bonuses. Existing version-1 saves are migrated in place by filling in missing combat fields; the save version and species IDs remain unchanged.
- On predator contact, the duck loses health and strikes back using its attack stat. Health bars appear while a fight is active or when a duck is injured. At zero health, a duck is exhausted and cannot damage the predator until a bug meal restores health; ducks are not permanently lost.
- Successful insect meals restore up to 32% of that duck's maximum health (at least 4 points, capped at full health) while preserving the existing insect-eating and growth behavior. Healthy ducks can continue eating bugs for growth.
- A defeated visitor fades away before the victory event/reward is queued. If the visitor lasts 22 seconds, it escapes and the flock receives a recovery message instead of a victory reward. Existing encounter cooldown, reward table, keepsakes, currency, and auto-save behavior remain in place.
- Validation target: syntax checks, old-save migration, combat collision timing, health caps, bug healing, victory/escape reward gating, and iPhone Safari layout/animation behavior.


## 33. Mini-boss animal encounters (2026-10-16)

- Predator health now scales much more meaningfully with the visitor's tier, the combined attack power of the owned flock, and the number of ducks. Even the snapping turtle has a minimum 35 health, preventing a single high-roll duck from immediately one-tapping it.
- The enemy health bar is slightly larger and easier to read during the fight.
- The active battle window is 30 seconds instead of 22 seconds, giving the flock more time to land repeated hits while keeping the visitor's escape outcome and reward gating intact.
- Individual ducks still take damage on contact and only strike back while they have health; bug meals can restore them. Existing attack rolls, duck health, encounter cooldown, reward table, keepsakes, currency, save format, and scrapbook UI are preserved.
- Validation target: JavaScript syntax, scaled boss health across flock sizes and enemy tiers, combat timeout, and reward/escape behavior. Live iPhone Safari battle pacing and GitHub Pages deployment should be checked in-browser.


## 34. Species-specific encounter movement speeds (2026-10-16)

- Replaced the previous level-based angular-speed formula, which made even the snapping turtle move at a frantic pace, with an explicit per-animal speed value.
- Current relative movement speeds: snapping turtle 0.012 (slow and steady), water snake 0.020, raccoon 0.024, red fox 0.030, hawk 0.038 (fastest), and coyote 0.028. The existing smooth movement, collision response, combat health, and 30-second encounter window remain intact.
- Embedded JavaScript syntax check passed. Actual perceived movement should still be checked in a live browser to fine-tune if necessary.


## 35. Duck knockout animation and injured flock tab (2026-10-16)

- When an enemy attack reduces a duck's health to zero, the duck is marked injured and plays a brief, approximately 0.9-second knockout animation: a small wobble, tilt, and downward slump. The duck is not lost.
- Added two handmade-style tabs inside **Your flock**: **Your ducks** and **Injured ducks**, with a live injured count. The injured tab lists each knocked-out duck, its rarity, and health, and provides a way to jump to that duck's care details.
- Injured ducks remain owned and present in the pond, but cannot deal damage to an enemy or repeatedly trigger enemy collisions while at zero health. A successful bug meal restores health and clears the injured status, moving the duck out of the injured tab automatically.
- The injured flag is stored on the existing duck save object; no save-version bump or save reset is required. Existing duck IDs, rarity-based attack stats, health calculations, bug feeding, growth, enemy rewards, and equipment remain intact.
- Validation: embedded JavaScript syntax compilation passed and the existing menu action wiring was retained. Live iPhone Safari animation behavior and GitHub Pages deployment still need an in-browser check.
