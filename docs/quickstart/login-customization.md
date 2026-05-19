# Login Customization

This guide covers the connect-modal authentication surface in VeChain Kit. From v2.7 onward the kit owns the entire connection UI for VeWorld and Sync2 — there is no hand-off to dapp-kit's native picker.

### What's new in v2.7

- **Custom in-house connection flow** for VeWorld and Sync2. Clicking a wallet button drives `@vechain/dapp-kit` programmatically (`setSource()` + `connect()`) and renders the kit's own "Waiting for signature…" view. WalletConnect's QR modal is preserved because that modal _is_ the QR.
- **Granular `loginMethods`**: `veworld`, `sync2`, `wallet-connect`, `apple` are now first-class entries you can pin individually. The legacy `dappkit` value still works.
- **Variation A layout**: one recommended primary CTA (filled inverted + "recommended" dot) — usually VeWorld in the default config, but the dev controls which method gets the emphasis via `isPrimary`; the rest render as outline secondary. "More options ⌄" link footer opens an in-modal sub-view with overflow wallets / socials / ecosystem apps.
- **Themeable accent**: spinner, focus rings and the "Waiting for signature…" headline read from `theme.accent`.

### Overview

VeChain Kit provides four main authentication approaches:

1. **Pre-built `WalletButton`** — fastest, handles state automatically.
2. **Custom button that opens the modal** — `useConnectModal()`.
3. **Custom button that triggers a single wallet directly** — `useConnectWithDappKitSource(source, setContent)`.
4. **Wallet-only via the legacy dapp-kit modal** — `useDAppKitWalletModal()`.

### Method 1: WalletButton

The simplest path. Renders the kit's standard login pill, owns connection state, and switches between login / profile automatically.

```tsx
'use client';
import { WalletButton } from '@vechain/vechain-kit';

export function Page() {
  return <WalletButton />;
}
```

### Method 2: Custom button → kit's connect modal

```tsx
'use client';
import { useConnectModal, useAccountModal, useWallet } from '@vechain/vechain-kit';

export function CustomAuthButton() {
  const { connection } = useWallet();
  const { open: openConnect } = useConnectModal();
  const { open: openAccount } = useAccountModal();

  return connection.isConnected ? (
    <button onClick={openAccount}>View account</button>
  ) : (
    <button onClick={openConnect}>Sign in</button>
  );
}
```

### Method 3: Trigger one wallet, no grid

Skip the grid entirely and open the kit's "Waiting for signature…" view straight away — useful when your app is wallet-specific.

```tsx
'use client';
import { useConnectWithDappKitSource, useModal } from '@vechain/vechain-kit';

export function ConnectWithVeWorld() {
  const { setConnectModalContent, openConnectModal } = useModal();
  const { connect } = useConnectWithDappKitSource('veworld', setConnectModalContent);

  return (
    <button onClick={async () => {
      openConnectModal();           // show the modal as the loading surface
      await connect();              // drives setSource('veworld') + connect()
    }}>
      Connect VeWorld
    </button>
  );
}
```

Allowed sources: `'veworld' | 'sync2' | 'wallet-connect'`. WalletConnect will additionally pop its own QR modal.

### Method 4: OAuth from a custom UI (Privy)

For self-hosted Privy apps, you can drive OAuth flows directly:

```tsx
import { useLoginWithOAuth } from '@vechain/vechain-kit';

const { initOAuth } = useLoginWithOAuth();
await initOAuth({ provider: 'google' }); // 'google' | 'apple' | 'github' | 'discord' | 'twitter' | 'linkedin'
```

### Configuring the grid: `loginMethods`

The order and shape of the connect-modal grid is controlled by `loginMethods` on `<VeChainKitProvider>`. Each entry has a `method`, an optional `gridColumn` (1–4 — buttons span that many of the 4 columns), and an optional `isPrimary` flag that controls which button gets the recommended-CTA treatment.

