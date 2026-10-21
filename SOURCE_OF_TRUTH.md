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

### Active pond discovery loop (first gameplay expansion)
- While the pond is open, small interactive discoveries should appear directly inside the water at a gentle, non-intrusive cadence. Initial discovery types: a water glimmer (Pond Pennies), a floating keepsake petal (collection item), and tiny tracks (a clue-style discovery).
- Discoveries must be visibly distinct, tappable with comfortable mobile hit areas, and award their reward exactly once. They should not spawn over an open modal or during an active predator encounter. Missed discoveries may expire without penalty; no progress is lost.
- The Keepsakes screen also acts as a Pond Journal, recording total discoveries and counts by type and showing the next collection milestones. Milestones award small, clearly communicated rewards and persist in the existing local save.
- Build this loop incrementally: pond discoveries and journal first; then duck bonding/friendship and more elaborate flock activities. Keep existing recruitment odds, predator combat, economy, save compatibility, and the four main menu destinations intact.

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
- A defeated visitor fades away before the victory event/reward is queued. If the visitor lasts 30 seconds, it escapes and the flock receives a recovery message instead of a victory reward. Existing encounter cooldown, reward table, keepsakes, currency, and auto-save behavior remain in place.
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


## 36. Animated dizzy indicator for injured ducks (2026-10-16)

- Once the duck's short knockout slump finishes, an ongoing animated dizziness indicator appears above its head while it remains injured.
- Three small warm-gold, hand-drawn sparkle stars orbit and rotate around a subtle curved swoosh. Their movement is tied to the existing pond animation frame, so the indicator animates naturally without timers or additional assets.
- The indicator is drawn above the duck and remains distinct from the duck's health sliver. It disappears when the duck recovers and its injured status clears.
- Preserved the existing knockout timing, injured-duck tab, duck save data, combat behavior, and game mechanics.
- Validation: inline JavaScript syntax compilation passed and the indicator is gated to injured ducks after the 0.9-second knockout animation. Live iPhone Safari rendering still needs an in-browser check.


## 37. Tighter dizzy stars and injured-duck pain expression (2026-10-16)

- Tightened the three warm-gold dizzy stars around the duck's head: they now use a smaller orbit, reduced vertical spread, and smaller sparkle shapes, with the curved swoosh tucked closer to the head.
- While injured, ducks now close their visible eye into a soft wince, with small angled brow marks and a subtle downturned expression. Their normal species-specific eye details return automatically once they recover.
- These are renderer-only visual changes. No health, recovery, save-data, combat, or flock-tab behavior changed.
- Validation: embedded JavaScript syntax compilation passed. Live iPhone Safari rendering still needs an in-browser check.


## 38. Centered injured-duck dizzy animation (2026-10-16)

- Positioned the injured duck's dizzy stars and swoosh using the duck's facing direction and the same horizontal offset as its head, rather than centering them on the body.
- This keeps the animation centered over the head whether the duck faces left or right.
- No gameplay, save data, or recovery behavior changed. Embedded JavaScript syntax compilation passed; live browser rendering still needs confirmation.


## 39. Stronger injured-duck pain expression (2026-10-16)

- Reworked the injured face from a smooth, cheerful closed-eye arc into a tighter, angular pinched eyelid with a visible kink.
- Added brows that angle inward and down, a small tension crease, and a warmer cheek mark to distinguish pain from collectible ducks' naturally happy closed-eye expression.
- The face change only applies while injured and disappears on recovery. No gameplay or save data changed.
- Embedded JavaScript syntax compilation passed; live browser appearance still needs an in-game check.

## 40. Flock-wide knockout and predator retreat (2026-10-16)

- If a predator knocks out the last duck still standing, the encounter immediately switches to the escape outcome and the wild animal retreats instead of continuing to attack.
- The encounter resolution displays a specific event message naming the animal: “The wild [animal] overwhelmed your flock. Take some time to recover.”
- All ducks remain owned and recoverable in the Injured ducks tab; no duck is deleted and the defeat outcome grants no predator victory reward or keepsake.
- Existing single-duck knockout animations, ordinary timed escapes, enemy defeat rewards, save format, and encounter cooldown remain unchanged.
- Embedded JavaScript syntax compilation passed. The full-fight behavior and event timing should still be verified in a live browser.

## 41. Collision-safe duck arrival positions (2026-10-16)

- When a duck first receives a swim position, its starting angle is selected from sampled points around the pond's swim ring. The selection checks the positions of ducks already placed in the current animation frame and prefers a minimum 33-pixel separation, rather than assigning a fixed angle that can overlap a flock mate.
- The placement check also applies when existing ducks are initialized after a reload or when the pond is first drawn, avoiding the same overlap issue for uninitialized swim states.
- If the flock is too crowded for any sampled point to meet the preferred clearance, the system selects the candidate with the greatest available distance. Existing continuous swim movement and collision avoidance remain intact.
- Embedded JavaScript syntax compilation passed. Live browser placement and small-screen crowding should still be verified in-game.

## 42. Duck-to-duck path avoidance (2026-10-16)

