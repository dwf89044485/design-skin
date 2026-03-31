# Design Skin JSON Schema

The Design Skin JSON is the canonical intermediate format. All output generators read from this structure.

## Complete Schema

```json
{
  "meta": {
    "name": "string — kebab-case identifier (e.g., 'aurora-dark', 'stripe-clean')",
    "description": "string — 1-2 sentence description of the aesthetic",
    "source": "figma | images | hybrid | verbal",
    "sourceUrls": ["array of source URLs or file paths"],
    "generated": "ISO 8601 timestamp",
    "version": "1.0"
  },

  "colors": {
    "primitive": {
      "description": "Raw color values organized by hue. These are the building blocks — semantic tokens alias into these.",
      "schema": {
        "<hue-name>": {
          "50": "#hex",
          "100": "#hex",
          "200": "#hex",
          "300": "#hex",
          "400": "#hex",
          "500": "#hex — the 'base' shade",
          "600": "#hex",
          "700": "#hex",
          "800": "#hex",
          "900": "#hex",
          "950": "#hex"
        }
      },
      "example": {
        "blue": { "50": "#eff6ff", "100": "#dbeafe", "500": "#3b82f6", "900": "#1e3a5f" },
        "gray": { "50": "#f9fafb", "100": "#f3f4f6", "500": "#6b7280", "900": "#111827" },
        "emerald": { "50": "#ecfdf5", "500": "#10b981", "900": "#064e3b" }
      },
      "notes": "Include at minimum: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900. Add 950 for very dark needs. Not every hue needs all 11 stops — use what the source design actually uses, and fill gaps mathematically."
    },

    "semantic": {
      "description": "Purpose-driven color assignments. Each token maps to a primitive value, with optional light/dark mode variants.",
      "schema": {
        "<token-name>": {
          "light": "#hex or reference like 'blue.500'",
          "dark": "#hex or reference like 'blue.300'",
          "description": "string — what this token is for"
        }
      },
      "tokens": {
        "primary": "Main brand/action color",
        "primary-hover": "Hover state of primary",
        "primary-active": "Active/pressed state of primary",
        "secondary": "Supporting accent color",
        "secondary-hover": "Hover state of secondary",
        "background": "Page/app background",
        "surface": "Card/panel background (elevated from background)",
        "surface-hover": "Surface on hover",
        "surface-active": "Surface on active/pressed",
        "muted": "Subdued background for tags, badges, inactive areas",
        "text-primary": "Main body text",
        "text-secondary": "Supporting/muted text",
        "text-tertiary": "Very subdued text (placeholders, captions)",
        "text-inverse": "Text on colored backgrounds",
        "border": "Default border color",
        "border-strong": "Emphasized borders",
        "border-subtle": "Very light borders (dividers)",
        "ring": "Focus ring color",
        "success": "Success/positive state",
        "warning": "Warning/caution state",
        "error": "Error/destructive state",
        "info": "Informational state"
      },
      "notes": "Not every design uses all tokens — include only what's actually present or strongly implied. For tokens with obvious light/dark parity, include both modes. The 'light' key is required, 'dark' is generated automatically if not explicitly found."
    },

    "gradients": {
      "description": "Named gradient definitions used in the design.",
      "schema": {
        "<gradient-name>": {
          "type": "linear | radial | conic",
          "angle": "number (degrees, for linear)",
          "stops": [
            { "color": "#hex", "position": "percentage" }
          ],
          "usage": "string — where this gradient is used"
        }
      }
    }
  },

  "typography": {
    "fontFamilies": {
      "description": "Font stacks used in the design.",
      "schema": {
        "display": {
          "family": "string — primary font name",
          "fallbacks": ["array of fallback fonts"],
          "googleFontsUrl": "string — Google Fonts import URL if applicable",
          "weights": [400, 500, 600, 700],
          "note": "Used for: headings, hero text, display elements"
        },
        "body": {
          "family": "string",
          "fallbacks": ["array"],
          "googleFontsUrl": "string",
          "weights": [400, 500, 600],
          "note": "Used for: body text, UI labels, descriptions"
        },
        "mono": {
          "family": "string",
          "fallbacks": ["array"],
          "googleFontsUrl": "string",
          "weights": [400, 500],
          "note": "Used for: code, data, technical content"
        }
      },
      "notes": "At minimum include 'display' and 'body'. 'mono' is optional but recommended. If the source design uses a single font family, set both display and body to the same font but note the different weights used for each context."
    },

    "scale": {
      "description": "Type scale — each level maps to a size, weight, line-height, and letter-spacing.",
      "schema": {
        "<level-name>": {
          "fontSize": "string (rem value, e.g., '2.25rem')",
          "fontSizePixels": "number (px equivalent for reference)",
          "lineHeight": "string (e.g., '1.2' or '2.5rem')",
          "fontWeight": "number (e.g., 700)",
          "letterSpacing": "string (e.g., '-0.025em')",
          "fontFamily": "display | body — which stack to use"
        }
      },
      "levels": [
        "display-2xl — hero/splash text (optional)",
        "display-xl — major page headings",
        "display-lg — section headings",
        "h1 — primary heading",
        "h2 — secondary heading",
        "h3 — tertiary heading",
        "h4 — quaternary heading",
        "body-lg — large body text (lead paragraphs)",
        "body — default body text",
        "body-sm — small body text",
        "caption — captions, footnotes",
        "overline — uppercase labels, tags"
      ],
      "notes": "Include only levels that are actually used in the source design. The names are suggestions — adapt to what the design actually has."
    }
  },

  "spacing": {
    "description": "Spacing scale values. Most design systems use a base unit multiplied up.",
    "baseUnit": "number (typically 4 or 8)",
    "scale": {
      "description": "Named spacing values.",
      "schema": {
        "0": "0px",
        "0.5": "2px (if base is 4)",
        "1": "4px",
        "1.5": "6px",
        "2": "8px",
        "3": "12px",
        "4": "16px",
        "5": "20px",
        "6": "24px",
        "8": "32px",
        "10": "40px",
        "12": "48px",
        "16": "64px",
        "20": "80px",
        "24": "96px"
      }
    },
    "semantic": {
      "description": "Purpose-driven spacing tokens (optional — use when the design has clear patterns).",
      "schema": {
        "page-padding": "spacing value — outer page padding",
        "section-gap": "spacing value — gap between major sections",
        "card-padding": "spacing value — internal card padding",
        "input-padding-x": "spacing value — horizontal input padding",
        "input-padding-y": "spacing value — vertical input padding",
        "button-padding-x": "spacing value",
        "button-padding-y": "spacing value",
        "stack-gap": "spacing value — default gap in vertical stacks",
        "inline-gap": "spacing value — default gap in horizontal rows"
      }
    }
  },

  "borders": {
    "widths": {
      "default": "1px",
      "thick": "2px",
      "heavy": "3px or 4px"
    },
    "radii": {
      "none": "0",
      "sm": "string (e.g., '0.25rem')",
      "default": "string (e.g., '0.5rem')",
      "md": "string",
      "lg": "string",
      "xl": "string",
      "2xl": "string",
      "full": "9999px"
    },
    "style": "solid | dashed — default border style observed"
  },

  "shadows": {
    "description": "Elevation levels expressed as box-shadow values.",
    "schema": {
      "<level-name>": {
        "value": "CSS box-shadow string",
        "usage": "string — when to use this level"
      }
    },
    "levels": {
      "xs": "Very subtle shadow (pressed/inset elements)",
      "sm": "Subtle shadow (buttons, inputs)",
      "default": "Standard shadow (cards, dropdowns)",
      "md": "Medium shadow (floating elements)",
      "lg": "Strong shadow (modals, popovers)",
      "xl": "Very strong shadow (toast notifications, overlays)",
      "inner": "Inset shadow (if used in design)"
    }
  },

  "effects": {
    "description": "Special visual effects observed in the design.",
    "backdrop": {
      "blur": "string — backdrop-filter blur value (e.g., '12px')",
      "saturation": "string — backdrop-filter saturate value if used"
    },
    "overlays": {
      "description": "Overlay patterns used in the design.",
      "schema": {
        "<overlay-name>": {
          "background": "CSS background value",
          "opacity": "number",
          "usage": "string"
        }
      }
    },
    "textures": {
      "description": "Texture/noise/grain effects.",
      "grain": "boolean — whether noise/grain texture is used",
      "grainOpacity": "number — opacity of grain overlay if used"
    },
    "notes": "Only include effects that are actually present in the source design. Don't invent effects."
  },

  "motion": {
    "description": "Animation and transition preferences.",
    "durations": {
      "fast": "string (e.g., '100ms')",
      "default": "string (e.g., '200ms')",
      "slow": "string (e.g., '300ms')",
      "slower": "string (e.g., '500ms')"
    },
    "easings": {
      "default": "CSS easing (e.g., 'cubic-bezier(0.4, 0, 0.2, 1)')",
      "in": "CSS easing for enter",
      "out": "CSS easing for exit",
      "bounce": "CSS easing for playful motion (if applicable)"
    },
    "notes": "Motion preferences are often implicit. If the source design doesn't show animations, derive reasonable defaults from the overall aesthetic — minimal designs get subtle/fast transitions, playful designs get bouncier/longer ones."
  },

  "styleNotes": {
    "description": "Qualitative observations about the design's personality. These don't map to CSS values but guide how the skin should be applied.",
    "aesthetic": "string — overall aesthetic direction (e.g., 'minimal and refined', 'bold and playful')",
    "density": "compact | comfortable | spacious",
    "mood": "string — emotional tone (e.g., 'professional and trustworthy', 'fun and energetic')",
    "distinguishingFeatures": ["array of strings — what makes this design unique"],
    "colorStrategy": "string — how color is used (e.g., 'monochromatic with single accent', 'vibrant multi-color')",
    "typographyStrategy": "string — how type is used (e.g., 'strong hierarchy with large display type', 'uniform and understated')",
    "recommendations": ["array of strings — suggestions for applying this skin effectively"]
  }
}
```

