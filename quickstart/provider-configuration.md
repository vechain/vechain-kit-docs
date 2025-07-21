# Provider Configuration

This guide covers how to set up and configure the `VeChainKitProvider` in your application.

### Basic Setup

To use VeChain Kit in your application, wrap your app with the `VeChainKitProvider`:

```typescript
'use client';

import { VeChainKitProvider } from "@vechain/vechain-kit";

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <VeChainKitProvider
      network={{ type: "test" }}
      feeDelegation={{
        delegatorUrl: "https://sponsor-testnet.vechain.energy/by/441",
        delegateAllTransactions: false,
      }}
    >
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

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <VeChainKitProvider
      // ... configuration
    >
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
      
      // Fee Delegation
      feeDelegation={{
        delegatorUrl: "https://sponsor-testnet.vechain.energy/by/441",
        delegateAllTransactions: false, // Set to true to delegate all transactions
      }}
      
      // Login Methods Configuration
      loginMethods={[
        { method: "vechain", gridColumn: 4 },
        { method: "dappkit", gridColumn: 4 },
        { method: "ecosystem", gridColumn: 4 },
      ]}
      
      // DApp Kit Configuration
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
      
      // UI Configuration
      darkMode={false}
      language="en"
      allowCustomTokens={false}
      
      // Login Modal UI Customization
      loginModalUI={{
        logo: '/your-logo.png',
        description: 'Welcome to our DApp',
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
  delegateAllTransactions: boolean // true: delegate all | false: only social login
}
```

**Note**: Social login transaction sponsorship is mandatory and always enabled.

#### Login Methods

Configure available authentication methods with a flexible grid layout:

```typescript
loginMethods: [
  // Always available methods
  { method: "vechain", gridColumn: 4 },    // VeChain login
  { method: "dappkit", gridColumn: 4 },    // VeChain wallets
  { method: "ecosystem", gridColumn: 4 },  // Ecosystem apps (Mugshot, Cleanify, Greencart, etc.)
  
  // Privy-dependent methods (require privy configuration)
  { method: "email", gridColumn: 2 },      // Email login
  { method: "passkey", gridColumn: 2 },    // Passkey authentication
  { method: "google", gridColumn: 4 },     // Google OAuth
  { method: "more", gridColumn: 2 },       // Additional Privy methods
]
```

**Grid Layout**: The `gridColumn` property determines the width of each login option in the modal (based on a 4-column grid).

#### DApp Kit Configuration

Configure wallet connection options:

```typescript
dappKit: {
  allowedWallets: ["veworld", "wallet-connect", "sync2"],
  walletConnectOptions: {
    projectId: "YOUR_WALLET_CONNECT_PROJECT_ID", // Get from https://cloud.reown.com
    metadata: {
      name: string,
      description: string,
      url: string,
      icons: string[]
    }
  }
}
```

### Privy Integration (Optional)

To enable social login methods, configure Privy:

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

#### Type Safety with Privy

TypeScript enforces that Privy-dependent methods can only be used when Privy is configured:

```typescript
// ❌ This will show a type error
const invalidConfig = {
  // No privy configuration
  loginMethods: [
    { method: 'email' }  // Type error: 'email' requires Privy
  ]
};

// ✅ This is valid
const validConfig = {
  privy: { appId: 'xxx', clientId: 'yyy' },
  loginMethods: [
    { method: 'email' },   // OK: Privy is configured
    { method: 'vechain' }  // OK: Always available
  ]
};
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

### Environment Variables

Required environment variables:

```bash
# .env.local
NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID=your_project_id_here
```

### Best Practices

1. **Dynamic Import**: Always use dynamic import in Next.js to avoid SSR issues
2. **Environment Variables**: Store sensitive configuration in environment variables
3. **Fee Delegation**: Consider your fee delegation strategy based on user experience needs
4. **Login Methods**: Choose login methods that match your target audience
5. **Metadata**: Provide clear app metadata for wallet connection requests

### Next Steps

* Implement Authentication Methods
* Configure Fee Delegation
* Customize UI Theme
* Handle Wallet Interactions
