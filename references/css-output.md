# CSS Custom Properties Output Template

Generate these files when the user selects CSS output.

## skin-variables.css

```css
/* =============================================================================
   Design Skin: {{SKIN_NAME}}
   Pure CSS Custom Properties — framework-agnostic

   Usage: @import './skin-variables.css';
   Dark mode: add class="dark" or data-theme="dark" to <html>
   Auto dark mode: respects prefers-color-scheme by default
   ============================================================================= */

/* Google Fonts */
/* {{GOOGLE_FONTS_IMPORT}} */

/* === Color Primitives === */
:root {
  /* These are raw palette values — use semantic tokens in your code instead */
  /* {{PRIMITIVE_COLOR_VARS}} */
}

/* === Semantic Tokens (Light Mode Default) === */
:root {
  /* Brand */
  --skin-primary: ;
  --skin-primary-hover: ;
  --skin-primary-active: ;
  --skin-secondary: ;
  --skin-secondary-hover: ;

  /* Surfaces */
  --skin-background: ;
  --skin-surface: ;
  --skin-surface-hover: ;
  --skin-surface-active: ;
  --skin-muted: ;

  /* Text */
  --skin-text-primary: ;
  --skin-text-secondary: ;
  --skin-text-tertiary: ;
  --skin-text-inverse: ;
  --skin-text-link: ;
  --skin-text-link-hover: ;

  /* Borders */
  --skin-border: ;
  --skin-border-strong: ;
  --skin-border-subtle: ;
  --skin-ring: ;
  --skin-ring-offset: ;

  /* Status */
  --skin-success: ;
  --skin-success-bg: ;
  --skin-warning: ;
  --skin-warning-bg: ;
  --skin-error: ;
  --skin-error-bg: ;
  --skin-info: ;
  --skin-info-bg: ;

  /* Typography */
  --skin-font-display: ;
  --skin-font-body: ;
  --skin-font-mono: ;

  /* Type Scale */
  --skin-text-display-2xl: ;
  --skin-text-display-xl: ;
  --skin-text-display-lg: ;
  --skin-text-h1: ;
  --skin-text-h2: ;
  --skin-text-h3: ;
  --skin-text-h4: ;
  --skin-text-body-lg: ;
  --skin-text-body: ;
  --skin-text-body-sm: ;
  --skin-text-caption: ;
  --skin-text-overline: ;

  /* Spacing */
  --skin-space-unit: ;
  --skin-space-xs: ;
  --skin-space-sm: ;
  --skin-space-md: ;
  --skin-space-lg: ;
  --skin-space-xl: ;
  --skin-space-2xl: ;
  --skin-space-3xl: ;

  /* Borders */
  --skin-radius-sm: ;
  --skin-radius-default: ;
  --skin-radius-md: ;
  --skin-radius-lg: ;
  --skin-radius-xl: ;
  --skin-radius-2xl: ;
  --skin-radius-full: 9999px;
  --skin-border-width: ;
  --skin-border-width-thick: ;

  /* Shadows */
  --skin-shadow-xs: ;
  --skin-shadow-sm: ;
  --skin-shadow-default: ;
  --skin-shadow-md: ;
  --skin-shadow-lg: ;
  --skin-shadow-xl: ;
  --skin-shadow-inner: ;

  /* Motion */
  --skin-duration-fast: ;
  --skin-duration-default: ;
  --skin-duration-slow: ;
  --skin-duration-slower: ;
  --skin-ease-default: ;
  --skin-ease-in: ;
  --skin-ease-out: ;
  --skin-ease-bounce: ;

  /* Effects */
  --skin-blur-sm: ;
  --skin-blur-default: ;
  --skin-blur-lg: ;
  --skin-overlay-light: ;
  --skin-overlay-dark: ;
}

/* === Dark Mode === */
.dark,
[data-theme="dark"] {
  --skin-primary: ;
  --skin-primary-hover: ;
  /* ... all semantic tokens with dark values ... */
}

@media (prefers-color-scheme: dark) {
  :root:not(.light):not([data-theme="light"]) {
    --skin-primary: ;
    --skin-primary-hover: ;
    /* ... same dark values ... */
  }
}
```

## skin-utilities.css

