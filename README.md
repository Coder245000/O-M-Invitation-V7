# Omer & Marwa — Premium Interactive Wedding Invitation

A mobile-first Arabic digital wedding invitation based on the supplied reference recording, with a continuous ivory/champagne-gold visual flow and cinematic opening animation.

## What is implemented

- Slow envelope opening. The light ignites on the engraved strokes of the **O** and **M** inside the wax seal, then runs outward along the two engraved flap arms, lighting the arabesque scrollwork as it passes. The top flap alone then swings up, carrying the wax seal with it, and the envelope dissolves into the welcome artwork. See *Opening light* below.
- A continuous, seamless page strip: adjacent floral frames overlap and feather into each other so the pages read as one connected design rather than separate cards.
- The browser background matches the invitation's ivory/champagne paper palette; the old dark/blurry card surround and heavy shadows are removed.
- Subtle independent motion on the bride and groom to give the welcome artwork the same living/approaching feel as the supplied reference.
- Hanging lantern/light animation with a restrained sway and warm breathing glow across the invitation scenes.
- Scratch-to-reveal date interaction retained and refined.
- The date now reads naturally **right-to-left**: **٢٨ | نوفمبر | ٢٠٢٦** (day on the right, year on the left).
- The venue placeholder is displayed in Arabic as **[المكان] - القاهرة، مصر**, without inventing a venue that was not supplied.
- Existing Arabic names, family details, date, time and programme content are preserved.

## Project files

- `index.html` — invitation structure, motion layers, envelope light system and Arabic venue overlay.
- `style.css` — seamless floral-page composition, envelope/light animation, couple movement, lantern movement, responsive layout and scratch styling.
- `script.js` — envelope sequencing, viewport activation and scratch-to-reveal behavior.
- `assets/cover.webp` — envelope artwork.
- `assets/hero.webp` — welcome/couple artwork.
- `assets/family.webp` — family-details artwork.
- `assets/date.webp` — date artwork with day/year visually swapped for right-to-left reading.
- `assets/timeline.webp` — event-programme artwork.
- `assets/letters-mask.webp` — alpha mask of the **O** and **M** letterforms, in `seal.webp`'s own 260×260 space.
- `assets/band-left.webp`, `assets/band-right.webp` — alpha masks of the arabesque relief inside the two flap arms.
- `assets/wash-left.webp`, `assets/wash-right.webp` — soft feathered masks hugging each flap seam.
- `assets/flap-top.webp` — the top flap's outline: a pentagon unioned with a disc around the wax seal.
- `assets/flap-body.webp` — its complement, so the envelope body never paints the flap or the seal.
- `assets/flap-glow.webp` — the same shape, heavily feathered, for the light inside the envelope.
- `assets/paper-body.webp`, `assets/paper-flap.webp` — the engraved artwork itself, lifted off `cover.webp` as a fine relief mask and pre-split by flap and body.

## Venue link — where to change the map URL

Open `index.html` and search for **`VENUE MAP LINK`**. Directly under that comment is:

```html
<a class="venue-link" href="https://maps.app.goo.gl/..." target="_blank" ...></a>
```

Replace the `href` with any Google Maps share link. That is the only edit needed — the
button's gold frame and its Arabic label are printed into `date.webp`, so the artwork stays
the single source of truth for how it looks, and `.venue-link` is only a transparent
hotspot sitting on top of it.

The hotspot's four numbers in `style.css` (`left`/`right`/`top`/`height`) are the button's
frame measured in `date.webp` — currently `26.2%` in from each side, `93.9%` down, `4.7%`
tall. They only need changing if the artwork itself changes, and if you do swap `date.webp`
you must re-measure them or the tap target will drift off the button.

### Page seams

The pages meet edge to edge — nothing is shifted, so nothing is clipped, no arch is ghosted
over the page above, and the venue button at the foot of the date page stays clear.