## Extraction Rules

### Color Extraction

1. **From Figma Variables**: Map directly. Primitive collections → `colors.primitive`, semantic collections → `colors.semantic`.
2. **From Figma Styles**: Extract color values, group by usage (fill, text, stroke).
3. **From Images**: Identify dominant colors, then build a palette. Use at minimum 3 hues (primary, neutral, accent). Generate the full shade scale (50-900) mathematically from observed values.
4. **Generating missing shades**: Given a base color (500), compute lighter shades by mixing toward white with decreasing saturation, and darker shades by mixing toward a dark blue-gray (not pure black) with slightly increased saturation.

### Typography Extraction

1. **From Figma**: `get_design_context` returns exact font specs. Map directly.
2. **From Images**: Identify the typeface (or closest match). If uncertain, list 2-3 candidates and note the uncertainty.
3. **Google Fonts matching**: When the exact font isn't freely available, suggest the closest Google Fonts alternative. Common substitutions:
   - SF Pro → Inter or Plus Jakarta Sans
   - Helvetica Neue → Inter
   - Circular → Plus Jakarta Sans
   - GT Walsheim → DM Sans
   - Tiempos → Lora or Source Serif Pro

### Spacing Extraction

1. **From Figma**: Look at auto-layout gap and padding values. Find the base unit (GCD of common values).
2. **From Images**: Estimate spacing relationships. Look for consistent patterns (e.g., card padding is always ~24px).

### Shadow Extraction

1. **From Figma Effect Styles**: Map directly to shadow levels.
2. **From Images**: Estimate shadow intensity and spread from visual appearance.

## Dark Mode Derivation

When the source design only provides a light mode, derive dark mode automatically:

1. **Background**: Invert — light backgrounds (gray-50/100) → dark (gray-900/950)
2. **Surface**: Invert — white/gray-50 → gray-800/850
3. **Text**: Invert — gray-900 → gray-100, gray-600 → gray-400
4. **Primary/Accent**: Keep hue, increase lightness by 1-2 stops (blue-600 → blue-400)
5. **Borders**: Keep opacity patterns, shift to lighter border on dark background
6. **Shadows**: Increase shadow opacity (shadows are less visible on dark backgrounds)
7. **Preserve the character**: Dark mode should feel like the same design, not a different one
