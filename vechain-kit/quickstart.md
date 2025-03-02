---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Quickstart

## 1) Install package

{% code overflow="wrap" %}
```sh
yarn add @tanstack/react-query@"^5.64.2" @chakra-ui/react@"^2.8.2" @vechain/dapp-kit-react@"1.4.1" @vechain/vechain-kit

// or

npm i @tanstack/react-query@"^5.64.2" @chakra-ui/react@"^2.8.2" @vechain/dapp-kit-react@"1.4.1" @vechain/vechain-ki
```
{% endcode %}

{% hint style="danger" %}
Only supported on React and Next.js
{% endhint %}

{% hint style="warning" %}
React query, chakra and dapp-kit are peer dependencies.
{% endhint %}

## 2) Define Provider

```typescript
'use client';

import VeChainKitProvider from '@vechain/vechain-kit'

export function VeChainKitProviderWrapper({ children }: Props) {
    return (
         <VechainKitProvider
            feeDelegation={{
                delegatorUrl: process.env.NEXT_PUBLIC_DELEGATOR_URL!
            }}
            dappKit={{
                 allowedWallets: ['veworld', 'wallet-connect', 'sync2'],
                 walletConnectOptions: {
                    projectId:
                        // Get this on https://cloud.reown.com/sign-in
                        process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID!,
                    metadata: {
                        name: 'Your App Name',
                        description:
                            'This is a demo description.',
                        url:
                            typeof window !== 'undefined'
                                ? window.location.origin
                                : '',
                        icons: [
                            typeof window !== 'undefined' ? "https://path-to-logo.png" : '',
                        ],
                    },
                },
            }}
            darkMode={true}
            language="en"
            network={{
                type: 'main',
            }}
        >
            {children}
        </VechainKitProvider>
    );
}

```

On Next.js you will need to dynamically load the import

```typescript
import dynamic from 'next/dynamic';

const VeChainKitProviderWrapper = dynamic(
    async () =>
        (await import('@vechain/vechain-kit')).VeChainKitProvider,
    {
        ssr: false,
    },
);
```

### Available Login Methods

The modal supports several authentication methods:

* Social Login - Email and Google authentication through Privy (only available for self hosted Privy)
* VeChain Login - Direct VeChain wallet authentication
* Passkey - Biometric/device-based authentication (only available for self hosted Privy)
* DappKit - Connection through VeWorld or other VeChain wallets
* Ecosystem - Cross-app authentication within the VeChain ecosystem
* More Options - Additional Privy-supported login methods (only available for self hosted Privy)

### Login Modal Customization

The modal implements a dynamic grid layout system that can be customized through the `loginMethods` configuration.

The modal can be configured through the `VeChainKitProvider` props.

```typescript
<VeChainKitProvider
    loginModalUI={{
        logo: '/your-logo.png',
        description: 'Custom login description',
    }}
    loginMethods={[
        { method: 'vechain', gridColumn: 4 },
        { method: 'email', gridColumn: 2 },
        { method: 'passkey', gridColumn: 2 },
    ]}
    allowCustomTokens={false} // allow the user to manage custom tokens
>
    {children}
</VeChainKitProvider>
```

* vechain, dappkit, and ecosystem are always valid options
* The Privy-dependent methods (email, google, passkey, more) are only available when the privy prop is defined
* TypeScript will show an error if someone tries to use a Privy-dependent method when privy is not configured

```typescript
// This will show a type error
const invalidConfig: VechainKitProviderProps = {
    // no privy prop specified
    loginMethods: [
        { method: 'email' }  // ❌ Type error: 'email' is not assignable to type 'never'
    ],
    // ... other required props
};

// This is valid
const validConfig1: VechainKitProviderProps = {
    // no privy prop
    loginMethods: [
        { method: 'vechain' },  // ✅ OK
        { method: 'dappkit' },  // ✅ OK
        { method: 'ecosystem' } // ✅ OK
    ],
    // ... other required props
};

// This is also valid
const validConfig2: VechainKitProviderProps = {
    privy: {
        appId: 'xxx',
        clientId: 'yyy',
        // ... other privy props
    },
    loginMethods: [
        { method: 'email' },     // ✅ OK because privy is configured
        { method: 'google' },    // ✅ OK because privy is configured
        { method: 'vechain' },   // ✅ OK (always allowed)
        { method: 'ecosystem' }  // ✅ OK (always allowed)
    ],
    // ... other required props
};
```

#### **Ecosystem button**

