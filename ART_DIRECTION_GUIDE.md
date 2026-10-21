# Abby's Pond — Complete Art Direction & Visual Guide

**Status:** Proposed visual direction for implementation. This guide establishes the target look; it does not claim that the art changes described here are already implemented.
**Target:** iPhone Safari first, responsive desktop support. Keep the existing game mechanics, pond movement, and gameplay readable while upgrading the presentation.

## 1. Core art direction

### Chosen style: watercolor-and-ink storybook

Make Abby's Pond feel like a lovingly illustrated nature journal: warm watercolor-like color fields, fine ink contours, hand-shaped silhouettes, gentle paper grain, detailed plants, and expressive cartoon ducks. Use SVG for precise scalable shapes and CSS for lighting, texture, and restrained motion, but soften the vector construction with organic outlines and surface variation.

Do not combine three competing styles. Use polished vector construction as an implementation technique, not as the visible style. Use soft shadows sparingly, but do not make the scene a 3D/isometric diorama. Pixel-art accents should no longer be a prominent visual language; retain them only if a small existing detail truly benefits from them.

### Visual principles

- **Warm, not washed out:** muted greens and blue-greens, cream highlights, earthy brown outlines, and small accents of butter yellow, blush, and flower pink.
- **Organic, not geometrically perfect:** irregular shore contours, varied plant shapes, imperfect leaf edges, and individual duck silhouettes.
- **Detailed, not cluttered:** highest detail on the pond, ducks, and nearby shoreline. Keep the center water readable for gameplay and keep UI away from the main action.
- **Softly illustrated, not blurry:** preserve crisp silhouette edges and readable facial features; use texture inside shapes and around their edges rather than blurring everything.
- **Handmade, not generic:** avoid default translucent rectangles, standard pill buttons, repeated generic circles, and stock-icon aesthetics.
- **Alive, not constantly animated:** most motion should be slow and subtle; save stronger animation for duck reactions, rewards, enemy encounters, and important feedback.

### Palette tokens

| Token | Hex | Use |
|---|---|---|
| Deep pond | `#315F5B` | Deepest water and cool shadow |
| Pond teal | `#477F78` | Main water midtone |
| Shallow water | `#79A99A` | Shore shallows and reflected light |
| Water glint | `#D4E8CE` | Small light ripples |
| Moss | `#687B4A` | Moss, deep foliage, UI accents |
| Leaf | `#91A76A` | Sunlit foliage and lily pads |
| Bank earth | `#80634A` | Soil, roots, wood and warm outlines |
| Ink brown | `#443A30` | Primary outlines and readable text |
| Paper cream | `#F4EBD7` | Journal panels and light UI surfaces |
| Aged paper | `#E5D5B8` | Paper shadows, borders and worn edges |
| Butter yellow | `#EBCB73` | Rewards, flowers and small highlights |
| Berry blush | `#D98F85` | Rare flowers and occasional romantic accents |

Use color variation within each object. The hex values are starting points, not a mandate to fill large areas with flat colors.

## 2. Pond redesign — highest priority

The current perfect cyan oval should become a layered pond nestled into a soft grassy bank. Build the edge from broad irregular curves rather than a mathematically perfect ellipse. The silhouette should have a few gentle coves and bulges, not a noisy sawtooth outline. Preserve a clear water boundary for duck movement and collisions: the art contour may be organic, but gameplay bounds must remain deliberate.

### Layer stack

1. **Ground halo:** uneven dark moss/earth silhouette slightly larger than the water.
2. **Bank:** narrow irregular grass-and-soil ring, with roots, small pebbles, and occasional flowers.
3. **Shallows:** a broken translucent sage-teal band inside the edge.
4. **Water base:** layered deep teal gradients, darker toward selected deeper areas and edges.
5. **Surface light:** low-contrast moving ripple streaks and small, slow patches of reflected sky.
6. **Foreground life:** lily pads, reeds, water striders, and a few floating leaves with shadows.

### Starter HTML

```html
<div class="pond-scene" aria-label="Illustrated pond">
  <div class="pond-bank">
    <div class="pond-water">
      <div class="pond-shallows" aria-hidden="true"></div>
      <div class="pond-ripples" aria-hidden="true"></div>
      <div class="pond-glints" aria-hidden="true"></div>
      <!-- Existing duck, bug and lily-pad gameplay elements render here. -->
    </div>
  </div>
</div>
```