- Updated flock collision handling so two ducks do not reverse swimming direction together when they approach too closely. Reversing both preserved their relative motion and could make them repeatedly converge and bounce off each other.
- A close encounter now makes one duck yield by reversing direction while both ducks share a 2.6-second cooldown, giving them time to separate before another avoidance response can trigger.
- The swim paths remain continuous in position; no teleporting or save-data changes were introduced.
- Kept the pond dimensions unchanged for now. The existing swim area should be evaluated with the improved avoidance before enlarging the pond, so the pond's visual balance is not changed without need.
- Embedded JavaScript syntax compilation passed. Live flock behavior and crowded-pond performance still need an in-browser check.

## 43. Stronger duck sprite separation (2026-10-16)

- Increased the duck-to-duck center spacing threshold from 29 to 37 pixels, accounting for the actual width of the drawn duck sprites so bodies are less likely to overlap and visually cut across one another.
- When ducks approach within that threshold, one duck yields and briefly increases its swim rate while moving away. The other duck keeps its continuous path, avoiding the previous synchronized bounce behavior and helping the pair clear one another faster.
- The yielding boost lasts 1.1 seconds; both ducks have a 1-second collision-response cooldown. Spawn placement automatically uses the larger spacing threshold plus its existing 4-pixel margin.
- No pond-size or save-format changes were needed. Embedded JavaScript syntax compilation passed; live crowded-flock visual testing remains necessary to confirm the sprite spacing on device.

## 44. Faster flock movement and roaming predator path (2026-10-16)

- Ducks swim faster while an encounter is active, helping more of the flock spread around the pond during the fight. A duck that yields during close-quarters avoidance gets a stronger temporary speed burst.
- Changed the predator's path from a near-constant orbit on the same outer ring as the ducks to a varying-radius sweep that crosses more of the pond.
- Removed the predator's automatic direction reversal after each hit. It continues moving through the encounter rather than repeatedly bouncing between the same nearby ducks.
- Preserved combat damage, per-duck hit cooldowns, rewards, encounter duration, upgrades, and save format.
- Embedded JavaScript syntax compilation passed. Crowded-flock combat should still be checked in the live game to confirm that the enemy reaches more ducks on device.

## 45. Stable duck spacing without collision bounce loops (2026-10-16)

- Replaced alternating swim directions and collision-triggered reversals/speed boosts with stable, same-direction swim paths. Ducks begin at spaced points around the swim loop and maintain a shared movement rate, so their relative spacing no longer collapses because one duck turns around or accelerates past another.
- New ducks choose a clear starting angle near their evenly spaced slot when possible. Existing pond dimensions, duck sprites, encounter speed-up, combat logic, rewards, and save format remain unchanged.
- Embedded JavaScript syntax compilation passed. Live visual testing with a large flock is still needed to confirm sprite clearance on device.

## 46. Periodic duck swim formations (2026-10-16)

- Added a flock-level formation controller that periodically switches between the pond-wide circle and a distinct capital-Z formation at randomized intervals (roughly 18–32 seconds between switches after the initial wait).
- In Z mode, ducks occupy the three straight strokes of a large Z: a horizontal top row, a diagonal descending stroke, and a horizontal bottom row. The Z translates gently across the pond instead of following the circular swim ring, and the transition blends over 2.8 seconds.
- All ducks retain the shared movement controller and encounter speed-up. The circle remains available as the alternate formation; no save-format changes.
- Fixed a segment interpolation bug that pinned every duck in the final third of the path to the bottom-right endpoint. Each duck now interpolates across the full bottom stroke, as it already does across the top stroke and diagonal.
- Embedded JavaScript syntax compilation passed. Formation transitions, Z legibility, and sprite spacing still need visual verification in a live browser.
- Improved Z-slot assignment: ducks are now distributed evenly by actual screen-space distance along the entire capital-Z path, rather than splitting the flock into three equal index groups. Because the diagonal stroke is longer than the horizontal strokes, this keeps the gaps more consistent and ensures the ducks progress to their proper positions along the top, diagonal, and bottom strokes. No save-format or gameplay-mechanic changes.
- Added a 30% chance for the flock to start in its fully formed Z arrangement when the formation controller initializes; otherwise it starts in the circle as before. The next formation change is scheduled normally from the starting shape, and no save-format changes were needed.
- Widened and deepened the Z path slightly (from normalized points ±0.32/±0.20 to ±0.36/±0.23) to give ducks more breathing room while preserving the capital-Z shape and distance-based spacing.
- Increased the Z path spacing again (normalized points now ±0.43 horizontally and ±0.28 vertically) to bring the ducks closer to the generous personal-space feel of the circle formation, while keeping the same formation logic and smooth transitions.
- The Z formation now travels around a gentle oval route across the pond: its center moves with the lead duck's swim angle while each duck stays in its assigned position along the Z, mirroring the circle formation's movement without losing the Z shape.
- Embedded JavaScript syntax compilation passed. Please verify the formation visually in-game, especially with different flock sizes, because visual browser testing has not been performed here.



## 47. Expanded animated duck formations (2026-10-16)