```tsx
<VeChainKitProvider
  privy={{
    appId: '...',
    clientId: '...',
    loginMethods: ['google', 'apple', 'email', 'twitter', 'discord'],
    appearance: { /* ... */ },
  }}
  dappKit={{
    allowedWallets: ['veworld', 'sync2', 'wallet-connect'],
    walletConnectOptions: { /* ... */ },
  }}
  loginMethods={[
    { method: 'veworld', gridColumn: 4, isPrimary: true },  // recommended CTA — filled, "recommended" dot
    { method: 'google',  gridColumn: 4 },                   // outline secondary
    { method: 'apple',   gridColumn: 4 },                   // outline secondary
    { method: 'more',    gridColumn: 4 },                   // opens an in-modal sub-view
  ]}
>
```

#### Recommended primary CTA — `isPrimary`

One method per grid renders as the recommended CTA: filled inverted surface (dark on light mode, white on dark mode) with a small green dot. The rest render as outline secondary. Two ways the kit picks which:

1. **Explicit** — any entry with `isPrimary: true` (excluding `more`, which is a footer link). If multiple entries set `isPrimary`, the first one wins.
2. **Implicit fallback** — when no entry sets `isPrimary`, the kit highlights the first visible method in the array. So a minimal config like `[{ method: 'google' }, { method: 'apple' }]` still gets Google as the recommended CTA without thinking about emphasis.

Currently supported as primary: `veworld`, `google`, `apple`, `github`. Other methods can sit on the main grid but won't switch to the filled treatment when first / `isPrimary: true` — they keep their outline look. (Adding more is a one-prop change per button; open an issue if you need it.)

```tsx
// Default: Google is primary because it's first.
loginMethods={[
  { method: 'google',  gridColumn: 4 },
  { method: 'apple',   gridColumn: 4 },
  { method: 'more',    gridColumn: 4 },
]}

// Explicit: Apple is primary, Google is outline.
loginMethods={[
  { method: 'google',  gridColumn: 4 },
  { method: 'apple',   gridColumn: 4, isPrimary: true },
  { method: 'more',    gridColumn: 4 },
]}
```

#### Method values

| Method            | Requires                                                                                  | Renders                                                                                  |
|-------------------|-------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| `veworld`         | `dappKit.allowedWallets` includes `'veworld'`                                             | Custom VeWorld flow + the kit's "Waiting for signature…" view                            |
| `sync2`           | `dappKit.allowedWallets` includes `'sync2'`                                               | Custom Sync2 flow + same waiting view                                                    |
| `wallet-connect`  | `dappKit.allowedWallets` includes `'wallet-connect'` + `walletConnectOptions.projectId`   | Triggers WalletConnect's QR modal programmatically (kit's loading view sits behind)      |
| `google`          | `privy`                                                                                   | Privy Google OAuth (full color "G")                                                      |
| `apple`           | `privy`                                                                                   | Privy Apple OAuth                                                                        |
| `github`          | `privy`                                                                                   | Privy GitHub OAuth                                                                       |
| `email`           | `privy`                                                                                   | Inline email pill + 6-digit code modal                                                   |
| `passkey`         | `privy`                                                                                   | Privy WebAuthn                                                                           |
| `vechain`         | `privy`                                                                                   | VeChain cross-app login (single wallet across Privy ecosystem apps)                      |
| `ecosystem`       | —                                                                                         | Footer button that opens a sub-view listing x2earn ecosystem apps                        |
| `more`            | —                                                                                         | "More options ⌄" link footer → in-modal sub-view with all overflow options              |
| `dappkit`         | `dappKit`                                                                                 | **Legacy.** Opens dapp-kit's native picker modal — preserved for backwards compatibility |

#### The `'more'` sub-view

When the user taps **More options ⌄**, the modal cross-fades into a sub-view that surfaces _everything you configured but didn't put on the main grid_:

