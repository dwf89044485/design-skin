# Tailwind CSS Output Template

Generate these files when the user selects Tailwind output.

## tailwind.skin.config.js

```js
// =============================================================================
// Design Skin: {{SKIN_NAME}}
// Generated from: {{SOURCE}}
// Date: {{DATE}}
//
// Usage: Import and merge with your existing tailwind.config.js
//   import skinConfig from './tailwind.skin.config.js'
//   export default { ...yourConfig, theme: { extend: { ...yourConfig.theme?.extend, ...skinConfig.theme.extend } } }
// =============================================================================

/** @type {import('tailwindcss').Config} */
export default {
  theme: {
    extend: {
      // — Colors —
      colors: {
        // Primitive palette
        // {{PRIMITIVE_COLORS}}

        // Semantic tokens (reference CSS custom properties for dark mode support)
        skin: {
          primary: 'var(--skin-primary)',
          'primary-hover': 'var(--skin-primary-hover)',
          secondary: 'var(--skin-secondary)',
          background: 'var(--skin-background)',
          surface: 'var(--skin-surface)',
          'surface-hover': 'var(--skin-surface-hover)',
          muted: 'var(--skin-muted)',
          'text-primary': 'var(--skin-text-primary)',
          'text-secondary': 'var(--skin-text-secondary)',
          'text-tertiary': 'var(--skin-text-tertiary)',
          border: 'var(--skin-border)',
          'border-strong': 'var(--skin-border-strong)',
          ring: 'var(--skin-ring)',
          success: 'var(--skin-success)',
          warning: 'var(--skin-warning)',
          error: 'var(--skin-error)',
          info: 'var(--skin-info)',
        },
      },

      // — Typography —
      fontFamily: {
        // {{FONT_FAMILIES}}
      },

      fontSize: {
        // {{FONT_SIZES}}
        // Format: 'key': ['size', { lineHeight: 'lh', letterSpacing: 'ls', fontWeight: 'wt' }]
      },

      // — Spacing —
      spacing: {
        // {{SPACING_SCALE}}
      },

      // — Borders —
      borderRadius: {
        // {{BORDER_RADII}}
      },

      borderWidth: {
        // {{BORDER_WIDTHS}}
      },

      // — Shadows —
      boxShadow: {
        // {{SHADOWS}}
      },

      // — Motion —
      transitionDuration: {
        // {{DURATIONS}}
      },

      transitionTimingFunction: {
        // {{EASINGS}}
      },

      // — Effects —
      backdropBlur: {
        // {{BACKDROP_BLURS}}
      },
    },
  },
}
```

## skin.css