### Starter CSS

```css
.pond-scene {
  position: relative;
  isolation: isolate;
  width: min(100%, 760px);
  aspect-ratio: 1.22 / 1;
  margin-inline: auto;
  padding: 3.2%;
  filter: drop-shadow(0 12px 18px rgba(49, 54, 37, .16));
}

.pond-bank {
  position: absolute;
  inset: 1.5%;
  overflow: hidden;
  border: 3px solid #6e6848;
  border-radius: 47% 53% 45% 55% / 48% 43% 57% 52%;
  background:
    radial-gradient(ellipse at 20% 26%, rgba(205, 190, 130, .42), transparent 28%),
    radial-gradient(ellipse at 78% 72%, rgba(49, 69, 42, .38), transparent 32%),
    linear-gradient(135deg, #9ba875, #6d8051 55%, #7c7651);
  box-shadow: inset 0 0 0 5px rgba(244, 235, 215, .25),
              inset 0 -8px 15px rgba(51, 57, 36, .22);
}

.pond-water {
  position: absolute;
  inset: 3.4%;
  overflow: hidden;
  isolation: isolate;
  border: 2px solid rgba(49, 75, 64, .55);
  border-radius: 48% 52% 51% 49% / 45% 47% 53% 55%;
  background:
    radial-gradient(ellipse at 31% 28%, rgba(154, 195, 169, .62), transparent 25%),
    radial-gradient(ellipse at 72% 68%, rgba(35, 83, 77, .52), transparent 37%),
    linear-gradient(155deg, #78a99a 0%, #54877d 38%, #3c6f68 76%, #315f5b 100%);
  box-shadow: inset 0 0 0 7px rgba(206, 225, 194, .13),
              inset 0 0 26px rgba(33, 67, 59, .22);
}

.pond-shallows {
  position: absolute;
  inset: 1.5%;
  border: 10px solid rgba(190, 211, 163, .14);
  border-radius: 48% 52% 51% 49% / 45% 47% 53% 55%;
  pointer-events: none;
}

.pond-ripples, .pond-glints {
  position: absolute;
  inset: -15%;
  pointer-events: none;
  mix-blend-mode: screen;
}

.pond-ripples {
  opacity: .22;
  background: repeating-radial-gradient(ellipse at 32% 45%,
    transparent 0 23px, rgba(208, 232, 207, .35) 24px 25px,
    transparent 26px 47px);
  animation: pond-ripple-drift 26s linear infinite;
}

.pond-glints {
  opacity: .42;
  background: repeating-linear-gradient(118deg,
    transparent 0 62px, rgba(222, 237, 210, .2) 63px 65px,
    transparent 66px 116px);
  mask-image: radial-gradient(ellipse at 42% 34%, #000, transparent 65%);
  animation: pond-light-drift 19s ease-in-out infinite alternate;
}

@keyframes pond-ripple-drift {
  from { transform: translate3d(-1.5%, -1%, 0) scale(1); }
  to   { transform: translate3d(1.5%, 1%, 0) scale(1.035); }
}
@keyframes pond-light-drift {
  from { transform: translateX(-1.5%); opacity: .25; }
  to   { transform: translateX(1.5%); opacity: .48; }
}

@media (prefers-reduced-motion: reduce) {
  .pond-ripples, .pond-glints { animation: none; }
}
```

**Integration notes:** this CSS is a starting visual treatment, not a finished organic shoreline generator. Its rounded contours are deliberately irregular but still relatively simple. For production, define a custom uneven path with inline SVG (or a reusable SVG mask) so the bank and water have related but non-identical contours. Keep the gameplay collision boundary separate from decorative texture. Check that `mask-image` and blend modes have a graceful fallback in mobile Safari; the water should still look attractive without them.

### Shoreline detail placement

- Put dense reed clusters in three to five selected shoreline areas rather than forming a uniform wreath around the entire pond.
- Place small stones in uneven groups of two to four, with different sizes and muted values. A few can overlap the bank edge.
- Use tiny grass blades, moss tufts, and occasional white/yellow flowers to break up long bare shoreline sections.
- Put more detail on the outer bank than inside the central water so duck sprites remain legible.
- Keep corners and likely duck routes open enough that reeds and plants do not visually trap the flock.
- Give bank objects tiny contact shadows, and avoid repeating the same exact cluster with identical rotation.

### Textured lily pads

