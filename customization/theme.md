# Theme

This guide explains how to customize the VeChain Kit theme to match your app's design.

### Quick Start

The theme system is designed to be simple - you only need to provide a base `backgroundColor` and `textColor`, and all other colors are automatically derived. You can optionally customize specific aspects like overlay, buttons, and glass effects.

```tsx
<VeChainKitProvider
    theme={{
        backgroundColor: isDarkMode ? '#1f1f1e' : '#ffffff',
        textColor: isDarkMode ? 'rgb(223, 223, 221)' : '#2e2e2e',
        overlay: {
            backgroundColor: 'rgba(0, 0, 0, 0.6)',
            blur: 'blur(3px)',
        },
        effects: {
            glass: {
                enabled: true,
                intensity: 'low',
            },
        },
    }}
    // ... other props
>
    {children}
</VeChainKitProvider>
```

### Simplified API

The theme configuration has been simplified to focus on what matters most:

#### Base Colors

* **`backgroundColor`** (optional) - Base background color. Automatically derives:
  * Modal background (100% opacity)
  * Card background (80% opacity)
  * Sticky header background (90% opacity)
  * Secondary/tertiary colors (with opacity overlays)
  * Border colors
* **`textColor`** (optional) - Base text color. Automatically derives:
  * Primary text (100% opacity)
  * Secondary text (70% opacity)
  * Tertiary text (50% opacity)

#### Overlay Configuration

Customize the modal overlay independently:

```tsx
overlay: {
    backgroundColor: 'rgba(0, 0, 0, 0.6)', // Overlay background color
    blur: 'blur(10px)', // Overlay blur effect
}
```

#### Button Customization

Customize button styles:

```tsx
button: {
    bg: 'rgba(255, 255, 255, 0.1)', // Background color
    color: '#ffffff', // Text color
    border: '1px solid rgba(255, 255, 255, 0.2)', // Border (full CSS string)
}
```

This applies to all `vechainKitSecondary` buttons. Hover and active states are handled automatically through opacity.

#### Login Button Customization

Customize login button styles (applies to `loginIn` variant):

```tsx
loginButton: {
    bg: 'transparent', // Background color
    color: '#ffffff', // Text color
    border: '1px solid rgba(255, 255, 255, 0.1)', // Border (full CSS string)
}
```

Hover and active states are handled automatically through opacity.

### Complete Example

Here's a complete example with glass effects:

```tsx
import type { VechainKitThemeConfig } from '@vechain/vechain-kit';

const theme: VechainKitThemeConfig = {
    backgroundColor: isDarkMode ? '#1f1f1e' : '#ffffff',
    textColor: isDarkMode ? 'rgb(223, 223, 221)' : '#2e2e2e',
    overlay: {
        backgroundColor: isDarkMode
            ? 'rgba(0, 0, 0, 0.6)'
            : 'rgba(0, 0, 0, 0.4)',
        blur: 'blur(3px)',
    },
    button: {
        bg: isDarkMode ? 'rgba(255, 255, 255, 0.05)' : 'rgba(0, 0, 0, 0.1)',
        color: isDarkMode ? 'rgb(223, 223, 221)' : '#2e2e2e',
        border: 'none',
    },
    loginButton: {
        bg: 'transparent',
        color: isDarkMode ? 'white' : '#1a1a1a',
        border: isDarkMode
            ? '1px solid rgba(255, 255, 255, 0.1)'
            : '1px solid #ebebeb',
    },
    fonts: {
        family: 'Inter, sans-serif',
        sizes: {
            small: '12px',
            medium: '14px',
            large: '16px',
        },
        weights: {
            normal: 400,
            medium: 500,
            bold: 700,
        },
    },
    effects: {
        glass: {
            enabled: true,
            intensity: 'low',
        },
    },
};

<VeChainKitProvider theme={theme} {...otherProps}>
    {children}
</VeChainKitProvider>;
```

### Partial Configuration

You only need to specify the values you want to customize. All other values will use sensible defaults:

```tsx
// Minimal config - just enable glass effects
theme={{
    effects: {
        glass: {
            enabled: true,
            intensity: 'medium',
        },
    },
}}

// Customize overlay only
theme={{
    overlay: {
        backgroundColor: 'rgba(0, 0, 0, 0.5)',
        blur: 'blur(5px)',
    },
}}

// Customize buttons only
theme={{
    button: {
        bg: '#6366f1',
        color: '#ffffff',
        border: 'none',
    },
}}

// Customize fonts only
theme={{
    fonts: {
        family: 'Inter, sans-serif',
    },
}}
```
