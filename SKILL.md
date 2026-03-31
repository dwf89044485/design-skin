---
name: design-skin
description: >
  Extract visual style DNA from UI/UX images, screenshots, or Figma files and generate
  production-ready theme/skin packages for React, Tailwind CSS, or plain HTML/CSS projects.
  Use this skill whenever the user wants to: extract a design style from reference images or
  screenshots, pull design tokens from a Figma file, create a reusable theme or skin from
  existing designs, generate a Tailwind config or CSS variables from a visual reference,
  capture the "vibe" or aesthetic of a design for reuse, build a style system from Figma
  components/variables, or set up theming for vibe-coding sessions. Triggers on phrases like
  "extract style", "design skin", "theme from figma", "style from screenshots", "create skin",
  "design tokens from", "generate theme", "capture design style", "reusable theme",
  "skin package", "style extraction", "design DNA", or when the user provides UI images or
  Figma URLs and asks to make the style reusable.
---

# Design Skin — Extract Styles, Generate Themes

Transform visual references into production-ready theme packages. Accept any combination of
UI/UX screenshots, design mockups, or Figma files and produce a structured, reusable skin
that can be dropped into React, Tailwind, or HTML/CSS projects.

## How It Works

```
Input (any combination):
  ├── UI/UX images / screenshots
  ├── Figma URL (design pages, components, variables)
  └── Verbal style description from user

        ↓ Extract & Analyze

Skin Profile (structured intermediate — skin.json):
  ├── tokens     — hard values (palette, type, space, depth, motion, layout, components...)
  ├── character  — soft qualities (mood, composition, visual language, voice...)
  └── spectacle  — advanced effects (3D, particles, shaders, scroll, cursor...)

        ↓ Generate

Output (user's choice):
  ├── Tailwind CSS config + CSS layer
  ├── CSS Custom Properties + utility classes
  ├── React Theme (typed) + Provider + hooks
  └── Combined package (all of the above)
```

The profile has three layers — see `references/skin-schema.md` for the full spec:

- **tokens**: Everything with a concrete CSS equivalent. Colors, typography, spacing,
  surfaces, depth, motion, layout, icons, component patterns. These become your CSS
  variables and config values.
- **character**: The feeling. Mood, personality, visual language, composition style,
  interaction feel, brand voice. These guide subjective decisions when applying the skin.
- **spectacle**: Advanced rendering beyond flat CSS. 3D, particles, shaders, scroll
  effects, cursor effects, glassmorphism, canvas drawings, SVG animations. Each has an
  `active` flag — only include what's actually present.

The schema is open-ended — if the source design has qualities not covered by the standard
fields, add them under the appropriate layer.

## Figma MCP Dependency

Figma extraction requires Figma MCP tools (`get_screenshot`, `get_design_context`,
`get_variable_defs`, etc.). If the user's task involves Figma and these tools are not
available in your environment:

1. Tell the user they need Figma MCP to proceed with Figma extraction
2. Fetch https://github.com/figma/mcp-server-guide for the latest setup instructions,
   identify the method that matches the user's environment and tools, and guide them
3. If the user asks you to help install, do it and walk them through OAuth authentication
4. Offer fallback: screenshots, exported JSON tokens, or verbal description all work
   without Figma MCP — just with less precision

## Phase 1: Gather Inputs

Identify what the user has provided and collect everything before extracting.

### From Images / Screenshots

When the user provides images (uploaded files, paths, or URLs):
1. Read each image using the Read tool
2. Analyze systematically across all three layers:
   - **tokens**: Extract every measurable property — colors, fonts, sizes, spacing,
     radii, shadows, grid structure, icon style, component patterns
   - **character**: Assess the feeling — what mood does this evoke? What's the density,
     complexity, ornamentation level? How is hierarchy communicated? What's the
     composition strategy?
   - **spectacle**: Identify anything beyond standard CSS — animated backgrounds,
     particles, 3D elements, scroll effects, custom cursors, glassmorphism, noise/grain
     textures. If you can see it but can't express it in a CSS property, it belongs here.
3. When multiple images show the same design system, synthesize into one coherent
   understanding — don't treat each image independently.

### From Figma Files

When the user provides a Figma URL:
1. Extract fileKey and nodeId from the URL
2. Fetch structured data using Figma MCP tools:
   - `get_screenshot` + `get_metadata` → visual overview and structure
   - `get_variable_defs` → design tokens (the most authoritative source)
   - `get_design_context` → component specs (typography, colors, spacing)
   - `search_design_system` → library components, variables, styles
3. Prioritize: Figma variables > styles > component values > visual measurement

### From Verbal Description