- Expanded automatic flock switching from two formations to five: circle, capital-Z zigzag, V-shaped vee, diamond loop, and a shallow curved arc.
- New non-circle shapes distribute ducks along their paths by measured pixel distance, including diagonal segments, to keep spacing even and avoid bunching.
- Each formation moves as a coordinated flock around a broad oval route; the non-circle shapes also drift through a subtle shared heading change rather than remaining fixed on the pond.
- Formation changes blend between the previous and next shape over 2.8 seconds with eased interpolation, avoiding sudden teleports. Existing shared swim speed, predator encounter speed-up, duck limits, and save data remain unchanged.
- New ponds retain a 30% chance to start in the Z formation; otherwise they start in one of the other four shapes. Automatic switching selects a different formation at each change.
- JavaScript syntax compilation passed after the implementation. As with the previous movement work, the new shapes still need visual verification in a live browser at several flock sizes to confirm spacing and transitions.
- Increased the scale of all non-circle formation paths by 25%, widening the Z, V, diamond, and arc layouts while preserving their shapes, measured-distance slot distribution, shared swimming motion, and eased transitions. Circle spacing is unchanged. JavaScript syntax compilation passed; verify the wider formations in-game for pond-edge clearance and different flock sizes.
- Refined formation-specific spacing: widened the V and curved arc paths while preserving their shared movement and eased transitions. Fixed diamond slot assignment so ducks are distributed around the closed loop without assigning both the first and last duck to the same endpoint. JavaScript syntax compilation passed; live visual spacing and pond-edge clearance still need browser verification.
- Added a shared pond-boundary safety clamp to every formation target. Circle and non-circle points are constrained to a conservative inner region of the visible water ellipse, leaving clearance from the shoreline for duck sprites. Since formation transitions interpolate between safe targets, the blended positions stay within that safe region as well. JavaScript syntax compilation passed; live visual verification is still recommended.
- Improved formation swimming so the flock travels farther around the pond instead of lingering near one spot. Circle formations now also translate around the pond while rotating; all formations use a quicker shared swim pace and a wider oval drift. Added synchronized short rest pauses after roughly 9–16 seconds of movement, with rests lasting about 1.6–3.4 seconds. Pond-safe target clamping and smooth formation transitions remain in place. JavaScript syntax compilation passed; visual behavior still needs live browser verification.
- Widened and deepened the V formation and gave it a larger formation-specific scale so ducks distributed near the bottom vertex have more path distance between them and are less likely to overlap. Pond-boundary safety remains active. JavaScript syntax compilation passed; confirm the V visually in-game with a larger flock.

## 48. Predator impact smoke and duck damage flash (2026-10-16)

- Each successful predator-to-duck collision now triggers a small, soft pixel-smoke cloud at the point of impact. The cloud expands slightly, rises, and fades away over roughly half a second.
- The struck duck also flickers briefly for about 420 milliseconds, alternating its opacity to clearly communicate that it took damage. The flash is triggered on each registered hit, including hits that knock a duck out; the existing knockout slump and injured-state visuals continue as before.
- Existing combat damage, per-duck hit cooldowns, predator movement, health bars, rewards, recovery, and save data remain unchanged. The effects use the existing canvas animation loop and require no new assets or timers.
- Embedded JavaScript syntax compilation passed. Live visual timing and layering should still be checked in a browser; no live browser test was performed for this change.

## 49. More readable predator impact smoke (2026-10-16)

- Preserved the existing soft, pale pixel-smoke style while increasing its initial opacity and puff size slightly, and extending its rise-and-fade lifespan from about 520 ms to 620 ms. The effect is more visible at the instant of impact without becoming a large or lingering cloud.
- Embedded JavaScript syntax compilation passed. Live browser visual verification was not performed.

## 50. Combat hit-flash startup and canvas-state fix (2026-10-16)
- Fixed the duck damage-flash effect appearing on all pond assets when a new game starts.
- A duck only qualifies for the flash while it has a real, positive `hitFlashAt` timestamp and is inside the 420 ms flash window; the default unset timestamp no longer triggers a false startup flash when the game clock is near zero.
- Moved the canvas `save()` before the hit-flash alpha change so the temporary opacity is scoped to that duck's draw pass and restored afterward instead of leaking into later pond assets.
- Preserved the existing impact-smoke effect, combat damage, flash timing, and knockout presentation.
- Validation: embedded JavaScript syntax check passed. Live browser visual testing was not performed.

## 51. Enemy defeat and flock celebration feedback (2026-10-16)
- Added a more expressive predator defeat animation: the enemy rises slightly, rotates, compresses vertically while expanding, fades away, and emits a brief ring of tiny sparkles over a 1-second exit.
- When the flock defeats an enemy, nearby confetti pieces pop around the ducks and fall away within a small radius; the ducks perform a short synchronized bobbing cheer before resuming their normal swim behavior.
- Replaced the bottom event-queue defeat announcement with a centered canvas banner over the pond. It displays the victory and reward (including a bonus keepsake when awarded), then fades out over a short interval. Other event notifications continue to use their existing UI lane.
- Preserved the existing battle/reward logic, and the fleeing/defeat states remain distinct.
- Validation: embedded JavaScript syntax check passed. Live browser visual testing was not performed.