Lily pads should read as leaves, not green dots. Each pad needs a gently uneven perimeter, a small V-shaped notch, subtle color variation, a fine central vein, and one or two branching veins. Vary size, tilt, and color; use two overlapping pads occasionally, but do not make every pad a cluster.

Use an inline SVG for the leaf silhouette and veins. Place a soft shadow behind it with CSS. Add a small pale reflected edge on the sun-facing side and a darker lower edge. The leaf itself should be muted olive/sage rather than neon green. Keep water-strider pad occupancy reservations and edge-jump logic independent of these art changes.

Example visual structure:

```html
<svg class="lily-pad" viewBox="0 0 100 78" aria-hidden="true">
  <defs>
    <linearGradient id="padFill" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#b5c783"/>
      <stop offset=".58" stop-color="#829c5e"/>
      <stop offset="1" stop-color="#526f49"/>
    </linearGradient>
  </defs>
  <path class="pad-leaf" d="M50 39 L84 12 C98 28 91 57 69 68 C45 82 15 68 8 46 C2 25 21 7 43 9 C54 10 64 16 70 24 Z" fill="url(#padFill)"/>
  <path d="M50 39 L43 10 M50 39 L17 29 M50 39 L24 57 M50 39 L68 64 M50 39 L83 22" fill="none" stroke="#d4dda2" stroke-opacity=".58" stroke-width="1.5" stroke-linecap="round"/>
</svg>
```

```css
.lily-pad {
  overflow: visible;
  filter: drop-shadow(0 3px 2px rgba(30, 55, 42, .24));
  transform-origin: 50% 55%;
  animation: pad-float 6.5s ease-in-out infinite alternate;
}
.lily-pad .pad-leaf {
  stroke: #536a43;
  stroke-width: 2;
  stroke-linejoin: round;
}
@keyframes pad-float {
  from { transform: rotate(-1.2deg) translateY(0); }
  to { transform: rotate(1.2deg) translateY(-2px); }
}
```

Use unique SVG IDs or define the gradient in a shared reusable SVG symbol carefully; duplicated literal IDs in many inline SVGs can cause gradient collisions. Stagger pad animation delays and keep motion under a few pixels.

## 3. Duck sprites — turn shapes into characters

Every duck should feel like a small illustrated animal, not a colored ellipse with a beak. Preserve each duck type's recognizable silhouette and rarity, while giving all types the same rendering language.

### Anatomy and rendering

- **Body:** a plump but species-specific silhouette with a softly varied belly, shoulder, and rump. Avoid a perfect oval.
- **Feather texture:** two or three low-contrast feather strokes or small feather group shapes on the wing and tail. Texture must not look like noisy fur.
- **Wing:** a clearly separated wing shape with a curved feather edge and a slightly lighter top plane.
- **Face:** small, expressive eye with a warm dark pupil and tiny catchlight; optional brow/cheek marks where appropriate. Keep it readable at normal gameplay scale.
- **Beak:** custom shape and color per type, with a small highlight and clear upper/lower contour where scale allows.
- **Feet:** usually mostly hidden in water, but visible during paddling or special poses. Add subtle water movement rather than permanently exposing feet.
- **Water contact:** soft contact shadow below the body and small ripples around the lower silhouette.
- **Outline:** warm brown, around 1.5–2.5px at a 100px reference sprite, adjusted proportionally for actual display size.

### Duck identity through silhouette

Give each species at least two distinct identity features, not just a new body color: short/stubby vs. long body; round vs. swept tail; crest vs. smooth head; broad vs. narrow wing; speckles, neck band, mottling, or a distinctive facial patch. Keep rarity readable through richer patterning, a more distinctive silhouette, or tasteful ornament—not a blinding glow on every rare duck.

### Core poses and idle cycle

1. **Paddling:** body glides smoothly; subtle body bob; small V-shaped wake behind the duck; feet briefly sweep backward below the water surface.
2. **Resting/sleeping:** eyes close, neck tucks slightly, body settles lower in the water, and a tiny bubble or slow breathing motion may appear occasionally.
3. **Preening:** head turns back toward the wing, beak makes two or three short grooming movements, wing lifts slightly, then the duck returns to swimming.
4. **Looking around:** short head turn and blink, with a tiny pause before resuming movement.
5. **Eating an insect:** type-specific head dip or neck stretch, quick peck, then a pleased expression; restore health only according to the existing gameplay rules.
6. **Celebration:** visibly cheerful face, small wing lift or bounce, and a localized burst of confetti; keep this distinct from normal idle motion.

