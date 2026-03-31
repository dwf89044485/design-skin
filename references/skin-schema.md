# Design Skin Profile — Schema Reference

The profile is a structured capture of everything that makes a design look and feel
the way it does. It's organized into three layers, from concrete to abstract:

- **Tokens** — hard values you can paste into CSS (hex codes, pixel sizes, font names)
- **Character** — soft qualities that guide subjective decisions (mood, density, voice)
- **Spectacle** — advanced rendering that goes beyond flat CSS (3D, particles, shaders, scroll magic)

Every section is optional. Include what's present in the source; omit what isn't.
The profile should reflect reality, not fill a template.

---

## `meta`

| Field | Type | Purpose |
|-------|------|---------|
| `name` | string | Kebab-case identifier, e.g. `notion-clean` |
| `about` | string | 1-2 sentence aesthetic summary |
| `sources` | string[] | URLs, file paths, or "verbal" |
| `created` | string | ISO 8601 |

---

## `tokens` — The Measurable Layer

Everything here has a concrete CSS equivalent.

### `tokens.palette`

Raw color swatches organized by hue family. These are the primitives that semantic
roles reference.

```
{
  "<hue>": {
    "50": "#hex", "100": "#hex", ... "900": "#hex", "950": "#hex"
  }
}
```

Include as many or few hues as the design actually uses. Fill gaps mathematically
(lighter → mix toward white with decreasing saturation; darker → mix toward dark
blue-gray, not pure black).

### `tokens.roles`

Semantic color assignments. Each role has `light` and `dark` values (derive dark
automatically if only light is provided).

Common roles (include what applies — this is not an exhaustive checklist):

- Brand: `primary`, `primary-hover`, `primary-active`, `secondary`, `accent`
- Surfaces: `bg`, `surface`, `surface-raised`, `surface-sunken`, `overlay`
- Text: `ink`, `ink-muted`, `ink-faint`, `ink-inverse`, `link`, `link-hover`
- Borders: `edge`, `edge-strong`, `edge-subtle`, `ring`
- Feedback: `positive`, `caution`, `negative`, `neutral`

### `tokens.gradients`

Named gradient definitions if the design uses them. Each gradient has type
(`linear`, `radial`, `conic`), angle/position, color stops, and where it's used.

### `tokens.type`

#### `tokens.type.stacks`

Font families with fallbacks, weights available, and Google Fonts URL if applicable.
Keys are roles: `display`, `body`, `mono`, plus any custom role the design needs.

#### `tokens.type.scale`

Named sizes, each with `size`, `lineHeight`, `weight`, `tracking`, and which stack
it uses. Use names that match the design's actual hierarchy — don't force every design
into the same set of names.

#### `tokens.type.character`

How typography is used: is the scale dramatic or flat? Tight or airy tracking?
Heavy or light weight preference? This helps generation match the feel, not just
the measurements.

### `tokens.space`

#### `tokens.space.grid`

Base unit (typically 4 or 8), plus the numeric scale derived from it.

#### `tokens.space.roles`

Purpose-specific spacing: `page-margin`, `section-gap`, `card-inset`,
`control-padding-x`, `control-padding-y`, `stack-gap`, `row-gap`, etc.
Only include what the design actually has patterns for.

#### `tokens.space.density`

`compact`, `comfortable`, or `spacious` — plus a note about section rhythm
(e.g. "alternating 64px/96px section gaps" or "uniform 48px throughout").

### `tokens.surfaces`

Border radii scale (from `none` through `full`), border widths, default border
style, and divider treatment.

### `tokens.depth`

Shadow definitions by elevation level. Each level has the CSS `box-shadow` value
and a note about when to use it. Also captures the overall depth philosophy:
flat, subtle layers, dramatic elevation, or something else.

### `tokens.motion`

Timing (`fast`, `default`, `slow`, `dramatic`), easing curves (`default`, `enter`,
`exit`, `bounce`), and the motion philosophy: "minimal functional", "smooth and
deliberate", "playful elastic", "cinematic", etc.