## 52. More noticeable victory celebration (2026-10-16)
- Extended the flock's cheer bob from about 1.05 seconds to 1.45 seconds and increased its bounce height so the ducks' reaction reads more clearly.
- Increased the localized confetti burst from four to six pieces per duck, widened the burst slightly, and made the pieces larger, more energetic, and longer-lived.
- Gave the enemy's defeat exit an extra quarter-second and a more pronounced rise, rotation, and squash/stretch before fading away.
- Extended the centered victory banner from 2.6 to 3.2 seconds before it fades out.
- Validation: embedded JavaScript syntax check passed. Live browser visual testing was not performed.


## 53. Cheerful duck expressions during victory celebrations (2026-10-16)

- During the existing victory cheer, healthy ducks now switch from their normal species-specific eye to a clearly visible happy, closed-eye smile.
- Added a lifted brow, warmer rosy cheek with tiny highlights, and a small curved smile accent so the emotional change reads at pond scale alongside the bounce and confetti.
- The expression lasts for the same 1.45-second celebration window, then returns automatically to each species' normal face. Injured ducks keep their existing pain expression.
- No combat, rewards, duck stats, save data, or celebration timing changed.
- Embedded JavaScript syntax compilation passed. Live browser visual testing was not performed.


## 54. Session 7 — bespoke handcrafted storybook UI system (2026-10-16)

### Approved art direction
- Overall style: **handcrafted storybook**, with balanced illustrated detail and pixel-art accents. The UI should feel like one illustrated object collection designed for this pond, not a stack of generic cozy-game widgets.
- Decoration level: richly detailed botanical borders and custom panel designs, balanced by readable negative space around controls and copy.
- Visual language: warm paper, muted ink, moss/sage, pressed-flower rose, pond-glass blue, honeyed ochre, hand-inked linework, tiny imperfect marks, and restrained pixel accents that echo the pond's pixel art.
- Each UI paper surface should feel individually made. Torn notebook edges must be procedurally varied per element (including their little notches, fibers, and corner wear), rather than reusing one identical clip-path or a static page image. Variation must remain subtle enough not to damage legibility, touch targets, or text.
- Add small lively, hand-drawn pond-world doodles where they help the composition: duckling profiles, sprigs, clover, lily pads, water ripples, tiny stars, seed heads, and pressed petals. Doodles should be custom-feeling, varied in placement and scale, and integrated into the paper surfaces instead of pasted on uniformly. They must not compete with content or imply a non-functional control.
- Maintain a coherent shared system across the pond HUD, home navigation, modal headers, cards, upgrades, flock/duck detail, inventory, collection wheel, settings, status notes, empty states, and confirmations. Use a consistent type hierarchy and a shared family of hand-drawn strokes, botanical corner motifs, paper treatments, and button states.
- Keep gameplay rendering and the pond itself visually dominant. UI detail frames and supports the scene; it must not obscure ducks or make essential information hard to scan.
- Mobile-first constraints remain unchanged: iPhone Safari, safe areas, comfortable 44px minimum primary touch targets, readable contrast, scrollable menus, responsive narrow-screen layout, and reduced-motion support.
- Preserve all gameplay rules, economy, duck and inventory data, collection odds, combat, formations, and the existing save key/schema. This UI pass must not reset or migrate saved progress.
- Do not add remote image dependencies, generic emoji as a substitute for bespoke art, placeholder decorations, or inert controls. Any decorative doodles should be lightweight inline vector/CSS artwork or procedural marks and remain crisp at phone scale.

### Session 7 implementation contract
1. Establish the shared tokens, component treatments, and illustration rules in this document before implementation.
2. Apply the design consistently to existing UI surfaces without rewriting gameplay systems.
3. Generate per-surface torn-edge variation at runtime, with a stable restrained range and safe fallback for reduced motion or unsupported features.
4. Validate JavaScript syntax and inspect all existing menu render paths for CSS regressions. Live iPhone Safari verification remains a separate check if no browser is available.

## Session 7 follow-up — Integrated HUD and tactile home controls (2026-10-16)

The home screen now uses a compact, shared status bar for Pond Pennies and Your Flock. Each status uses a custom inline SVG coin or duck silhouette, while the existing live values remain bound to the original `money`, `income`, `count`, and `collection` elements. The settings control is integrated into the title row.

The four existing menu actions remain their original buttons and retain their original `data-menu` destinations: Find a duck (`roll`), Pond upgrades (`upgrades`), Your flock (`flock`), and Keepsakes (`inventory`). Their visual treatment is now a four-column row of distinct, tactile wooden/ceramic tiles with layered depth, inset highlights, readable high-contrast labels, and a clear pressed state. Keep these real controls connected to the existing menu system; do not replace them with decorative demo buttons.

The pond container blends into the moss/grass palette with a restrained green halo rather than a thick, high-contrast frame. The canvas and all existing pond-rendering layers remain intact. Duck health indicators are now small floating rounded pills; their displayed fill eases toward the real health ratio each frame, without changing combat health, damage, healing, encounter logic, or persisted duck data.

The UI refinement is presentation-only. Preserve `abbys-pond-save-v1`, all current save migration/validation, economy, offline earnings, duck identities and rarity odds, upgrades, equipment, collection, inventory, predator behavior, combat rewards, duck and strider animation, canvas rendering, tap-to-ripple behavior, and existing modal actions. The title row, compact HUD, pond frame, and bottom tiles must not use the generic torn-paper clipping treatment; retain the per-instance handcrafted paper treatment for appropriate modal/content panels. Continue to support iPhone Safari safe areas, narrow widths, touch input, keyboard focus, and reduced-motion preferences.