Animations should be state-based and non-blocking. Do not reset movement positions or formations to play an idle pose. Blend the pose over the existing swim path, and ensure hit, death, and victory animations can interrupt lower-priority idle animations cleanly.

### Duck sprite quality checklist

- Distinct silhouette at small size.
- Eyes and beak remain visible on phone screens.
- Wing and body are separable through shape and value, not outline alone.
- Feather marks follow the body curvature.
- Shadow and water contact match the pond lighting.
- Animation loops do not pop at the seam.
- Each rarity and species remains recognizable without relying on a text label.

## 4. UI and button design — a handmade field journal

The UI should feel like the player is tending a pond through a beautifully kept nature journal. Use warm paper cards, botanical linework, pressed-leaf details, hand-inked separators, and small wood or ceramic controls. Do not place large opaque panels over the pond unless a modal is open.

### Surface system

- **HUD headers:** warm cream paper strips with thin brown ink outlines and small illustrated symbols.
- **Modal panels:** pale field-journal pages, subtly layered with a slightly darker paper edge, imperfect corner detail, and a small leaf or pin motif.
- **Action buttons:** carved wooden tags for primary actions; smooth painted ceramic or paper tabs for secondary actions.
- **Danger/destructive actions:** understated muted brick red with explicit wording and confirmation.
- **Selected state:** a pressed-in shadow, darker painted fill, or small leaf marker—not a bright neon border.
- **Disabled state:** reduced contrast and a clear explanation of what is needed, without becoming unreadable.

### Typography

Use a readable rounded sans-serif for body text and values, with a friendly handwritten display face for short headings only. Keep numbers (Pond Pennies, costs, stats) in a highly legible face with stable character widths. If loading a web font, provide a system fallback and avoid making gameplay wait on an external font.

### Button HTML

```html
<button class="journal-button journal-button--wood" type="button">
  <svg class="journal-button__icon" viewBox="0 0 24 24" aria-hidden="true">
    <path d="M12 21s-8-4.8-8-11a4.5 4.5 0 0 1 8-2.7A4.5 4.5 0 0 1 20 10c0 6.2-8 11-8 11Z"/>
    <path d="M7 11c2-1 3-1 5 0s3 1 5 0"/>
  </svg>
  <span>Find a Duck</span>
</button>
```

### Button CSS

```css
.journal-button {
  --button-face: #e8d6ae;
  --button-edge: #8a6947;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: .55rem;
  min-height: 48px;
  padding: .7rem .95rem;
  border: 2px solid var(--button-edge);
  border-radius: 11px 9px 12px 8px;
  color: #443a30;
  background: linear-gradient(160deg, #f1e4c7, var(--button-face) 72%, #d8c093);
  box-shadow: inset 0 1px rgba(255,255,255,.55),
              inset 0 -3px rgba(102,76,48,.12),
              0 3px 0 rgba(89,65,44,.22);
  font: 600 14px/1.15 system-ui, sans-serif;
  text-shadow: 0 1px rgba(255,255,255,.25);
  cursor: pointer;
  transition: transform 120ms ease, box-shadow 120ms ease, filter 120ms ease;
}

.journal-button--wood {
  --button-face: #c6a06e;
  --button-edge: #79573a;
  background: linear-gradient(145deg, #dfbd88, #bd905c 78%, #a7794d);
}

.journal-button__icon {
  width: 22px;
  height: 22px;
  flex: 0 0 auto;
  fill: rgba(244,235,215,.18);
  stroke: currentColor;
  stroke-width: 1.6;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.journal-button:hover { filter: brightness(1.035); }
.journal-button:active {
  transform: translateY(2px);
  box-shadow: inset 0 2px 4px rgba(61,45,31,.18), 0 1px 0 rgba(89,65,44,.2);
}
.journal-button:focus-visible {
  outline: 3px solid #477f78;
  outline-offset: 3px;
}
.journal-button:disabled {
  cursor: not-allowed;
  filter: grayscale(.25);
  opacity: .62;
}
@media (prefers-reduced-motion: reduce) {
  .journal-button { transition: none; }
}
```

Use the existing game's button handlers and accessible labels; this markup is a skin example, not a replacement for existing interactions. On mobile, keep interactive targets at least about 44px high, maintain adequate spacing, and keep the bottom action row clear of the pond and iPhone safe areas.

