# Chakra v3 host: useToken returns a snapshot, not a CSS variable

If your app is on Chakra UI v3 and you pipe its theme tokens into the Kit's `theme` prop, you may see the Kit's modal stuck on light or dark colors no matter how you toggle the host theme. The Kit isn't broken — Chakra v3's `useToken` is returning a literal color snapshot, not a CSS variable reference.

### Symptom

* `next-themes` (or any class-based theming) toggles `html.class="dark"` correctly
* The host's own surfaces re-render (CSS variables resolve to the new mode)
* But the Kit's modal, card, sticky header, and other surfaces are stuck on whatever color was active when the page first hydrated
* Only Kit components that read `useVeChainKitConfig().darkMode` directly (e.g. the VeWorld button) update after a toggle

### Root cause

Chakra UI v3's `useToken` returns the **resolved value** at call time, not the CSS variable reference:

```js
// node_modules/@chakra-ui/react/dist/esm/styled-system/system.js
const tokenFn = (path, fallback) => tokenMap.get(path)?.value || fallback;
tokenFn.var = (path, fallback) => tokenMap.get(path)?.variable || fallback;
```

A wrapper like:

```tsx
// ❌ Snapshots the resolved color at render time
const [bgPrimary] = useToken('colors', ['bg.primary'])
// bgPrimary === '#1B1D1F'    when Chakra evaluated bg.primary in dark scope
// bgPrimary === 'white'      when it evaluated in light scope
```

is darkMode-frozen by the time the value reaches the Kit. The Kit receives a literal color and has no way to know it was meant to be reactive.

### Fix

Pass the **CSS variable reference** to the Kit so the underlying color stays governed by `html.class`. Two equivalent options:

#### Option A — use Chakra v3's `sys.token.var(...)` resolver

```tsx
import { useChakraContext } from '@chakra-ui/react'

const sys = useChakraContext()
const tokVar = (path: string) => sys.token.var(`colors.${path}`) as string

const bgPrimary       = tokVar('bg.primary')
const primaryDefault  = tokVar('actions.primary.default')
const primaryText     = tokVar('actions.primary.text')
const primaryHover    = tokVar('actions.primary.hover')
const secondaryDefault = tokVar('card.subtle')
const secondaryHover  = tokVar('card.hover')
const borderSecondary = tokVar('border.secondary')

// bgPrimary === 'var(--vbd-colors-bg-primary)'
```

#### Option B — hardcode the CSS variable

If your `cssVarsPrefix` is fixed:

```tsx
const bgPrimary = 'var(--vbd-colors-bg-primary)'
const primaryDefault = 'var(--vbd-colors-actions-primary-default)'
// …etc
```

Either way, the Kit's CSS variables now point at your host's variables, and the underlying color flips at paint time whenever `html.class` flips. No re-render needed.

### How to verify

Open the connect or account modal in light mode, then in DevTools inspect:

```text
--chakra-colors-vechain-kit-modal      ← should equal var(--your-prefix-...) or a CSS variable
--your-prefix-colors-bg-primary        ← should switch between values on theme toggle
```

If `--chakra-colors-vechain-kit-modal` resolves to a hex literal like `#1B1D1F` when the host is in light mode, your wrapper is still using `useToken(...).value`. Switch to `sys.token.var(...)`.

### Reference reproduction

The `examples/next-chakra-v3` workspace in [vechain/vechain-kit](https://github.com/vechain/vechain-kit) mirrors this exact host stack (Next.js + React 18 + Chakra v3 + next-themes) and demonstrates the correct wrapper pattern.