Validation performed for this commit: repository content checks confirmed the four original action buttons and their menu destinations remain in place, the original live HUD IDs are retained, and the new health-pill easing and custom SVG icons are present. A live iPhone Safari visual/playability pass is still required; do not treat source-level checks as proof of final device appearance or deployment completion.

### Session 7 menu visibility correction (2026-10-16)

A legacy CSS block positioned the four `.action` controls with per-button left offsets and bottom-dock positioning. That conflicted with the newer four-column grid and could move the buttons outside the intended visible layout. The current repair explicitly restores the existing `.actions` container to a four-column grid and resets each original button's position, inset offsets, width, height, and margins for normal document flow. It does not recreate or replace the buttons, and retains the existing `data-menu` destinations and delegated click wiring. All four controls must remain visible, tappable, and in the same visual system as the updated HUD. Validate on a narrow iPhone Safari viewport as part of the next live test.

### Session 7 HUD/menu layout correction and visual contract (2026-10-16)

A second layout correction supersedes the earlier attempt to put the four controls in normal document flow. The pond canvas and main app intentionally occupy the full viewport with overflow clipping, so normal-flow controls could end up beneath the header or outside the visible area. The four existing menu buttons are now explicitly docked as a fixed bottom row above the iPhone safe area, with all legacy per-button side offsets and transforms reset. They remain the original buttons and keep the existing `data-menu` destinations and click wiring.

The final home-screen styling uses a compact title/settings row, a paired Pond Pennies / Your Flock status strip with the existing inline coin and duck SVGs, tactile high-contrast wooden/ceramic menu tiles with layered outer shadows and inset bevels, and a pressed state that moves the tile down 3px. The pond remains a full-screen canvas; its edge treatment is a subtle moss-toned vignette/glow rather than a hard oval frame. Health pills continue to be presentation-only and must never mutate combat health.

Do not reintroduce generic torn-paper clipping on the header, HUD, pond, or bottom action row. Keep the content-panel paper treatment for menus where appropriate. Preserve gameplay, save key/schema, menu handlers, duck rendering, and all canvas interactions. Source checks confirm the four original menu destinations and handler wiring remain. Live iPhone Safari rendering and GitHub Pages propagation still require confirmation in the playable build.


## Session 7 final micro-polish (2026-10-16)

- Removed stray list/radio-style corner markers from the four fixed bottom navigation tiles with explicit marker, list-style, pseudo-element, and stray-input suppression. The original four buttons, their custom SVG artwork, and their existing menu destinations remain unchanged.
- Refined floating duck and enemy health bars in the canvas renderer: reduced the bar height from 5px to 4px, added a subtle dark-brown 1px outline (`#3a2e2b`), rounded the bar ends into small pills, and added a restrained drop shadow. Health ratios and combat behavior are unchanged.
- Added a soft offset canvas shadow beneath the central oval pond shoreline, matching the requested `0 8px 24px rgba(0, 0, 0, 0.15)` depth treatment without applying a rectangular shadow to the full-screen canvas element.
- Validation: the edited JavaScript blocks were checked for expected source matches and remain within the existing animation loop. Live iPhone Safari appearance and GitHub Pages propagation still require visual confirmation.


## Enchanted field-journal modal redesign (2026-10-16)

- Restyled the modal backdrop and shell as warm parchment with a restrained wooden/leather shadow, stitched dashed header divider, tactile close control, and scrollable content that respects safe-area spacing.
- Removed repetitive generated corner doodles from modal cards and headers. Cards now use warm cream surfaces, subtle borders and shadows, and consistent 16–20px inner padding; descriptions wrap safely to avoid clipping.
- Standardized modal action buttons to the rosewood/terracotta gradient with a tactile 3D lower edge, pressed state, and clear disabled treatment. Rarity pills now use soft watercolor/wax-seal styling.
- Replaced the native duck selector in Your Flock with horizontally scrolling duck-avatar tiles. Each tile uses the existing `selectduck:` action, preserving selection behavior and save data. Existing keepsake slot selectors remain unchanged.
- Added a brass/wood wheel rim and pointer treatment, plus a soft green water/vine-style collection progress meter. No roll probabilities, economy values, combat, or saved-data fields were changed.
- Source-level checks confirm the custom selector and modal styles are present. Live GitHub Pages and mobile Safari visual behavior still need confirmation.


### Hosted storybook asset pass (2026-10-16)