- **Other wallets** — entries in `dappKit.allowedWallets` not on the main grid (VeWorld / Sync2 / WalletConnect).
- **Other sign-in** — natively-rendered Privy methods in `privy.loginMethods` (Google, Apple, GitHub, email, passkey). Anything else you configured in Privy (Twitter, Discord, Farcaster, TikTok, LinkedIn, …) is reachable via a fallback link that opens Privy's own modal.
- **Ecosystem apps** — the x2earn apps configured via Privy ecosystem.

Items already shown on the main grid are excluded. Sections collapse when they'd be empty. The dev can hide the whole sub-view by omitting `'more'` from `loginMethods`.

#### Default `loginMethods`

If you don't pass `loginMethods`, the kit picks a sensible default:

```ts
// With Privy:
loginMethods = [
  { method: 'veworld', gridColumn: 4 },
  { method: 'google',  gridColumn: 4 },
  { method: 'apple',   gridColumn: 4 },
  { method: 'more',    gridColumn: 4 },
];

// Without Privy:
loginMethods = [
  { method: 'veworld',        gridColumn: 4 },
  { method: 'sync2',          gridColumn: 2 },
  { method: 'wallet-connect', gridColumn: 2 },
];
```

#### Special: only `dappkit`

When `loginMethods` is exactly `[{ method: 'dappkit', ... }]`, `useConnectModal().open()` opens dapp-kit's native picker directly — the kit's modal is never rendered. This preserves the v2.6.x behavior for apps that explicitly pinned `dappkit`.

### Theming the connect modal

The modal honors the theme tokens passed to `<VeChainKitProvider>`:

- **Modal surface / overlay / borders** — `theme.modal.backgroundColor`, `theme.modal.border`, `theme.modal.rounded`, `theme.overlay.backgroundColor`.
- **Text colors** — `theme.textColor` cascades to primary / secondary / tertiary.
- **Primary button (VeWorld CTA)** — `theme.buttons.primaryButton.{bg, color, border, rounded, hoverBg}`.
- **Brand accent** — `theme.accent` controls the spinner top arc, focus rings, the "Waiting for signature…" headline and the email-submit link when the address is valid. Default `#3b82f6` (light) / `#60a5fa` (dark).

```tsx
<VeChainKitProvider
  theme={{
    accent: '#ff6600',
    buttons: {
      primaryButton: { bg: '#0E0D18', color: '#FFFFFF', rounded: '16px' },
    },
  }}
>
```

**Brand-locked surfaces** (intentionally not themed): Google's white tile + colored "G", Apple's glyph, WalletConnect's `#3B99FC`, GitHub's `#24292e`, the recommended-provider green dot on VeWorld. These have to remain recognisable as brand icons.

### VeWorld mobile in-app browser

When your dApp is opened from inside the VeWorld mobile wallet browser, the kit detects `window.vechain.isInAppBrowser` and skips the grid entirely — `setSource('veworld') + connectV2(null)` runs as soon as the connect intent is triggered. The user sees no modal because they're already inside the wallet.

### Migrating from v2.6.x

- **No breaking change** for apps that pinned `{ method: 'dappkit' }` — it still opens dapp-kit's modal exactly as before.
- The **default** `loginMethods` changed. If you _did not_ pass `loginMethods`, the modal previously rendered `[vechain, ecosystem, dappkit]` and now renders `[veworld, google, apple, more]` (Privy) or `[veworld, sync2, wallet-connect]` (no Privy). To keep the v2.6 grid, pass it explicitly.
- The granular methods (`veworld`, `sync2`, `wallet-connect`) are gated on `dappKit.allowedWallets`. If a method is in your `loginMethods` but its source isn't in `allowedWallets`, the button is hidden.

### Error handling

The kit handles rejections (`'rejected' | 'cancelled' | 'user denied' | 'closed'`) by returning to the main grid silently. Any other error transitions the modal to the redesigned error view (red disc + **Back** / **Try again**). If you drive `useConnectWithDappKitSource` yourself, the same state machine applies — your `setCurrentContent` setter is what flips between loading / error / main.