The ways to show the ecosystem login button are:

1. You define "ecosystem" in the loginMethods in the config
2. You do not define the loginMethods in the config, so we default to showing the ecosystem login button

To not show the ecosystem login button, you must explicitly define the loginMethods array in the config and not include ecosystem in the options.

By default we have a list of default apps that will be shown as ecosystem login options. If you want to customize this list you can pass the `allowedApps` array prop. You can find the app ids in the [Ecosystem](https://dashboard.privy.io/) tab in the Privy dashboard.

## 3) Setup Fee Delegation (mandatory)

Fee delegation is **mandatory** in order to use this kit. Learn how to setup fee delegation in the following guide:

{% content-ref url="fee-delegation.md" %}
[fee-delegation.md](fee-delegation.md)
{% endcontent-ref %}

## 4) Setup Privy (optional)

If you already use Privy you can pass an additional prop with you settings and you will be able to access Privy SDK, customizing the login modal based on your needs.

Pros of self hosting Privy:

* No UI confirmations on users transactions
* Allow your users to backup their keys and update security settings directly in your app
* Targetted social login methods

Cons:

* Price
* Responsibilities to correctly secure your Privy account, since it contains access to user's wallet settings
* Your users will need to login into other apps through ecosystem mode

To setup Privy you need to add the following parameters:

```javascript
import { VechainKitProvider } from '@vechain/vechain-kit';

export default function App({ Component, pageProps }: AppProps) {
    return (
        <VechainKitProvider
            privy={{
                appId: process.env.NEXT_PUBLIC_PRIVY_APP_ID!,
                clientId: process.env.NEXT_PUBLIC_PRIVY_CLIENT_ID!,
                loginMethods: ['google', 'twitter', 'sms', 'email'],
                appearance: {
                    walletList: ['metamask', 'rainbow'],
                    accentColor: '#696FFD',
                    loginMessage: 'Select a social media profile',
                    logo: 'https://i.ibb.co/ZHGmq3y/image-21.png',
                },
                embeddedWallets: {
                    createOnLogin: 'all-users',
                },
                allowPasskeyLinking: true,
            }}
            ...
            //other props
        >
            {children}
        </VechainKitProvider>
    );
}
```

Go to[ privy.io](https://privy.io) and create an app. You will find the APP ID and the CLIENT ID in the **App Settings** tab.

For further information on how to implement Privy SDK please refer to their docs: [https://docs.privy.io/](https://docs.privy.io/)

This project uses:

* [_@privy-io/react-auth_](https://www.npmjs.com/package/@privy-io/react-auth) for Privy connection type
* [_@privy-io/cross-app-connect_](https://www.npmjs.com/package/@privy-io/cross-app-connect) for ecosystem cross app connection

You can import privy from the kit as&#x20;

```typescript
import { usePrivy } from "@vechain/vechain-kit";

const { user } = usePrivy();
```

## 5) Use the kit

Once you setup the kit provider and created your fee delegation service you are good to go and you can allow your users to login.

### Wallet Button

You can use this component by importing it from the kit, it will handle for you the connection state and show a login button if the user is disconnected or the profile button when the user is connected.

```typescript
'use client';

import { WalletButton } from '@vechain/vechain-kit';

export function Page() {
    return (
        <WalletButton />
    );
}
```

Read more [here](quickstart.md#wallet-button) on how to customize this button here.

### Custom button

Alternatively, you can create your own custom button and invoke the connect modal or account modal based on your needs.

```typescript
'use client';

import { useConnectModal, useAccountModal, useWallet } from '@vechain/vechain-kit';

export function Page() {
    const { connection } = useWallet();
    
    const { 
        open: openConnectModal, 
        close: closeConnectModal, 
        isOpen: isConnectModalOpen 
    } = useConnectModal();
    
     const { 
        open: openAccountModal, 
        close: openAccountModal, 
        isOpen: isAccountModalOpen 
    } = useAccountModal();
    
    if (!connection.isConnected) {
        return (
            <button onClick={openConnectModal}> Connect </button>
        );
    }
    
    return (
        <button onClick={openAccountModal}> View Account </button>
    );
}
```

## Support for devs

Are you having issues using the kit? Join our discord server to receive support from our devs or open an issue on our Github!

Discord: [https://discord.gg/wGkQnPpRVq](https://discord.gg/wGkQnPpRVq)

Github: [https://github.com/vechain/vechain-kit/issues](https://github.com/vechain/vechain-kit/issues)
