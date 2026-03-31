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

Design Skin JSON (structured intermediate format):
  ├── colors (palette, semantic mappings, gradients)
  ├── typography (font stacks, scale, weights)
  ├── spacing (scale, rhythm)
  ├── borders & radii
  ├── shadows & elevation
  ├── effects (blur, opacity patterns)
  ├── motion (timing, easing preferences)
  └── style notes (qualitative feel, mood, personality)

        ↓ Generate

Output (user's choice):
  ├── Tailwind CSS config (tailwind.config.js + CSS layer)
  ├── CSS Custom Properties (variables.css + utility classes)
  ├── React Theme (theme.ts + ThemeProvider + hooks)
  └── Combined package (all of the above)
```

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
2. Analyze the visual design systematically — don't just glance, really study it:
   - **Colors**: Identify the full palette. Look for primary, secondary, accent colors.
     Note background colors, text colors, border colors, hover/active states.
     Pay attention to gradients, overlays, and transparency patterns.
   - **Typography**: Identify font families (or closest match if not exact).
     Note the type scale — how many distinct sizes are used and their relative hierarchy.
     Look at font weights, letter-spacing, line-height patterns.
   - **Spacing**: Measure the rhythm. Is it a 4px grid? 8px? Note padding patterns
     in cards, buttons, sections. Look at gap patterns in layouts.
   - **Borders & Radii**: Sharp corners or rounded? How rounded? Consistent or varied?
     Border widths and colors.
   - **Shadows & Depth**: Flat design or layered? Shadow intensity, blur, spread patterns.
     How many elevation levels?
   - **Visual Effects**: Blur, grain, noise, texture overlays, glassmorphism,
     gradient meshes, or other distinctive effects.
   - **Overall Feel**: Is this minimal or dense? Warm or cool? Playful or serious?
     What makes it distinctive?

3. If multiple images show the same design system from different angles, synthesize them
   into one coherent understanding — don't treat each image independently.

### From Figma Files

When the user provides a Figma URL:
1. Extract fileKey and nodeId from the URL
2. Fetch structured data using the Figma MCP tools in this order:

   **a. Get visual overview first:**
   ```
   get_screenshot(fileKey, nodeId)  → see what we're working with
   get_metadata(fileKey, nodeId)    → understand structure
   ```

   **b. Extract variables/tokens (the gold mine):**
   ```
   get_variable_defs(fileKey, nodeId) → design tokens already defined
   ```
   Variables give you the most reliable extraction — they're the designer's own token
   definitions. Map them directly:
   - COLOR variables → color tokens
   - FLOAT variables → spacing, radius, font-size tokens
   - STRING variables → font-family tokens

   **c. Get design context for key components:**
   ```
   get_design_context(fileKey, nodeId) → detailed component specs
   ```
   This returns typography specs, color values, spacing, and component structure.
   Use it on representative components (buttons, cards, inputs, headers) to understand
   the design language.

   **d. Search for design system assets:**
   ```
   search_design_system(query="button", fileKey)  → find components
   search_design_system(query="color", fileKey)    → find color styles
   ```

3. Prioritize data sources in this order:
   - Figma variables (most authoritative — designer defined these as tokens)
   - Figma styles (text styles, color styles, effect styles)
   - Component property values (colors, sizes extracted from actual components)
   - Visual measurement from screenshots (last resort)

### From Verbal Description

When the user describes a style verbally ("I want something like Apple's design" or
"dark mode, neon accents, glassmorphism"), use this as a style direction guide that
informs your extraction and generation choices.

## Phase 2: Build the Design Skin JSON

This is the intermediate structured format. Always build this before generating output files.
Read `references/skin-schema.md` for the complete schema specification.

The JSON has these top-level sections:

```json
{
  "meta": {
    "name": "skin-name",
    "description": "Brief description of the design aesthetic",
    "source": "figma | images | hybrid | verbal",
    "generated": "2024-01-15T10:30:00Z"
  },
  "colors": { },
  "typography": { },
  "spacing": { },
  "borders": { },
  "shadows": { },
  "effects": { },
  "motion": { },
  "styleNotes": { }
}
```

**Present the Design Skin JSON to the user for review before generating output files.**
Walk through each section briefly and ask if anything looks off. This is the checkpoint
where corrections are cheapest.

## Phase 3: Generate Output Files

Ask the user which output format they want (or generate all if they're unsure):

### Option A: Tailwind CSS Config

Read `references/tailwind-output.md` for the detailed template.

Generate:
- `tailwind.skin.config.js` — extends the user's existing Tailwind config
- `skin.css` — CSS layer with custom properties and utility classes
- `skin-preview.html` — standalone preview page showing all tokens in action

The Tailwind config should use `extend` so it doesn't clobber existing settings:
```js
// tailwind.skin.config.js — merge into your tailwind.config.js
export default {
  theme: {
    extend: {
      colors: { /* extracted palette */ },
      fontFamily: { /* extracted fonts */ },
      // ...
    }
  }
}
```

### Option B: CSS Custom Properties

Read `references/css-output.md` for the detailed template.

Generate:
- `skin-variables.css` — all tokens as CSS custom properties
- `skin-utilities.css` — utility classes built on the variables
- `skin-preview.html` — standalone preview page

### Option C: React Theme

Read `references/react-output.md` for the detailed template.

Generate:
- `skin-theme.ts` — typed theme object with all tokens
- `SkinProvider.tsx` — React context provider with dark/light mode support
- `useSkin.ts` — hook for consuming the theme
- `skin.css` — CSS custom properties synced with the theme
- `SkinPreview.tsx` — component showing all tokens

### Option D: Combined Package

Generate all of the above in a structured directory:
```
design-skin-output/
├── skin.json             ← the Design Skin JSON
├── tailwind/
│   ├── tailwind.skin.config.js
│   └── skin.css
├── css/
│   ├── skin-variables.css
│   └── skin-utilities.css
├── react/
│   ├── skin-theme.ts
│   ├── SkinProvider.tsx
│   ├── useSkin.ts
│   └── skin.css
└── preview.html          ← universal preview page
```

## Phase 4: Preview & Validate

Always generate a preview that shows the skin in action. The preview **must be a single
self-contained HTML file** with all CSS inlined (no external file references) so it works
when opened directly in a browser.

The preview page should include:

1. **Color Palette** — all colors rendered as swatches with labels and hex values.
   Show both primitive palette (full shade scale) and semantic tokens.
2. **Typography Scale** — each type scale level rendered as a sample sentence using
   the actual imported fonts. Include font name, size, weight, and line-height labels.
3. **Spacing Visualization** — boxes at each spacing scale step with pixel values.
4. **Component Samples** — at minimum: primary button, secondary/outline button, card
   with title/body/footer, text input with label, badge/tag, and alert/toast. Style
   every component using only the skin's CSS custom properties.
5. **Dark/Light Toggle** — a toggle button in the top-right corner that switches between
   modes by toggling the `dark` class on `<html>`. Both modes should look complete and
   polished — dark mode is not an afterthought.

The preview serves two purposes:
- The user can immediately see if the extraction is accurate
- It demonstrates that the skin actually works as functional code

Open the preview in the browser (`open preview.html`) and ask the user to review it.

## Phase 5: Iterate

If the user wants changes:
- Adjust the Design Skin JSON
- Regenerate only the affected output files
- Refresh the preview

Common adjustments:
- "Make the colors warmer/cooler" → shift hue of the palette
- "The spacing feels too tight/loose" → scale the spacing multiplier
- "Add a dark mode" → generate mode-aware variables with inverted semantics
- "The fonts don't match" → swap font families, adjust scale

## File Naming Convention

Use these exact file names for consistency across all skins. This matters because
downstream tools and vibe-coding sessions will look for these specific names.

| Output Format | Files |
|---|---|
| Tailwind | `skin.json`, `tailwind.skin.config.js`, `skin.css`, `preview.html` |
| CSS | `skin.json`, `skin-variables.css`, `skin-utilities.css`, `preview.html` |
| React | `skin.json`, `skin-theme.ts`, `SkinProvider.tsx`, `useSkin.ts`, `skin.css`, `preview.html` |
| Combined | All above, organized in subdirectories (see Option D) |

The `skin.json` is always generated regardless of output format — it's the canonical
representation of the extracted design and enables re-generating different output formats
later without re-extracting.

## Important Principles

**Accuracy over creativity.** When extracting from a reference, the goal is to faithfully
capture what's there — not to "improve" it. The user chose that design for a reason.
Creative liberties belong in the generation phase, not the extraction phase.

**Tokens, not hardcoded values.** Every value in the output should trace back to a named
token. This makes the skin maintainable and adjustable. A button's background isn't
`#3B82F6`, it's `var(--skin-color-primary)`.

**Extend, don't replace.** Generated configs should layer on top of existing project
settings. Use Tailwind's `extend`, CSS layers with low specificity, and React context
that falls through to defaults.

**Dark mode is not optional.** If the source design doesn't include a dark variant,
generate a reasonable one automatically. Use the extracted palette to derive dark-mode
equivalents (invert lightness, preserve hue/saturation).

**Fonts need fallbacks.** Always include a fallback stack. If the exact font from the
design isn't available as a web font, suggest the closest Google Fonts alternative and
note the substitution.

**The preview is the proof.** Never ship extracted tokens without a working preview.
If the preview looks wrong, the tokens are wrong — go back and fix them.