```css
/* =============================================================================
   Design Skin: {{SKIN_NAME}}
   Utility Classes — optional convenience layer

   These classes wrap the CSS custom properties for quick use.
   Skip this file if you're using Tailwind (use tailwind.skin.config.js instead).
   ============================================================================= */

/* === Typography Utilities === */
.skin-font-display { font-family: var(--skin-font-display); }
.skin-font-body { font-family: var(--skin-font-body); }
.skin-font-mono { font-family: var(--skin-font-mono); }

.skin-text-display-2xl {
  font-size: var(--skin-text-display-2xl);
  font-family: var(--skin-font-display);
  line-height: 1.1;
  letter-spacing: -0.025em;
  font-weight: 800;
}

/* ... one class per type scale level ... */

.skin-text-body {
  font-size: var(--skin-text-body);
  font-family: var(--skin-font-body);
  line-height: 1.6;
  font-weight: 400;
}

/* === Color Utilities === */
.skin-bg-primary { background-color: var(--skin-primary); }
.skin-bg-surface { background-color: var(--skin-surface); }
.skin-bg-background { background-color: var(--skin-background); }
.skin-bg-muted { background-color: var(--skin-muted); }

.skin-text-primary { color: var(--skin-text-primary); }
.skin-text-secondary { color: var(--skin-text-secondary); }
.skin-text-tertiary { color: var(--skin-text-tertiary); }
.skin-text-inverse { color: var(--skin-text-inverse); }

.skin-border { border-color: var(--skin-border); }
.skin-border-strong { border-color: var(--skin-border-strong); }

/* === Shadow Utilities === */
.skin-shadow-sm { box-shadow: var(--skin-shadow-sm); }
.skin-shadow { box-shadow: var(--skin-shadow-default); }
.skin-shadow-md { box-shadow: var(--skin-shadow-md); }
.skin-shadow-lg { box-shadow: var(--skin-shadow-lg); }
.skin-shadow-xl { box-shadow: var(--skin-shadow-xl); }

/* === Radius Utilities === */
.skin-rounded-sm { border-radius: var(--skin-radius-sm); }
.skin-rounded { border-radius: var(--skin-radius-default); }
.skin-rounded-md { border-radius: var(--skin-radius-md); }
.skin-rounded-lg { border-radius: var(--skin-radius-lg); }
.skin-rounded-xl { border-radius: var(--skin-radius-xl); }
.skin-rounded-full { border-radius: var(--skin-radius-full); }

/* === Transition Utilities === */
.skin-transition {
  transition-property: color, background-color, border-color, box-shadow, transform, opacity;
  transition-duration: var(--skin-duration-default);
  transition-timing-function: var(--skin-ease-default);
}

.skin-transition-fast {
  transition-property: color, background-color, border-color, box-shadow, transform, opacity;
  transition-duration: var(--skin-duration-fast);
  transition-timing-function: var(--skin-ease-default);
}

/* === Component Patterns (optional — common patterns built from tokens) === */
.skin-card {
  background-color: var(--skin-surface);
  border: var(--skin-border-width) solid var(--skin-border);
  border-radius: var(--skin-radius-lg);
  padding: var(--skin-space-lg);
  box-shadow: var(--skin-shadow-sm);
}

.skin-card:hover {
  background-color: var(--skin-surface-hover);
  box-shadow: var(--skin-shadow-default);
}

.skin-btn-primary {
  background-color: var(--skin-primary);
  color: var(--skin-text-inverse);
  font-family: var(--skin-font-body);
  font-weight: 500;
  padding: var(--skin-space-sm) var(--skin-space-lg);
  border-radius: var(--skin-radius-default);
  border: none;
  cursor: pointer;
  transition: background-color var(--skin-duration-fast) var(--skin-ease-default);
}

.skin-btn-primary:hover {
  background-color: var(--skin-primary-hover);
}

.skin-input {
  background-color: var(--skin-surface);
  color: var(--skin-text-primary);
  font-family: var(--skin-font-body);
  padding: var(--skin-space-sm) var(--skin-space-md);
  border: var(--skin-border-width) solid var(--skin-border);
  border-radius: var(--skin-radius-default);
  outline: none;
  transition: border-color var(--skin-duration-fast) var(--skin-ease-default),
              box-shadow var(--skin-duration-fast) var(--skin-ease-default);
}

.skin-input:focus {
  border-color: var(--skin-ring);
  box-shadow: 0 0 0 3px var(--skin-ring) / 0.15;
}

.skin-badge {
  display: inline-flex;
  align-items: center;
  font-family: var(--skin-font-body);
  font-size: var(--skin-text-caption);
  font-weight: 500;
  padding: var(--skin-space-xs) var(--skin-space-sm);
  border-radius: var(--skin-radius-full);
  background-color: var(--skin-muted);
  color: var(--skin-text-secondary);
}
```

## Generation Rules

1. **Every value references a CSS custom property** — no hardcoded values in utility classes.
2. **Comment origin** — add comments showing where each value came from.
3. **Include all tokens from the Design Skin JSON** — don't skip sections.
4. **Dark mode uses the same variable names** — only the values change.
5. **Utilities are optional** — the variables file is the core deliverable.
