# Provider Configuration

This guide covers how to set up and configure the `VeChainKitProvider` in your application.

### Basic Setup

Wrap your app with the `VeChainKitProvider`:

```typescript
'use client';

import { VeChainKitProvider } from "@vechain/vechain-kit";

export function Providers({ children }) {
  return (
    <VeChainKitProvider>
      {children}
    </VeChainKitProvider>
  );
}
```

### Next.js Configuration

For Next.js applications, dynamically import the provider to avoid SSR issues:

```typescript
import dynamic from 'next/dynamic';

const VeChainKitProvider = dynamic(
  async () => (await import('@vechain/vechain-kit')).VeChainKitProvider,
  { ssr: false }
);

export function Providers({ children }) {
  return (
    <VeChainKitProvider>
      {children}
    </VeChainKitProvider>
  );
}
```

### Complete Configuration Example

Here's a comprehensive example with all available options:

```typescript
'use client';

import { VeChainKitProvider } from "@vechain/vechain-kit";

export function VeChainKitProviderWrapper({ children }: { children: React.ReactNode }) {
  return (
    <VeChainKitProvider
      // Network Configuration
      network={{
        type: "test", // "main" | "test" | "solo"
      }}
      
      // UI Configuration
      darkMode={false}
      language="en"
      theme={{
        // Brand accent — spinner, focus rings, "Waiting for signature…" headline
        accent: '#3b82f6',
        modal: { backgroundColor: 'black' },
      }}
      
      // Login Modal UI Customization
      loginModalUI={{
        logo: '/your-logo.png',
        description: 'Welcome to our DApp',
      }}
      
      // Login Methods Configuration (variation A default)
      loginMethods={[
        { method: "veworld", gridColumn: 4 },  // primary CTA, recommended
        { method: "google",  gridColumn: 4 },
        { method: "apple",   gridColumn: 4 },
        { method: "more",    gridColumn: 4 },  // overflow sub-view (wallets / socials / ecosystem)
      ]}
      
      // Sponsor transactions
      feeDelegation={{
          delegatorUrl: process.env.NEXT_PUBLIC_DELEGATOR_URL!,
      }}
      
      // Wallet Connection Configuration
      dappKit={{
        allowedWallets: ["veworld", "wallet-connect", "sync2"],
        walletConnectOptions: {
          projectId: process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID!,
          metadata: {
            name: "Your DApp Name",
            description: "Your DApp description visible in wallets",
            url: typeof window !== "undefined" ? window.location.origin : "",
            icons: ["https://your-domain.com/logo.png"],
          },
        },
      }}

      // Contract Address Overrides (optional)
      // Override default contract addresses for custom deployments
      contractAddresses={{
        b3trContractAddress: "0x...",
        vot3ContractAddress: "0x...",
      }}
    >
      {children}
    </VeChainKitProvider>
  );
}
```

### Configuration Options

#### Network Configuration

```typescript
network: {
  type: "main" | "test" | "solo" // Select mainnet or testnet or solo
}
```

#### Fee Delegation

Configure transaction fee sponsorship:

```typescript
feeDelegation: {
  delegatorUrl: string,           // Fee delegation service URL
}
```

#### Login Methods

Configure available authentication methods with a flexible grid layout. Each entry pins a `method` and an optional `gridColumn` (1–4) controlling how many of the 4 columns the button spans.