### Icon and glyph vocabulary

Use small custom inline SVG line illustrations with rounded caps. Each icon should use the same warm ink color and similar stroke thickness. Avoid mixing emoji, platform-dependent Unicode symbols, filled Material icons, and thin outline icons in one row.

| Action | Suggested icon | Visual construction |
|---|---|---|
| Find a Duck | Feather or footprint | Two small webbed footprints or a single curved feather |
| Pond Upgrades | Leaf with central vein | One sprout with two uneven leaves |
| Your Flock | Three duck head silhouettes | One larger duck head with two smaller heads behind it |
| Keepsakes | Pressed flower in a journal | Simple flower pressed between two page lines |
| Settings | Small wooden gear | Rounded gear with a small central circle |
| Inventory | Satchel or field pouch | Rounded pouch with flap and button |
| Injured Ducks | Leaf-cross or bandage | Small leaf beside a soft bandage mark; keep the meaning clear |
| Currency | Hand-drawn coin | Circle with a small feather or pond ripple mark |

Use decorative glyphs only when they have a clear meaning. Important button text should never be replaced by an icon alone.

### Layout guidance

- Keep the pond as the visual center and dominant area.
- Keep currency and flock count in a compact paper header rather than large floating translucent boxes.
- Use the established four-button bottom row: Find a Duck, Pond Upgrades, Your Flock, Keepsakes. Make the four tiles consistent in height and visual weight.
- Keep Settings in a small but accessible top corner control, inside safe-area padding.
- Reserve centered floating messages for short events such as predator alerts, rewards, and flock reactions; use a small paper slip rather than a permanent opaque box.
- Modal layouts should feel like a page from a field journal: heading, short intro, contents, then actions. Avoid excessive torn edges, tape, pins, leaves, and shadows all at once.
- For Sticker Peel, preserve the three-card layout and the tiny top-right cost labels; make the stickers feel tactile, with a paper edge and lift/curl animation, while preserving clear cost and roll-count text.

## 5. Surrounding environment — the pond sits in a real little place

The grass around the pond should feel like a small garden clearing rather than a blank green field. Build it in layers, with enough negative space that the pond stays the focus.

### Placement zones

- **Near shoreline:** reed clumps, damp soil, moss, small stones, and a few flowers. Keep dense clusters separated so the bank still reads as a continuous shape.
- **Mid-ground grass:** occasional clover, tiny white flowers, fallen leaves, and sparse grass tufts. Distribute them asymmetrically and avoid a uniform grid.
- **Far corners:** soft shrubs, broad leaves, a small flowering plant, or a subtle stump/branch. Use lower contrast and slightly less detail than the pond edge.
- **Open lawn:** keep large quiet patches. A premium composition needs rest areas as much as detail.
- **Interactive pond edges:** never cover duck sprites, enemy hitboxes, or critical visual feedback with foreground foliage.

### Insects

Use a few distinct insect silhouettes and behaviors: tiny flies drift in loose loops; butterflies move more slowly and occasionally hover near flowers; water striders stay close to the surface and react to lily pads. Give flying insects a tiny body, two faint wings, and a brief glint or shadow when appropriate. Do not make every insect sparkle constantly. Keep collision and insect-eating mechanics independent from decorative wing animation.

### Weather and time of day

Add weather as a subtle scene-wide layer rather than a full-screen effect that obscures play:

- **Sunny:** warm light patch across one side of the pond, brighter water glints, and slightly lifted leaf highlights.
- **Overcast:** softer contrast, muted reflections, and slower-looking light movement.
- **Light rain:** tiny surface rings and occasional droplets, with no dense rain curtain over the ducks.
- **Mist:** a low-opacity band near the far bank only; never hide duck silhouettes or interface text.
- **Evening:** deeper blue-green water, warmer paper UI, and gentle amber highlights. Avoid making the entire screen dark.

Weather should be infrequent or tied to a clear cycle. It should not interfere with taps, readability, or performance. If it is purely decorative, do not imply that it changes gameplay.

## 6. Subtle life and immersion effects

### Water response

- Spawn two or three small ripple rings when a duck begins paddling or changes direction sharply.
- Use a soft V-shaped wake that fades over a short distance behind a moving duck.
- Add a small circular disturbance when a duck eats an insect or lands in the water.
- Keep surface ripples low contrast and slower than duck movement.
- Let lily pads drift and rotate very slightly, with individual phase offsets. Avoid obvious synchronized bobbing.

