# React Theme Output Template

Generate these files when the user selects React output.

## skin-theme.ts

```typescript
// =============================================================================
// Design Skin: {{SKIN_NAME}}
// React Theme Definition — fully typed
//
// Usage:
//   import { skinTheme } from './skin-theme'
//   <SkinProvider theme={skinTheme}>...</SkinProvider>
// =============================================================================

export interface SkinColors {
  primary: string
  primaryHover: string
  primaryActive: string
  secondary: string
  secondaryHover: string
  background: string
  surface: string
  surfaceHover: string
  surfaceActive: string
  muted: string
  textPrimary: string
  textSecondary: string
  textTertiary: string
  textInverse: string
  textLink: string
  textLinkHover: string
  border: string
  borderStrong: string
  borderSubtle: string
  ring: string
  success: string
  successBg: string
  warning: string
  warningBg: string
  error: string
  errorBg: string
  info: string
  infoBg: string
}

export interface SkinTypography {
  fontDisplay: string
  fontBody: string
  fontMono: string
  scale: Record<string, {
    fontSize: string
    lineHeight: string
    fontWeight: number
    letterSpacing: string
    fontFamily: string
  }>
}

export interface SkinSpacing {
  unit: number
  scale: Record<string, string>
}

export interface SkinBorders {
  radii: Record<string, string>
  widths: Record<string, string>
}

export interface SkinShadows {
  xs: string
  sm: string
  default: string
  md: string
  lg: string
  xl: string
  inner: string
}

export interface SkinMotion {
  durations: Record<string, string>
  easings: Record<string, string>
}

export interface SkinEffects {
  blur: Record<string, string>
  overlays: Record<string, string>
}

export interface SkinStyleNotes {
  aesthetic: string
  density: 'compact' | 'comfortable' | 'spacious'
  mood: string
  distinguishingFeatures: string[]
}

export interface SkinTheme {
  name: string
  colors: {
    light: SkinColors
    dark: SkinColors
  }
  typography: SkinTypography
  spacing: SkinSpacing
  borders: SkinBorders
  shadows: {
    light: SkinShadows
    dark: SkinShadows
  }
  motion: SkinMotion
  effects: SkinEffects
  styleNotes: SkinStyleNotes
}

export const skinTheme: SkinTheme = {
  name: '{{SKIN_NAME}}',
  colors: {
    light: {
      // {{LIGHT_COLORS}}
    },
    dark: {
      // {{DARK_COLORS}}
    },
  },
  typography: {
    // {{TYPOGRAPHY}}
  },
  spacing: {
    // {{SPACING}}
  },
  borders: {
    // {{BORDERS}}
  },
  shadows: {
    light: {
      // {{LIGHT_SHADOWS}}
    },
    dark: {
      // {{DARK_SHADOWS}}
    },
  },
  motion: {
    // {{MOTION}}
  },
  effects: {
    // {{EFFECTS}}
  },
  styleNotes: {
    // {{STYLE_NOTES}}
  },
}
```

## SkinProvider.tsx