```typescript
loginMethods: [
  // --- Wallets (drives @vechain/dapp-kit programmatically; kit owns the UI) ---
  { method: "veworld",        gridColumn: 4 },  // primary CTA — filled, recommended dot
  { method: "sync2",          gridColumn: 4 },
  { method: "wallet-connect", gridColumn: 4 },  // triggers WC's own QR modal

  // --- VeChain native ---
  { method: "vechain",   gridColumn: 4 },       // VeChain cross-app social login
  { method: "ecosystem", gridColumn: 4 },       // x2earn ecosystem apps footer

  // --- Social (routed through VeChain's whitelabel cross-app popup unless you pass `privy`) ---
  { method: "google",  gridColumn: 4 },         // works without `privy` (cross-app intent)
  { method: "apple",   gridColumn: 4 },         // works without `privy`
  { method: "twitter", gridColumn: 4 },         // works without `privy`
  { method: "discord", gridColumn: 4 },         // works without `privy`
  { method: "github",  gridColumn: 4 },         // works without `privy`
  { method: "tiktok",  gridColumn: 4 },         // works without `privy`
  { method: "line",    gridColumn: 4 },         // works without `privy`
  { method: "email",   gridColumn: 4 },         // requires your own `privy` (inline, no popup)
  { method: "passkey", gridColumn: 4 },         // requires your own `privy`
  { method: "sms",     gridColumn: 4 },         // requires your own `privy`
  { method: "more",    gridColumn: 4 },         // sub-view with overflow socials/wallets/ecosystem

  // --- Legacy ---
  { method: "dappkit", gridColumn: 4 },         // Opens dapp-kit's native picker. Preserved for backwards compat.
]
```

**Defaults** (when `loginMethods` is omitted):

* With `privy`: `[veworld, google, apple, more]`
* Without `privy`: `[veworld, sync2, wallet-connect]`

**Gating**: granular wallet methods (`veworld`, `sync2`, `wallet-connect`) only render when their source is also in `dappKit.allowedWallets`.

For the full layout, theming and migration guide, see [Login Customization](login-customization.md).

#### Contract Address Overrides

Override default contract addresses for custom deployments on solo or testnet. Only the provided fields are overridden; the rest use the network defaults.

```typescript
contractAddresses: Partial<AppConfig>  // Any field from AppConfig can be overridden
```

This is useful when you deploy your own contract instances (e.g., B3TR, VOT3) and need the kit to use those addresses instead of the built-in defaults:

```typescript
<VeChainKitProvider
  network={{ type: "solo" }}
  contractAddresses={{
    b3trContractAddress: "0x026771d1be764467f8bdb78bb230df10c924b00d",
    vot3ContractAddress: "0xf7a08af15cb3501feee53ebe11f4428a966fa459",
    // You can override any AppConfig field — see Configs page for the full list
  }}
>
  {children}
</VeChainKitProvider>
```

To access the merged config (network defaults + your overrides) in your components, use the `useAppConfig` hook:

```typescript
import { useAppConfig } from '@vechain/vechain-kit';

function MyComponent() {
  const config = useAppConfig();
  console.log(config.b3trContractAddress); // Your overridden address
}
```

### Privy Integration (Optional)

Most social logins work without your own Privy account — the kit routes Google / Apple / X / Discord / GitHub / TikTok / LINE through VeChain's whitelabel cross-app popup automatically. Pass the `privy` prop only if you need email, passkey, SMS, additional OAuth providers, or want the login flow to render entirely inside your dApp instead of in a popup window.

To enable those flows with your own Privy account:

```typescript
<VeChainKitProvider
  privy={{
    appId: "your-privy-app-id",
    clientId: "your-privy-client-id",
    // Additional Privy configuration
  }}
  loginMethods={[
    // Now you can use Privy-dependent methods
    { method: "email", gridColumn: 2 },
    { method: "google", gridColumn: 4 },
    { method: "passkey", gridColumn: 2 },
  ]}
>
  {children}
</VeChainKitProvider>
```

### Ecosystem Apps Configuration

Customize which ecosystem apps appear in the login modal:

```typescript
<VeChainKitProvider
  loginMethods={[
    { method: "ecosystem", gridColumn: 4 }
  ]}
  ecosystemApps={{
    allowedApps: ["app-id-1", "app-id-2"], // Find app IDs in Privy dashboard
  }}
>
  {children}
</VeChainKitProvider>
```

### Best Practices

1. **Dynamic Import**: Always use dynamic import in Next.js to avoid SSR issues
2. **Environment Variables**: Store sensitive configuration in environment variables
3. **Fee Delegation**: Consider your fee delegation strategy based on user experience needs
4. **Login Methods**: Choose login methods that match your target audience
5. **Metadata**: Provide clear app metadata for wallet connection requests

### Next Steps

* Implement Authentication Methods
* Customize UI Theme
* Handle Wallet Interactions