### Duck feedback

- Small head turns, blinks, body bobs, and occasional preening keep idle ducks alive.
- Keep each animation short, with varied delays so the whole flock does not move in unison.
- Use brief, localized impact flashes or puffs for combat. Victory confetti should stay close to the celebrating ducks and be visually distinct from ambient particles.
- Make health restoration, growth, or rewards readable with a brief icon/number cue and restrained motion, not a large permanent glow.

### Lighting

Use a very subtle scene tint or gradient to suggest time of day. The tint should shift over a long interval and should not reduce text contrast. Keep lighting layers behind UI and avoid heavy blur filters on the entire game scene, especially on mobile Safari.

### Sound and motion respect

Visual polish should be paired with restrained sound if audio exists: soft water movement, light foot paddles, and gentle reward cues. Always respect `prefers-reduced-motion`; reduced motion should disable decorative drifting, long transitions, and parallax without disabling gameplay feedback.

## 7. Performance, mobile and implementation guardrails

- Prefer a small number of layered CSS backgrounds and SVGs over many full-screen high-cost filters.
- Avoid animating large blurred shadows or `filter: blur()` across the entire pond on every frame.
- Use `transform` and `opacity` for decorative motion where possible.
- Make textures optional enhancements: the pond must remain legible if a mask, blend mode, or animation is unsupported.
- Keep decorative elements `pointer-events: none`; never let them block pond taps or bottom buttons.
- Preserve the existing duck movement, formation, collision, lily-pad reservation, insect, enemy, health, reward, and save systems during art-only changes.
- Test at iPhone portrait dimensions, with safe-area padding, and at a narrow viewport where the bottom action row may need tighter spacing.
- Keep a clear contrast between duck sprites and water, especially for dark or blue-colored duck types.
- Use `aria-hidden="true"` for purely decorative SVGs and retain visible text or accessible labels for actions.
- Avoid external image files if the project's established direction is CSS and inline SVG. If original raster illustrations are later introduced, treat them as an intentional asset pipeline rather than pulling in random stock images.

## 8. Recommended implementation order

1. **Pond silhouette and water:** custom irregular bank/water paths, depth colors, slow surface texture. This delivers the largest immediate improvement.
2. **Lily pads and shoreline clusters:** detailed leaf shapes, rocks, reeds, and varied plant groupings.
3. **Duck sprite language:** consistent outlines, body/wing anatomy, species-specific silhouettes, feather details, and water contact.
4. **UI skin:** journal-paper panels, carved/painted buttons, consistent SVG icon family, and clearer hierarchy.
5. **Ambient motion:** wakes, small ripples, insects, plant drift, and subtle lighting.
6. **Weather and time-of-day:** only after the base scene is coherent and performant.
7. **Polish pass:** check readability at actual iPhone scale, animation seams, contrast, safe areas, and reduced-motion behavior.

Implement one layer at a time and inspect the actual GitHub Pages build at phone size before moving on. Do not replace working mechanics just to achieve a visual effect.

## 9. Definition of done

The art overhaul is successful when:

- The pond no longer reads as a perfect cyan oval or flat vector placeholder.
- Water has subtle depth, gentle movement, and enough contrast for gameplay.
- Shoreline details look hand-placed and varied rather than tiled or evenly distributed.
- Lily pads read as leaves with veins, not dots.
- Ducks have species-specific silhouettes, expressive faces, visible wing/body separation, and varied idle behaviors.
- HUD and menus feel like the same handcrafted field journal as the pond.
- The four bottom actions are visually consistent, readable, thumb-friendly, and do not cover the water.
- Ambient detail adds life without competing with the flock.
- iPhone performance, pointer interactions, collisions, game state, and saving remain intact.
- The result feels warm, personal, charming, and intentionally illustrated—not like a generic game UI template.

---

**Implementation note:** This is an art-direction and prototype-code guide. The snippets are examples to integrate into the existing structure, not a claim that the live game has already been redesigned.

## Implementation log — Session 9 visual pass (2026-10-17)

This pass implements a first substantial slice of the guide's pond and journal direction while preserving the existing gameplay renderer and event handlers.