```tsx
// =============================================================================
// Design Skin: {{SKIN_NAME}}
// Theme Provider with dark/light mode support
//
// Usage:
//   import { SkinProvider } from './SkinProvider'
//   import { skinTheme } from './skin-theme'
//
//   <SkinProvider theme={skinTheme}>
//     <App />
//   </SkinProvider>
// =============================================================================

import React, { createContext, useContext, useState, useEffect, useCallback, useMemo } from 'react'
import type { SkinTheme, SkinColors } from './skin-theme'

type ColorMode = 'light' | 'dark' | 'system'

interface SkinContextValue {
  theme: SkinTheme
  colorMode: ColorMode
  resolvedMode: 'light' | 'dark'
  setColorMode: (mode: ColorMode) => void
  colors: SkinColors
}

const SkinContext = createContext<SkinContextValue | null>(null)

interface SkinProviderProps {
  theme: SkinTheme
  defaultMode?: ColorMode
  storageKey?: string
  children: React.ReactNode
}

export function SkinProvider({
  theme,
  defaultMode = 'system',
  storageKey = 'skin-color-mode',
  children,
}: SkinProviderProps) {
  const [colorMode, setColorModeState] = useState<ColorMode>(() => {
    if (typeof window === 'undefined') return defaultMode
    const stored = localStorage.getItem(storageKey)
    return (stored as ColorMode) || defaultMode
  })

  const [systemPreference, setSystemPreference] = useState<'light' | 'dark'>('light')

  useEffect(() => {
    const mq = window.matchMedia('(prefers-color-scheme: dark)')
    setSystemPreference(mq.matches ? 'dark' : 'light')
    const handler = (e: MediaQueryListEvent) => setSystemPreference(e.matches ? 'dark' : 'light')
    mq.addEventListener('change', handler)
    return () => mq.removeEventListener('change', handler)
  }, [])

  const resolvedMode = colorMode === 'system' ? systemPreference : colorMode

  const setColorMode = useCallback((mode: ColorMode) => {
    setColorModeState(mode)
    if (typeof window !== 'undefined') {
      localStorage.setItem(storageKey, mode)
    }
  }, [storageKey])

  // Sync CSS custom properties with the theme
  useEffect(() => {
    const root = document.documentElement
    const colors = theme.colors[resolvedMode]
    const shadows = theme.shadows[resolvedMode]

    // Toggle dark class
    root.classList.toggle('dark', resolvedMode === 'dark')

    // Set color variables
    Object.entries(colors).forEach(([key, value]) => {
      const cssKey = `--skin-${key.replace(/([A-Z])/g, '-$1').toLowerCase()}`
      root.style.setProperty(cssKey, value)
    })

    // Set shadow variables
    Object.entries(shadows).forEach(([key, value]) => {
      root.style.setProperty(`--skin-shadow-${key}`, value)
    })

    // Set typography variables
    root.style.setProperty('--skin-font-display', theme.typography.fontDisplay)
    root.style.setProperty('--skin-font-body', theme.typography.fontBody)
    root.style.setProperty('--skin-font-mono', theme.typography.fontMono)

    // Set spacing variables
    root.style.setProperty('--skin-space-unit', `${theme.spacing.unit}px`)
    Object.entries(theme.spacing.scale).forEach(([key, value]) => {
      root.style.setProperty(`--skin-space-${key}`, value)
    })

    // Set border variables
    Object.entries(theme.borders.radii).forEach(([key, value]) => {
      root.style.setProperty(`--skin-radius-${key}`, value)
    })

    // Set motion variables
    Object.entries(theme.motion.durations).forEach(([key, value]) => {
      root.style.setProperty(`--skin-duration-${key}`, value)
    })
    Object.entries(theme.motion.easings).forEach(([key, value]) => {
      root.style.setProperty(`--skin-ease-${key}`, value)
    })
  }, [theme, resolvedMode])

  const value = useMemo(() => ({
    theme,
    colorMode,
    resolvedMode,
    setColorMode,
    colors: theme.colors[resolvedMode],
  }), [theme, colorMode, resolvedMode, setColorMode])

  return (
    <SkinContext.Provider value={value}>
      {children}
    </SkinContext.Provider>
  )
}

export { SkinContext }
```

## useSkin.ts

```typescript
// =============================================================================
// Design Skin: {{SKIN_NAME}}
// Hook for consuming the skin theme
//
// Usage:
//   const { colors, theme, colorMode, setColorMode } = useSkin()
// =============================================================================

import { useContext } from 'react'
import { SkinContext } from './SkinProvider'

export function useSkin() {
  const context = useContext(SkinContext)
  if (!context) {
    throw new Error('useSkin must be used within a <SkinProvider>')
  }
  return context
}

// Convenience hooks
export function useSkinColors() {
  return useSkin().colors
}

export function useSkinMode() {
  const { colorMode, resolvedMode, setColorMode } = useSkin()
  return { colorMode, resolvedMode, setColorMode }
}
```

## Generation Rules

1. **Full TypeScript types** — every token is typed, enabling autocomplete and type safety.
2. **CSS custom properties sync** — the provider writes theme values to CSS vars, so non-React parts of the page also get themed.
3. **SSR-safe** — check for `typeof window` before accessing localStorage/matchMedia.
4. **Color mode persistence** — saved to localStorage, respects system preference as default.
5. **Clean context API** — expose only what consumers need (colors, mode, toggle).
6. **Memoized** — context value is memoized to prevent unnecessary re-renders.