### `tokens.layout`

Grid system (columns, gutter, max-width), breakpoints, and alignment tendency
(`strict-grid`, `centered`, `asymmetric`, `fluid`).

### `tokens.icons`

Icon style (`outline`, `solid`, `duotone`, `custom`), stroke weight, size scale,
and preferred icon set if identifiable (e.g. Lucide, Heroicons, Phosphor).

### `tokens.components`

Observed patterns for common UI elements. Not full component specs — just enough
to capture the design language:

- `button`: shape, weight, fill strategy, size variants
- `input`: border treatment, focus state, label positioning
- `card`: surface treatment, shadow, border, padding pattern
- `nav`: layout pattern, indicator style, collapse behavior
- `modal`: overlay treatment, entry animation, sizing
- `badge`: shape, color strategy, sizing
- `table`: border style, row treatment, header weight

Include only components that appear in the source material.

---

## `character` — The Feeling Layer

These fields don't produce CSS directly. They guide subjective decisions when the
skin is applied: which way to go when there's a choice.

### `character.personality`

How the design would feel if it were a person:
- `mood`: 3-5 adjectives (e.g. `["calm", "precise", "trustworthy"]`)
- `archetype`: genre/style family (e.g. "corporate SaaS", "indie creative",
  "editorial luxury", "neo-brutalist", "playful consumer")
- `era`: design era influence ("swiss modernist", "y2k", "contemporary minimal")
- `metaphor`: central visual metaphor if any ("architecture", "nature", "machinery")
- `traits`: personality traits as if describing a person

### `character.visual_language`

How elements are presented:
- `complexity`: minimal → moderate → rich → maximal
- `ornamentation`: none → accent → decorative → heavy
- `whitespace`: generous → balanced → compressed
- `weight_distribution`: where visual mass concentrates (top, center, evenly spread)
- `focal_strategy`: single hero, distributed interest, progressive reveal
- `contrast_level`: high-contrast bold, medium, subtle low-contrast
- `texture`: clean/flat, grain/noise, photographic, illustrated

### `character.composition`

How layouts are arranged:
- `hierarchy_method`: scale contrast, color weight, spatial isolation, type hierarchy
- `balance`: symmetric, asymmetric, radial, mosaic
- `flow`: top-down, left-right, diagonal, Z-pattern, F-pattern
- `grouping`: cards, sections, overlapping layers, list-driven
- `negative_space`: structural (defining), decorative (breathing), minimal (dense)

### `character.imagery`

How images and graphics are treated:
- `photos`: warm/cool filter, high/low saturation, duotone, black-and-white, none
- `illustrations`: flat, 3D, hand-drawn, isometric, none
- `decorative_elements`: geometric shapes, organic blobs, lines, none
- `patterns`: geometric, organic, noise, none
- `image_shapes`: rectangular, rounded, circular, masked/clipped, full-bleed

### `character.interaction_feel`

How the UI responds to user action:
- `feedback`: instant/crisp, smooth/cushioned, bouncy/elastic, dramatic/theatrical
- `hover_behavior`: color shift, lift/shadow, scale, underline, glow, none
- `transitions`: snappy, gliding, springy, cinematic
- `loading`: skeleton, spinner, progress bar, shimmer, content fade-in
- `micro_density`: minimal (only essentials), moderate, rich (everything animates)

### `character.voice`

How the UI communicates through text and tone:
- `tone`: formal, professional, casual, playful, technical
- `cta_style`: direct imperative, friendly invitation, urgent, subtle
- `empty_states`: helpful, witty, minimal, illustrated
- `errors`: technical, friendly, apologetic, matter-of-fact

---

## `spectacle` — The Advanced Rendering Layer

Effects that go beyond standard CSS. Each entry has an `active` flag — set `false`
if the effect category isn't present.

### `spectacle.overview`