- Replaced the bright blue water palette with the guide's muted deep-pond / pond-teal / shallow-water family (#315F5B, #477F78, #79A99A).
- Added slow, low-contrast hand-painted water strokes and reflected-sky patches clipped to the existing organic water path. These are canvas details within the pond, not page or CSS overlays.
- Added a fine ink contour and retained the layered moss, soil, aged-paper, and grass bank rings around the existing organic shoreline.
- Replaced flat oval lily-pad fills with notched leaf silhouettes, shaded leaf gradients, and central/branch veins. Existing drift, collision avoidance, and pad motion remain in place.
- Refined the four bottom actions into a consistent four-tile field-journal row with the existing custom inline SVG illustrations, tactile painted-paper surfaces, legible labels, and pressed states. The first action no longer gets a separate oversized banner treatment.
- Kept decorations attached to the pond canvas or their own UI elements. No new full-screen overlays or mix-blend-mode effects were added by this pass.
- Remaining art-direction work: more hand-varied shoreline plants and stones, more anatomically illustrated duck silhouettes and feather details, and iPhone visual/performance verification. The active renderer still needs a device-size visual QA pass before calling the overall art overhaul complete.


### Session 9C — Pond object boundary correction (2026-10-17)
- Tightened lily-pad center limits and accounted for the full leaf/shadow footprint so drifting pads stay inside the visible, irregular shoreline.
- Tightened water-strider swimming and takeoff safety limits to keep their bodies, legs, and jump animations inside the pond instead of relying on the previous broad ellipse.
- Kept the existing duck formation paths, lily-pad drift/collision behavior, and insect movement intact.
- Commit: `ccc1b072d798eaaca84418b2a90e5c8d6a82dce2`.


### Session 9D — Clean pond edge (2026-10-17)
- Removed the decorative stone ring that was positioned on an ellipse and visibly crossed the organic pond shoreline.
- Corrected the foreground meadow plants' bank-clearance test to use the mirrored side-local coordinates, with a wider clear zone beside the water.
- Kept the pond surface, ducks, lily pads, striders, and game interactions unchanged.
- Commit: `4ded780f5464ae9ced8f0ce8118d4e96c1d93146`.


## Session 9E — Clear the lower pond edge
- Increased the side-bank plant exclusion width and extended its vertical coverage to account for the pond's lower curve on short mobile canvases.
- Applied the same boundary clearance to both leafy ground tufts and narrow reed sprites; no changes to water, ducks, lily pads, or interactions.


## Session 9F — Forest glade framing
- Pulled the inner pair of mirrored trees outward and scaled them down so crowns and trunks stay in the surrounding woodland instead of crowding or slipping under the pond bank.
- Kept the larger outer trees as a forest frame, preserving the open glade around the water and all gameplay/rendering behavior.


## Session 10 — Game menu UI rebuild (2026-10-21)

- Replaced the heavy notebook-page visual treatment across game menus with a coherent, polished game-panel system: rounded moss/cream surfaces, a clear colored header, calm borders, consistent button hierarchy, and subtle tactile pressed states.
- Improved mobile comfort with a bottom-sheet modal, larger close and action targets, safe-area-aware padding, a stable scroll region, and more predictable spacing on narrow screens.
- Rebuilt Keepsakes into a readable Pond Journal dashboard: overall discovery progress, three compact discovery counters, milestone reward rows with locked/claimed states, a keepsake basket, and a two-column duck field guide.
- Added distinct but related header tints for upgrades, flock, discoveries, settings, and the duck invitation flow while keeping a shared game UI language.
- Preserved existing gameplay handlers and state; this pass changes presentation and journal layout, not recruitment, upgrade prices, combat, or discovery rewards.
- Implementation commits: `37f9f252389b276716b2051c3232203c98236206` (journal structure), `e45e81de1f3ca3815341d817d9eef50d6717c20b` (game UI styling), `b1d57c05e8d230cd95d4a79a87175f0cde756ea5` (stylesheet cache refresh).
- Remaining QA: inspect the deployed page at actual iPhone viewport sizes and verify long duck names, scratch-off cards, scroll behavior, and all menu actions visually.


## Session 10 — Notification placement follow-up (2026-10-21)