```css
/* =============================================================================
   Design Skin: {{SKIN_NAME}}
   CSS Custom Properties Layer

   Import this file in your main CSS or add to your Tailwind @layer base.
   Dark mode activates automatically via prefers-color-scheme, or manually
   by adding class="dark" to the <html> element.
   ============================================================================= */

/* Google Fonts import (if applicable) */
/* {{GOOGLE_FONTS_IMPORT}} */

@layer base {
  :root {
    /* — Color Primitives (for reference, not direct use) — */
    /* {{PRIMITIVE_CSS_VARS}} */

    /* — Semantic Colors (Light Mode) — */
    --skin-primary: {{PRIMARY_LIGHT}};
    --skin-primary-hover: {{PRIMARY_HOVER_LIGHT}};
    --skin-secondary: {{SECONDARY_LIGHT}};
    --skin-background: {{BG_LIGHT}};
    --skin-surface: {{SURFACE_LIGHT}};
    --skin-surface-hover: {{SURFACE_HOVER_LIGHT}};
    --skin-muted: {{MUTED_LIGHT}};
    --skin-text-primary: {{TEXT_PRIMARY_LIGHT}};
    --skin-text-secondary: {{TEXT_SECONDARY_LIGHT}};
    --skin-text-tertiary: {{TEXT_TERTIARY_LIGHT}};
    --skin-text-inverse: {{TEXT_INVERSE_LIGHT}};
    --skin-border: {{BORDER_LIGHT}};
    --skin-border-strong: {{BORDER_STRONG_LIGHT}};
    --skin-border-subtle: {{BORDER_SUBTLE_LIGHT}};
    --skin-ring: {{RING_LIGHT}};
    --skin-success: {{SUCCESS_LIGHT}};
    --skin-warning: {{WARNING_LIGHT}};
    --skin-error: {{ERROR_LIGHT}};
    --skin-info: {{INFO_LIGHT}};

    /* — Typography — */
    --skin-font-display: {{FONT_DISPLAY}};
    --skin-font-body: {{FONT_BODY}};
    --skin-font-mono: {{FONT_MONO}};

    /* — Spacing — */
    --skin-space-unit: {{BASE_UNIT}}px;

    /* — Borders — */
    --skin-radius-sm: {{RADIUS_SM}};
    --skin-radius-default: {{RADIUS_DEFAULT}};
    --skin-radius-md: {{RADIUS_MD}};
    --skin-radius-lg: {{RADIUS_LG}};
    --skin-radius-xl: {{RADIUS_XL}};
    --skin-radius-full: 9999px;

    /* — Shadows — */
    --skin-shadow-sm: {{SHADOW_SM}};
    --skin-shadow-default: {{SHADOW_DEFAULT}};
    --skin-shadow-md: {{SHADOW_MD}};
    --skin-shadow-lg: {{SHADOW_LG}};
    --skin-shadow-xl: {{SHADOW_XL}};

    /* — Motion — */
    --skin-duration-fast: {{DURATION_FAST}};
    --skin-duration-default: {{DURATION_DEFAULT}};
    --skin-duration-slow: {{DURATION_SLOW}};
    --skin-ease-default: {{EASE_DEFAULT}};
    --skin-ease-in: {{EASE_IN}};
    --skin-ease-out: {{EASE_OUT}};
  }

  /* — Dark Mode — */
  .dark,
  [data-theme="dark"] {
    --skin-primary: {{PRIMARY_DARK}};
    --skin-primary-hover: {{PRIMARY_HOVER_DARK}};
    --skin-secondary: {{SECONDARY_DARK}};
    --skin-background: {{BG_DARK}};
    --skin-surface: {{SURFACE_DARK}};
    --skin-surface-hover: {{SURFACE_HOVER_DARK}};
    --skin-muted: {{MUTED_DARK}};
    --skin-text-primary: {{TEXT_PRIMARY_DARK}};
    --skin-text-secondary: {{TEXT_SECONDARY_DARK}};
    --skin-text-tertiary: {{TEXT_TERTIARY_DARK}};
    --skin-text-inverse: {{TEXT_INVERSE_DARK}};
    --skin-border: {{BORDER_DARK}};
    --skin-border-strong: {{BORDER_STRONG_DARK}};
    --skin-border-subtle: {{BORDER_SUBTLE_DARK}};
    --skin-ring: {{RING_DARK}};
    --skin-success: {{SUCCESS_DARK}};
    --skin-warning: {{WARNING_DARK}};
    --skin-error: {{ERROR_DARK}};
    --skin-info: {{INFO_DARK}};

    --skin-shadow-sm: {{SHADOW_SM_DARK}};
    --skin-shadow-default: {{SHADOW_DEFAULT_DARK}};
    --skin-shadow-md: {{SHADOW_MD_DARK}};
    --skin-shadow-lg: {{SHADOW_LG_DARK}};
    --skin-shadow-xl: {{SHADOW_XL_DARK}};
  }

  @media (prefers-color-scheme: dark) {
    :root:not(.light):not([data-theme="light"]) {
      /* Same dark mode values — auto-activates unless explicitly light */
      --skin-primary: {{PRIMARY_DARK}};
      /* ... (same as .dark block above) */
    }
  }
}
```

## skin-preview.html

Generate a single-file HTML page that:
1. Imports the skin.css file (inline the CSS if it's a standalone preview)
2. Includes a dark/light mode toggle button
3. Shows these sections:
   - **Color Palette**: Grid of color swatches with name + hex value
   - **Typography**: Each type scale level rendered as a sample sentence
   - **Spacing**: Visual boxes showing the spacing scale
   - **Components**: Button (primary, secondary, outline), Card, Input, Badge, Tag
   - **Shadows**: Boxes at each elevation level
4. Uses the skin's own fonts, colors, and spacing — it should feel like the design

The preview page itself should be clean and functional — it's a tool, not a showcase.
Use a simple layout with clear section labels.

## Generation Rules

1. **Always use CSS custom properties for semantic tokens** — this enables dark mode toggling without JavaScript in most cases.
2. **Tailwind config colors reference CSS vars** — so `bg-skin-primary` automatically respects dark mode.
3. **Include comments** — explain where values came from (e.g., `/* Figma: color/bg/primary */` or `/* Extracted from header background */`).
4. **Use rem for type, px for spacing/shadow** — unless the project has a different convention.
5. **Group logically** — primitives first, then semantics, then typography, then spacing, etc.
