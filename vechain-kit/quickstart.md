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

### 1) Install package

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

### 2) Define Provider

```typescript
'use client';

import VeChainKitProvider from '@vechain/vechain-kit'

export function VeChainKitProviderWrapper({ children }: Props) {
    return (
         <VechainKitProvider
            // Mandatory
            feeDelegation={{
                delegatorUrl: process.env.NEXT_PUBLIC_DELEGATOR_URL!
            }}
            dappKit={{
                allowedWallets: ['veworld', 'sync2'],
            }}
            privyEcosystemApps=[] // remove for using default ones
            loginModalUI={{
                variant: 'vechain-wallet-ecosystem',
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