What crosses each seam is only the flowers. `.seam-bleed` is a strip that repeats the top of
its own page, sits above the seam over the page before it, and is masked by
`assets/seam-mask.webp`:

- **opaque down the outer thirds**, where every page's blossoms are, so the two flower banks
  merge into one arrangement;
- **10% through the middle third**, where every page has its arch. Zero looked correct but
  left a visible tonal step at the join — the family page's foot is much lighter than the
  date page's head. Ten percent softens that step while keeping the arch illegible. Twenty
  percent starts to show the arch outline, which is the thing to avoid;
- **fading out upward**, so the strip dissolves into the page above it.

An earlier version instead pulled each page up over the one before it by 48 px and faded its
top in. That merged the flowers but dragged everything else with it: the arch ghosted over
the page above, the fade ran 74 px against a 48 px overlap so the last 26 px of every page
went translucent against the paper background (the washed-out foot), and it buried the venue
button. Masking the middle of *that* approach doesn't work either — because the page is
shifted, hiding its top also clips the tip off the arch.

Two things to keep in mind:

- **The scroll-in must not move the artwork.** `.stage-art` fades in on opacity and filter
  only. It used to also `translateY(16px) scale(.997)` until the stage went active; with the
  pages butted together that offset would open a gap at every seam.
- **Controls need `pointer-events:auto`.** `.stage`, `.stage-art` and `.invitation-art` are
  decorative and opt out of hit-testing; `.venue-link` and `.scratch-zone` opt back in.

If a future artwork has a wider arch, widen the transparent middle of `seam-mask.webp` or it
will start bleeding upward again.

## Opening light

The glow is not drawn by hand: every mask above was cut from `cover.webp` itself, so the
light lands exactly on the artwork rather than on an approximation of it.

Three layers sit over the envelope, each pinned to the card so they scale with it:

| Layer | Mask | Role |
| --- | --- | --- |
| `.letter-flare` | `letters-mask.webp` | a warm gold wipe across the **O**, **&** and **M** strokes |
| `.orn-flow` | `band-left/right.webp` | the engraved scrollwork lighting up as the wave passes |
| `.seam-wash` | `wash-left/right.webp` | soft spill hugging the fold, so the light reads as volumetric |
| `.flap-top__art` | `flap-top.webp` | the only part of the envelope that moves |
| `.envelope-piece--sheet` | `flap-body.webp` | the body: everything the flap is not |
| `.flap-glow` | `flap-glow.webp` | the light waiting inside, uncovered as the flap lifts |
| `.paper-glow` | `paper-body/flap.webp` | light seeping through the engraving on every flap |

Each layer holds an oversized gradient sheet that slides outward on `transform` alone.
The two flap arms sit at **318.8°** and **41.2°**, and the bright head of each gradient is
placed at **50.7%** of its gradient line, which is the top edge of the wax seal. That is why
the light appears to leave the seal and run to the corners.

Timings live in one block in `style.css` under *opening timeline*; `OPEN_SEQUENCE_MS` in
`script.js` must stay just past the end of that timeline (currently 6500 ms).

If a browser has no CSS mask support the three layers are hidden by an `@supports` guard
and the envelope still opens with its central bloom.

### Adjusting it

- **Faster or slower run:** change the `2.05s` duration on `.orn-flow` / `.seam-wash` and their `> i` children together, then raise or lower `OPEN_SEQUENCE_MS` to match.
- **Flap swing:** the angle in `@keyframes flapSwing` and the delay on `.flap-top`. Keep the curve near-linear and `flapBurn` finishing well before 90°.
- **Backlit paper:** peak opacity in `@keyframes paperGlowIn` and the gradient on `.paper-glow`.
- **Brighter or dimmer arms:** the final `background-color` alpha in `@keyframes ornRun`, and the peak opacity in `@keyframes washRun`.
- **Warmer or cooler light:** the gradient stops on `.orn-flow--left > i` and its three siblings — keep all four in step.
- **Letter colour:** the gradient on `.letter-flare__sweep`.
- **Regenerating masks:** if `cover.webp` is ever replaced, the band and wash masks must be recut; they are tied to this exact artwork. `letters-mask.webp` is tied to `seal.webp`.