When the user describes a style verbally ("like Linear but warmer", "Apple meets
brutalism"), use this to guide all three layers of extraction and generation.

## Phase 2: Build the Skin Profile

Always build the profile before generating output files. Read `references/skin-schema.md`
for the complete field reference.

The profile JSON:

```json
{
  "meta": { "name": "...", "about": "...", "sources": [], "created": "..." },
  "tokens": {
    "palette": { },
    "roles": { },
    "type": { "stacks": {}, "scale": {}, "character": "..." },
    "space": { "grid": {}, "roles": {}, "density": "..." },
    "surfaces": { },
    "depth": { },
    "motion": { },
    "layout": { },
    "icons": { },
    "components": { }
  },
  "character": {
    "personality": { },
    "visual_language": { },
    "composition": { },
    "imagery": { },
    "interaction_feel": { },
    "voice": { }
  },
  "spectacle": {
    "overview": { },
    "backgrounds": { },
    "particles": { },
    "three_d": { },
    "shaders": { },
    "scroll": { },
    "text": { },
    "cursor": { },
    "images": { },
    "glass": { },
    "canvas": { },
    "svg": { },
    "notes": ""
  }
}
```

Only populate what's actually present — don't invent. The `spectacle` layer may be
entirely empty for a clean corporate design, and that's correct.

**Present the profile to the user for review before generating output files.** Walk
through each layer briefly and ask if anything looks off. This is the checkpoint where
corrections are cheapest.

## Phase 3: Generate Output Files

Ask the user which output format they want (or generate all if they're unsure):

### Option A: Tailwind CSS Config

Read `references/tailwind-output.md` for the detailed template. Generate:
- `tailwind.skin.config.js` — extends the user's existing Tailwind config with `theme.extend`
- `skin.css` — CSS layer with custom properties (light + dark) and base styles
- `preview.html` — standalone preview page

### Option B: CSS Custom Properties

Read `references/css-output.md` for the detailed template. Generate:
- `skin-variables.css` — all tokens as CSS custom properties with light/dark modes
- `skin-utilities.css` — utility classes built on the variables
- `preview.html` — standalone preview page

### Option C: React Theme

Read `references/react-output.md` for the detailed template. Generate:
- `skin-theme.ts` — typed theme object
- `SkinProvider.tsx` — context provider with dark/light mode support
- `useSkin.ts` — consumption hook
- `skin.css` — CSS custom properties synced with the theme
- `preview.html` — standalone preview page

### Option D: Combined Package

All of the above, organized:
```
design-skin-output/
├── skin.json
├── tailwind/
├── css/
├── react/
└── preview.html
```

### How `character` and `spectacle` map to output

The `tokens` layer maps directly to CSS variables and config values. The other two layers
inform the output differently:

**`character` → generation guidance**: The `character` layer is embedded as comments and
documentation in the output files. When the skin is used later for vibe-coding, these
comments tell the AI (or the developer) *how* to apply the tokens — what kind of hover
effects to favor, how much whitespace to use, whether to lean formal or playful.

**`spectacle` → implementation recipes**: For each active spectacle entry, generate a
commented code block or reference snippet showing how to implement that effect. Include
technology choice, CDN imports needed, and a degradation strategy. These go into the
preview.html as live demonstrations where feasible, and as documented recipes in a
`spectacle-recipes.js` file for effects too heavy to inline.

## Phase 4: Preview & Validate

Always generate a preview as a **single self-contained HTML file** with all CSS inlined.

The preview should include:
1. **Color Palette** — primitive swatches + semantic token grid (light and dark)
2. **Typography Scale** — every level rendered with actual imported fonts
3. **Spacing Visualization** — boxes at each scale step
4. **Component Samples** — buttons, cards, inputs, badges, alerts, nav elements
5. **Dark/Light Toggle** — in the top-right, toggling `.dark` on `<html>`
6. **Spectacle Demos** — live demonstrations of any active spectacle effects
   (or screenshots with "requires [technology]" labels for heavy effects)

Open the preview in the browser and ask the user to review it.

## Phase 5: Iterate

If the user wants changes:
- Adjust the skin profile
- Regenerate only the affected output files
- Refresh the preview

## File Naming Convention

| Output | Files |
|---|---|
| Tailwind | `skin.json`, `tailwind.skin.config.js`, `skin.css`, `preview.html` |
| CSS | `skin.json`, `skin-variables.css`, `skin-utilities.css`, `preview.html` |
| React | `skin.json`, `skin-theme.ts`, `SkinProvider.tsx`, `useSkin.ts`, `skin.css`, `preview.html` |
| Combined | All above in subdirectories |

`skin.json` is always generated — it's the canonical profile and enables re-generating
different output formats later without re-extracting.

## Principles

**Accuracy over creativity.** Extraction captures what's there, not what could be better.

**Tokens, not magic numbers.** Every value traces to a named token. `var(--skin-primary)`,
never `#3B82F6`.

**Extend, don't replace.** Tailwind `extend`, CSS layers, React context fallthrough.

**Dark mode always.** Derive it automatically if the source only has one mode.

**Fonts need fallbacks.** Suggest closest Google Fonts alternative if exact match unavailable.

**The preview is the proof.** If the preview looks wrong, the tokens are wrong.

**Open schema.** If the design has something not in the standard fields, add it. The
schema serves the design, not the other way around.