- `intensity`: none, subtle, moderate, immersive
- `tech_ceiling`: CSS-only, canvas-2d, webgl, gpu-heavy
- `degradation`: what happens on weak hardware or `prefers-reduced-motion`
  (disable, simplify to CSS, static fallback)

### `spectacle.backgrounds`

Animated or generative backgrounds:
- `active`, `kind` (gradient-animation, noise-field, mesh-gradient, video,
  generative-art, particle-field), `tech`, `params` (colors, speed, density,
  blend mode, opacity)

### `spectacle.particles`

Floating particles, confetti, connected nodes, etc:
- `active`, `kind`, `tech`, `params` (count, shape, size range, movement,
  color behavior, mouse interaction, spawn area)

### `spectacle.three_d`

3D objects, scenes, product viewers:
- `active`, `kind` (hero model, product viewer, abstract geometry, scene background,
  text extrusion), `tech`, `params` (renderer, lighting, camera, materials,
  geometry, post-processing, interaction)

### `spectacle.shaders`

Custom GLSL or shader-like effects:
- `active`, `kind` (noise distortion, wave, morph, color shift, custom),
  `tech`, `params` (uniforms, vertex manipulation, fragment output, noise type,
  distortion intensity)

### `spectacle.scroll`

Scroll-driven effects:
- `parallax`: active, layer count, depth range, speed curve
- `triggered`: active, trigger points, animation type, scrub or play-once
- `morphing`: active, description of what transforms on scroll

### `spectacle.text`

Text animation effects:
- `active`, `kind` (split-animate, typewriter, glitch, gradient-fill, 3d-extrude),
  `params` (split strategy, stagger timing, effect style)

### `spectacle.cursor`

Custom cursor and pointer effects:
- `active`, `kind` (custom shape, magnetic buttons, spotlight, trail, blob),
  `params` (shape, size, blend mode, trail length, interaction zone)

### `spectacle.images`

Image hover/reveal effects:
- `active`, `kind` (hover distortion, reveal clip, parallax tilt, RGB shift,
  liquid), `tech`, `params`

### `spectacle.glass`

Glassmorphism, neumorphism, frosted effects:
- `active`, `style` (glass, frosted-layers, neumorphic-light, neumorphic-dark),
  `params` (blur radius, transparency, border treatment, shadow type, light angle)

### `spectacle.canvas`

Custom canvas drawings (generative art, data viz, pattern fill):
- `active`, `kind`, `tech`, `params` (draw method, animation loop, colors,
  responsiveness, interaction)

### `spectacle.svg`

SVG-specific animations:
- `active`, `kind` (path draw, shape morph, logo reveal, decorative loop),
  `params` (animation method, path morphing, stroke animation, filter effects)

### `spectacle.notes`

Free text for anything that doesn't fit the structured fields: layered effect
combinations, performance observations, ambiguous effects seen only in screenshots,
implementation notes.

---

## Extraction Priority

When multiple data sources are available, trust them in this order:

1. **Figma variables** — designer-defined tokens, highest authority
2. **Figma styles** — text styles, color styles, effect styles
3. **Figma component properties** — values measured from actual components
4. **Code** (CSS vars, Tailwind config, theme files) — if available
5. **Visual analysis** of screenshots/images — lowest precision, last resort

## Dark Mode Derivation

When only one mode exists, derive the other:
- Backgrounds: invert lightness (50↔900, 100↔800)
- Text: invert (dark text → light text, preserve hierarchy gaps)
- Brand colors: shift 2 stops lighter on dark, preserve hue
- Borders: maintain opacity patterns, flip contrast direction
- Shadows: increase opacity on dark (shadows less visible)
- Preserve the character — dark mode should feel like the same design, not a recolor

## Open Extension

This schema is not exhaustive. If the source design has a dimension not covered here
(e.g. sound design, haptic patterns, accessibility-specific tokens, responsive
behavior rules, localization variants), add it as a new top-level section under the
appropriate layer (`tokens`, `character`, or `spectacle`) and document what it captures.