### Two things that are easy to get wrong

**The letter mask belongs to `seal.webp`, not `cover.webp`.** The wax seal you see on
screen is `.seal-badge`, which paints `seal.webp` at **20.8%** of the card width.
`seal.webp` is itself a 260 px crop of `cover.webp` covering **27.6%** of the card, so it
is drawn about a third smaller than the seal printed underneath it. A mask cut from
`cover.webp` therefore lands too large and slightly off. `letters-mask.webp` is stored in
`seal.webp`'s own 260×260 space and `.letter-flare` is pinned to exactly the same box as
`.seal-badge` — if you change one, change the other.

**Only the top flap moves, and the seal must travel with it.** The wax seal is *printed on*
`cover.webp`, so any cut that runs through it tears it. An early version clipped the artwork
into four triangles that flew apart and the seal split into quarters. The fix is a pair of
complementary masks:

- `flap-top.webp` is the flap — a pentagon whose diagonals sit on the **outer** edge of the
  engraved bands (so both bands belong to the flap), unioned with **the wax seal's own
  traced silhouette**, grown 15 px.
- `flap-body.webp` is its complement, eroded 4 px so the flap overlaps it and no hairline
  seam can open along the join.

The silhouette matters. It used to be a plain circle of radius 158 px, which is wider than
the seal, so the flap carried a ring of paper around it and a distinct disc was visible
swinging upward. The outline is traced by walking outward from the seal centre along 720
rays and taking the strongest radial gradient — colour thresholding and GrabCut both fail
here, the first catching the seam glow and the second locking onto the flat stamped face
instead of the raised rim.

**There is no seal overlay.** An earlier build drew `seal.webp` on top of the printed seal
at 20.8% of the card width, but the artwork's seal is 27.6% wide, so the overlay was a
shrunken copy and the wax rim appeared twice — once compressed at the overlay's edge and
once in its natural place. The seal is simply part of `cover.webp` now, and `.letter-flare`
is masked at full card scale and pinned to the card rather than to any overlay.

Everything that *is* the envelope — flap, body, seal and letter light — fades over the same
4.40 s–5.40 s window, so nothing is left hanging over the welcome artwork.

The flap hinges on the card's top edge and reaches 40°, fading out well before its underside
could show. That number is not arbitrary: in the reference the wax seal rises about 11% of
the screen height across roughly 1.4 s, at a near-constant rate. Matching the rise matters
more than matching the angle, because more angle over the same time reads as a snap rather
than a swing. `.flap-top` therefore uses a near-linear curve, not an ease-in.

### Making the light read as engraving rather than a lit strip

Two layers do the travelling light and they are easy to get out of balance:

- `.orn-flow` is masked to the arabesque relief, so it lights the **scrollwork lines**.
- `.seam-wash` is a soft halo along the fold that gives the light volume.

If `.seam-wash` is turned up, it drowns `.orn-flow` and the whole band reads as one lit
slab. The wash is deliberately held at about half the ornament's strength, and its mask
profile is narrow (roughly 200 px across the fold, not 370 px). Speed matters as much:
the flow runs over 3.05 s on `--ease-flow`, which is close to linear. The earlier
`--ease-silk` covered two-thirds of the arm in the first third of its time, which made a
60 fps animation look like it was skipping frames — the fix is pacing, not frame rate.

The site is static and has no build step or external JavaScript dependency.

## Preview locally

```bash
python -m http.server 8000
```

Open `http://localhost:8000` on a phone-sized viewport. For QA screenshots of the interior, `http://localhost:8000/?preview=open` auto-starts the opening sequence.

## Publish on GitHub Pages

1. Upload the whole project, including the `assets` directory, to the repository.
2. Open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**.
4. Select `main` and `/ (root)`.
5. Save and wait for GitHub Pages to publish.