- Added Google Fonts Patrick Hand alongside the existing Caveat/Lora/Nunito families; modal titles use Caveat and supporting/card text uses Patrick Hand for a warmer hand-lettered journal feel.
- Modal paper now layers a translucent warm wash over the public-domain FreeSVG parchment asset (https://freesvg.org/download/131854); modal header and small card accents use the public-domain botanical divider SVG (https://freesvg.org/download/77185). The CSS retains cream gradients as graceful fallbacks if remote assets fail to load.
- Tightened modal shadow to the requested warm 0 16px 36px rgba(45,30,20,.3) treatment and added a more tactile pressed state for primary modal actions. Reduced-motion users do not receive the transition.
- Asset licensing checked against FreeSVG's public-domain/CC0 notice on the asset pages; Patrick Hand is listed in the Google Fonts repository under the SIL Open Font License. External assets are hosted remotely, so they require an internet connection and their host must remain available.
- No gameplay, duck data, odds, economy, or save schema changed.


### Inline-SVG wood-and-parchment modal pass (2026-10-16)

- Removed the prior remotely hosted parchment and botanical background CSS so modal styling no longer depends on external image assets.
- Added an 8px dark wood journal frame (with a slightly slimmer mobile breakpoint), layered parchment radial gradients and fine grain, and an inline-SVG botanical flourish above modal titles with a matching vine divider.
- Restyled item cards with the requested warm moss/cream border and inset/outer shadows; primary buttons now use the deep terracotta-to-wood gradient, engraved dark lower edge, light lettering, and pressed state.
- Updated collection meters to a moss-green gradient with an inline-SVG leaf accent and restyled selected flock tabs as pressed-paper tags with rounded tops.
- Preserved existing modal structure, duck selector actions, button actions, game mechanics, economy, and saved data. No external image URLs remain in the modal texture styling.
- Source-level checks confirmed the inline SVG ornamentation, frame, button treatment, leaf meter, and selected tab styles. Live iPhone Safari visual verification remains pending.


### Unique modal theme pass (2026-10-16)

- Added menu-specific theme assignment during modal rendering: Find a Duck uses the Meadow Wheel palette; Pond Upgrades uses Botanical Garden; Your Flock uses Duck Field Guide; Keepsakes uses Trinket Scrapbook.
- Added inline SVG corner stamps for each theme (leaf, duck portrait, wax-seal heart, and meadow sunburst) and theme-specific header accents.
- Botanical Garden upgrades use a sage palette, leaf flourishes beside upgrade names, and a moss-green left card border. Duck Field Guide selection tiles use white polaroid frames with alternating slight tilts; health tracks use pond-blue fill and a heart marker.
- Trinket Scrapbook item cards use stitched dashed borders and a ribbon/pin accent; rarity pills use distinct silk-pink Rare, clover-green Uncommon, and coordinated color treatments for other rarities.
- Find a Duck uses a honey-gold header, brass wheel bezel, and a restrained glowing sparkle effect around the spin button. The sparkle animation respects reduced-motion preferences.
- All new artwork is inline SVG data in CSS; no external image URLs were added. Gameplay, probabilities, economy, save data, and existing action handlers remain unchanged.
- Source-level checks confirmed theme assignment and the theme-specific styling hooks. Live mobile Safari visual verification remains pending.


### Modal header and Find-a-Duck wheel spacing fix (2026-10-16)

- Added 36px left inset to the themed modal header text wrapper so the corner stamp no longer overlaps the kicker/subheader or title.
- Increased the Meadow Wheel container padding to 24px 16px and top margin to 16px, with additional spacing and line-height for the decorative “a little bit of luck” heading.
- Constrained the wheel graphic to a 300px maximum width on standard screens and 260px on narrow phones, while retaining its square aspect ratio and centered alignment so it sits comfortably within the brass arch.
- Changes are CSS-only; game mechanics, wheel odds, economy, duck data, and save schema are unchanged. Source-level CSS review completed; live iPhone Safari visual verification remains pending.


### Final field-journal UI/UX polish (2026-10-16)

- Restyled modal cards with a warm paper gradient, softly embossed highlight/shadow, 16px corners, and roomier padding; kept theme accent edging while unifying the underlying paper surface.
- Reworked Your Flock section controls as overlapping fabric/leather bookmark ribbons. The selected ribbon lifts by 3px with a soft drop shadow; the inactive ribbon is slightly muted.
- Added tactile button feedback across primary, secondary, and danger actions: hover scale and light gleam on hover-capable devices, plus a quick pressed-down state. Disabled buttons remain excluded.
- Strengthened modal typography hierarchy with warm chocolate-brown text, cozy handwritten card headings, readable Georgia/Palatino-style descriptive copy, and softly embossed uppercase rarity/cost pills.
- Added a spring-like journal page entrance and a short closing animation. Closing now waits for the page-out animation before hiding the backdrop; reopening cancels the closing state. Reduced-motion preferences are respected.
- No gameplay, duck behavior, economy, probabilities, or save schema changed. Source-level checks confirmed the CSS and modal open/close wiring; live iPhone Safari visual verification remains pending.


### DIY scrapbook field-journal makeover (2026-10-16)

- Reworked the existing modal surfaces into a rustic notebook page using a warm #f7f1e3 paper base, layered paper-grain gradients, a binder-colored edge, and a subtle spiral-stitch track. Existing corner stamps remain intact.
- Restyled cards as hand-placed paper scraps with a warm ivory gradient, inner emboss highlight, soft shadows, generous 16px 20px padding, rounded/irregular corners, alternating slight rotations, and washi-tape strips.
- Reworked action buttons as tactile sticker/rubber-stamp controls with thick dark-brown edges, layered paper shadows, handwritten labels, hover lift, and press-down feedback at translateY(3px) scale(.97).
- Applied handwritten headers, serif notes/descriptions, warm high-contrast roasted-brown text, and paper/wax-seal-style rarity pills.
- Kept menu-specific visual identities: botanical green-wash tape and pressed-leaf feel for upgrades; polaroid-style duck selector tiles for Your Flock; dashed specimen cards and ribbon accents for Keepsakes; and a cardboard/brass pinned wheel for Find a Duck.
- Styled flock section tabs as paper bookmarks with paperclip-like top loops and a lifted selected state. Tightened mobile breakpoints and kept modal content horizontally clipped to avoid side-scrolling.
- The existing HTML layout is retained: .backdrop > .modal > .mhead + .content, with existing .card, .flock-tabs, .duck-choice, .wheel-card, and action button elements restyled through the inline stylesheet in index.html. No gameplay logic, duck data, economy, probabilities, or save schema changed.
- Source-level CSS and structure review completed; live iPhone Safari visual verification remains pending.

- Extracted the scrapbook skin into `scrapbook-ui.css` and linked it after the base inline stylesheet in `index.html`, so the complete new override stylesheet is independently readable/editable while GitHub Pages remains a single-page game.


## Cozy font system polish (2026-10-16)
- Added Google Fonts imports for Caveat (600/700), Patrick Hand, and Quicksand (500/600/700) at the top of `scrapbook-ui.css`, and updated the early font import in `index.html` so these fonts are requested consistently.
- Enforced a global cozy font stack on all elements, including nested UI and browser-native controls, with explicit inherited font declarations for buttons, inputs, selects, options, textareas, role buttons, and tabs.
- Applied Caveat to display headings, tab labels, and primary action labels; Patrick Hand to descriptions, helper notes, rarity tags, HUD labels, and numeric/stat text. Increased baseline UI text to 1.05rem and key stat values to 1.2rem for mobile readability.
- Updated the stylesheet cache-buster. This is a typography-only change; gameplay, duck data, economy, probabilities, and save schema remain unchanged. Live iPhone Safari verification is still pending.


## Font sizing and overflow safeguards (2026-10-16)
- Added wrapping and responsive sizing safeguards for the cozy font update so longer button labels, descriptions, stats, rarity tags, and flock tabs wrap instead of being clipped on narrow screens.
- Kept journal content vertically scrollable within the existing modal height cap, tightened mobile modal padding, and allowed action buttons to grow in height if their handwritten labels need a second line.
- Updated the stylesheet cache-buster. These are presentation-only changes; gameplay behavior and saved data are untouched. Live device verification remains pending.


## Find a Duck breadcrumb-lure minigame (2026-10-16)
- Replaced the visible prize wheel with a picnic-blanket breadcrumb lure scene inside the existing Find a Duck journal page. The illustrated scene includes a gingham blanket, crumbs, wildflowers, grass, and water ripples.
- The Toss Breadcrumbs action runs a 1.5-second rustling/ripple animation (shortened for reduced-motion accessibility) before revealing the result in a taped Polaroid-style card. Duck cards show name, rarity, and base attack/health stats; new ducks waddle/pop into view.
- Existing luck-upgrade rarity weights, invitation cost, duplicate compensation, collection data, and save schema are preserved. Added a 10% consolation outcome for a Keepsake Petal or small Pond Pennies pouch; this makes 90% of tosses duck outcomes, with the displayed rarity percentages applying conditionally to those duck outcomes. Duplicate ducks still award a Keepsake Petal and the existing 15%-of-cost penny bonus.
- Updated the stylesheet cache-buster. All changes are UI/roll interaction logic only; no save schema migration was introduced. Live iPhone Safari verification remains pending.


## Find a Duck picnic layout and reveal polish (2026-10-16)

- Added 28px left padding to the Meadow modal content to keep text and cards clear of the scrapbook binder edge, with extra top spacing before the “THE LITTLE PICNIC” heading.
- Restyled the meadow illustration as a rounded 18px window with a 3px sage-green border and clipped contents. The gingham blanket now has softer rounded corners and a cloth-like shadow.
- Moved “a little snack, with love” into its own caption below the illustration. The idle scene is intentionally pared back to meadow grass, gentle water ripples, the picnic blanket, and scattered breadcrumbs; decorative flowers and the extra snack-leaf mark were removed, and the lured duck appears only in its Polaroid reveal card.
- Refined the “VISITING FRIEND” / new-friend reveal card with a creamy #fffdfa paper surface, subtle shadow, taped top edge, and more breathing room around the duck, name, rarity, and stats.
- Moved the primary “Place Breadcrumbs” action to the end of the scrollable content and made it sticky near the bottom while scrolling. The button uses a tactile wood gradient, engraved lower edge, press-down feedback, disabled styling, and reduced-motion support.
- Updated the stylesheet cache-buster. Existing lure rewards, rarity odds, invitation cost, duplicate compensation, duck collection, and save schema are unchanged.
- Source checks confirmed the idle scene no longer contains a duck sprite, the Polaroid markup remains the only duck-reveal location, the new button and styling hooks exist, and the updated cache key is linked. Live iPhone Safari appearance and GitHub Pages propagation still need visual confirmation.


## Find a Duck storybook reveal animation rewrite (2026-10-16)

- Reworked the lure interaction into a staged sequence: breadcrumbs and illustrated reeds rustle for 1.5 seconds; for duck outcomes, a visitor sprite slides into the picnic scene, performs a small idle hop/pause for one second, and only then reveals the Polaroid card.
- Added static illustrated reed/cattail clusters around the sides and front of the picnic window. During the lure phase they sway gently between -3 and 3 degrees, while breadcrumbs glint in a staggered sequence.
- Removed the rubbery grass/blanket animation and removed all skewX, skewY, and scaleX CSS from the stylesheet. The blanket remains still; the grass no longer warps.
- Replaced the abrupt reveal with a soft upward paper-unfold animation for the Polaroid, followed by a subtle stamp appearance. The visitor uses translate-based movement and a small idle hop, without scale/stretch effects.
- Non-duck consolation rewards retain their existing rewards and reveal after the 1.5-second lure phase; duck rewards, duplicate compensation, rarity weights, costs, save schema, and collection mechanics remain unchanged.
- Added reduced-motion CSS handling and refreshed the stylesheet cache-buster. Source checks verified the sequence hooks, reed and crumb animations, delayed card class, and absence of skew/scaleX effects. Live iPhone Safari visual verification remains pending.


## Find a Duck scratch-off field notes replacement (2026-10-16)

- Replaced the breadcrumb picnic and lure interaction with a weathered field-journal page titled “Uncharted Footprints,” containing three tactile muddy-seal scratch panels.
- A player chooses one panel by tapping or pressing/dragging across it. The selected seal peels away with a soft paper-like fade; the other two panels fade and lock, preventing multiple rewards from one invitation.
- The revealed reward appears within the selected panel: duck Polaroids show the duck, rarity, attack and health stats; alternate rewards show Keepsake Petals or Pond Pennies.
- Preserved the existing invitation cost, luck-upgrade rarity weights, 90% duck / 10% consolation split, duplicate petal and 15%-of-cost penny compensation, collection behavior, reward amounts, and save schema. No save migration was introduced.
- Added responsive, handwritten-font styling, textured moss/grey seals, paper grain, torn journal edges, tactile feedback, and reduced-motion support. Removed the obsolete picnic-lure stylesheet block and refreshed the stylesheet cache key.
- Source-level checks passed before commit; live iPhone Safari appearance and GitHub Pages propagation still need verification.


## Scratch-off UI regression hotfix (2026-10-16)

- Fixed a regression introduced during the Find a Duck replacement: the replacement block accidentally removed the shared `rolling`, `lureOutcome`, and `flockTab` state declarations used throughout the UI. Restored those shared variables so modal navigation and all menu actions can reference them safely.
- Restored the `lureOutcomeMarkup` renderer used by the scratch-off reward panel so the selected note can display a duck Polaroid or consolation reward.
- Existing scratch reward flow, economy, save schema, and other menus remain unchanged.


## Pond navigation regression follow-up (2026-10-16)

- Follow-up testing showed that Find a Duck worked, but settings, upgrades, flock, and keepsakes did not render. The scratch-off replacement had inadvertently removed the four existing menu renderer functions (`settingsMenu`, `upgradeMenu`, `flockMenu`, and `inventoryMenu`). Restored the original menu renderers from the pre-replacement source while leaving the new scratch-off flow intact.
- Source checks confirmed all four menu renderer declarations are present again. Live iPhone Safari verification is still pending.


## 23. Session 9 — warm handcrafted visual polish (2026-10-17)

- Keep the home scene's existing pond, ducks, lily pads, canvas coordinates, animations, and gameplay logic unchanged.
- Warm the surrounding meadow with a soft pastel-green gradient using the page background, without full-screen pseudo-element overlays.
- Style resource counters as warm paper tags and bottom navigation as tactile painted/wooden tiles with restrained texture, clear dark typography, layered physical shadows, and a pressed scale state.
- Add a warm, layered shoreline frame through the existing pond element's border/outline/shadow only; it must not cover or intercept the water canvas.
- The water, duck sprites, and lily pads are drawn inside the existing canvas. CSS must not scale or transform the canvas to fake individual ripples, as that would move the rendered scene relative to its interaction coordinates and crop the organic shoreline. Preserve the renderer's existing in-pond ripples and slow water/pad motion; only add per-sprite CSS animation if those sprites become separate DOM elements.
- Avoid mix-blend-mode and any new pond-container pseudo-element overlays. Do not change element IDs, event handlers, game logic, save data, canvas sizing, or interaction geometry.
- Validate CSS and inline JavaScript syntax, confirm all existing IDs/handlers are unchanged, and inspect the live page before claiming visual verification.

## Formation transition stability (2026-10-21)
- Formation changes snapshot every currently rendered duck position before switching shapes. The 2.8-second eased transition now starts from those real positions and converges each duck into its new evenly spaced slot, instead of interpolating incompatible positions along unrelated formation paths.
- Each duck caches its last rendered coordinates for the next switch. Newly added ducks without a snapshot safely fall back to the current formation path; transition snapshots are cleared when the blend completes.
- This is intended to prevent flock shearing/separation during formation changes, especially with larger flocks. JavaScript syntax checked; visual gameplay verification remains pending.