- Moved transient toast notifications from the bottom safe-area position to the upper play area, below the top HUD and away from the four bottom navigation buttons.
- Restyled notifications as compact, readable cream-and-moss game toasts with constrained width, safe-area spacing, wrapping for longer messages, and a shorter-screen adjustment.
- Updated the stylesheet cache key in `index.html` so the deployed page requests the new CSS.
- Commits: `ec1fc5ee422fedfd3379c02b0c29888aa3797188` (notification styles), `f86a789d523c700fd5602fb0391803ce05707441` (cache refresh).


## Session 10 — Unified notification queue (2026-10-21)

- Unified transient feedback previously split between `say()` (bottom/upper toast) and `queueEvent()` (inline pond event line).
- Both public entry points now feed the same FIFO queue and render through the single `#toast` element, so two messages cannot be visible on top of one another.
- Notifications display one at a time for about 2.7 seconds, fade out, then advance to the next queued message. Consecutive duplicate messages are suppressed.
- The legacy `#event` element is retained to avoid disturbing page structure, but is hidden and no longer used as a notification surface.
- Commits: `44ba243141e563b4c3b0614ac97508be4e91225a` (shared queue), `989393eead718f450699b74064ddcaa434125e8f` (hide duplicate surface), `b1d7ef5ed9d03ea33677e4789ac21fc468243114` (stylesheet cache refresh).
- JavaScript syntax checked successfully; visual deployment verification remains outstanding.

## Session 10 — Unique duck ownership safeguard (2026-10-21)

- Repaired save loading so the flock contains at most one owned duck per species. When older saves contain duplicates, the highest-level copy is retained; duplicate copies' equipped keepsakes are returned to inventory rather than lost.
- Recruitment now checks the actual owned flock as well as the discovered-species list before granting a new duck, so stale collection data cannot create another copy.
- Duplicate invitations continue to grant the existing Keepsake Petal and Pond Penny compensation instead of adding another duck sprite to the pond.
- JavaScript syntax check passed. Live visual/gameplay verification remains outstanding.
- Implementation commit: `78f2772cf36dd1c8d26d7626299aae029bee3629`.

## Session 10 — Enemy combat readability and impact (2026-10-21)

- Active predators now display a pulsing cream-and-rust exclamation warning as they approach a living duck within 82 canvas pixels, making imminent contact easier to anticipate.
- Predator health bar enlarged from 42×4 to 68×9 canvas pixels, while duck health bars remain unchanged at 25×4.
- Enemy collisions now apply directional velocity and a small immediate displacement to the duck away from the predator. Velocity decays smoothly, returning the duck to its established swimming path without changing health, injury, knockout, or reward rules.
- JavaScript syntax check passed. Live visual/gameplay verification remains outstanding.
- Implementation commit: `ba22ab2d4b686275d8e7ceca70f9002ba25e2db9`.

## Duck position continuity correction (2026-10-21)
- Kept pair-spacing corrections in dedicated damped `sepX`/`sepY` offsets, separate from knockback displacement, so each frame starts from the previous corrected position rather than snapping back to formation coordinates.
- Removed per-frame recent-hit array reordering, avoiding unnecessary render-order changes while preserving stable duck identity and draw order.
- JavaScript syntax checked; live visual verification pending.

## Formation containment hardening (2026-10-21)
- Clamp both transition snapshots and destination formation points to the safe pond ellipse before interpolation, preventing stale/off-pond coordinates from becoming launch vectors.
- Bound knockback velocity/displacement and temporary spacing offsets to stop malformed or accumulated offsets from pushing ducks outside the flock's movement envelope.
- JavaScript syntax checked; live visual verification pending.


## Duck movement stability — implementation constraint (2026-10-21)

The pond renderer uses CSS-pixel coordinates for duck positions even when the canvas backing store is scaled for device-pixel ratio. Keep that separation intact. Formation interpolation and knockback determine desired positions; spacing and pond-edge projection are final positional constraints, not additional velocity sources.

For an elliptical boundary, calculate normalized radius `r = hypot(dx / edgeX, dy / edgeY)`. If `r > 1`, project to `(cx + dx / r, cy + dy / r)`. Do not multiply by the ellipse radii a second time. Do not persist pairwise separation/boundary corrections as another movement offset; that feeds the correction into the next frame and can cause jitter or discontinuity. This note documents the implemented correction; visual stability still requires runtime testing across flock sizes and mobile Safari resizing.

- **V/arrow formation:** use the same restrained 1.25 path scale as the other non-circle formations. Do not add a special expansion to the V; its sharp apex and wide wings can crowd ducks against the pond boundary and create visually explosive separation corrections.
