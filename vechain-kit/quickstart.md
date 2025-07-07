# Quickstart

## 1) Install package

{% code overflow="wrap" %}
```sh
yarn add @tanstack/react-query@"^5.64.2" @chakra-ui/react@"^2.8.2" @vechain/dapp-kit-react@"2.1.0-rc.1" @vechain/vechain-kit

// or

npm i @tanstack/react-query@"^5.64.2" @chakra-ui/react@"^2.8.2" @vechain/dapp-kit-react@"2.1.0-rc.1" @vechain/vechain-kit
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

import {VeChainKitProvider} from "@vechain/vechain-kit";

export function VeChainKitProviderWrapper({children}: any) {
  return (
    <VeChainKitProvider
      feeDelegation={{
        delegatorUrl: "https://sponsor-testnet.vechain.energy/by/441",
        // set to false if you want to delegate ONLY social login transactions
        // social login transactions sponsorship is currently mandatory
        delegateAllTransactions: false,
      }}
      loginMethods={[
        {method: "vechain", gridColumn: 4},
        {method: "dappkit", gridColumn: 4},
      ]}
      dappKit={{
        allowedWallets: ["veworld", "wallet-connect", "sync2"],
        walletConnectOptions: {
          projectId:
            // Get this on https://cloud.reown.com/sign-in
            process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID!,
          metadata: {
            name: "React Dapp Template",
            description: "This is the description of your app visible in VeWorld upon connection request.",
            url: typeof window !== "undefined" ? window.location.origin : "",
            icons: ["https://path-to-logo.png"],
          },
        },
      }}
      darkMode={false}
      language="en"
      network={{
        type: "test",
      }}
    >
      {children}
    </VeChainKitProvider>
  );
}

```

On Next.js you will need to dynamically load the import

```typescript
import dynamic from 'next/dynamic';

const VeChainKitProvider = dynamic(
    async () =>
        (await import('@vechain/vechain-kit')).VeChainKitProvider,
    {
        ssr: false,
    },
);
```

### Login Methods

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
        { method: 'dappkit', gridColumn: 4 }, // VeChain wallets, always available
        { method: 'ecosystem', gridColumn: 4 }, // Mugshot, Cleanify, Greencart, ...
        { method: 'email', gridColumn: 2 }, // only available with your own Privy
        { method: 'passkey', gridColumn: 2 },  // only available with your own Privy
        { method: 'google', gridColumn: 4 }, // only available with your own Privy
        { method: 'more', gridColumn: 2 }, // will open your own Privy login, only available with your own Privy
        
    ]}
    allowCustomTokens={false} // allow the user to manage custom tokens
>
    {children}
</VeChainKitProvider>
```

{% hint style="warning" %}
Login methods selection:

* vechain, dappkit, and ecosystem are always available options
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
{% endhint %}

{% hint style="info" %}
By default we have a list of default apps that will be shown as ecosystem login options. If you want to customize this list you can pass the `allowedApps` array prop. You can find the app ids in the [Ecosystem](https://dashboard.privy.io/) tab in the Privy dashboard.
{% endhint %}

## 3) Setup Fee Delegation (mandatory if allowing social login)

Fee delegation is **mandatory** if you want to use this kit with social login. Learn how to setup fee delegation in the following guide:

{% content-ref url="../social-login/fee-delegation.md" %}
[fee-delegation.md](../social-login/fee-delegation.md)
{% endcontent-ref %}

## 4) Setup Privy (optional)

If you have your own Privy app, you can pass an additional prop with your settings.

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

{% hint style="warning" %}
If you setup your own Privy be sure to go over the recommended security settings provided by Privy: \
[https://docs.privy.io/guide/security/implementation/](https://docs.privy.io/guide/security/implementation/) and [https://docs.privy.io/guide/security/implementation/csp](https://docs.privy.io/guide/security/implementation/csp)
{% endhint %}

{% hint style="info" %}
**Pros of self hosting Privy:**

* No UI confirmations on users transactions
* Allow your users to backup their keys and update security settings directly in your app
* Targetted social login methods

**Cons:**

* Price
* Responsibilities to correctly secure your Privy account, since it contains access to user's wallet settings
* Your users will need to login into other apps through ecosystem mode
{% endhint %}

## 5) Setup Legal Documents (optional)

To prompt users to review and accept your policies, like **Terms and Conditions**, **Privacy Policy**, or **Cookie Policy,** VeChainKit offers a simple plug-and-play solution.

You can avoid building your own if you haven't already.

By enabling also the tracking consent, you will allow VeChainKit to prompt your users to collect data to improve the kit.

***

When the `legalDocuments` option is configured, the users will see:

* **Left:** A **modal prompt** when connecting their wallet, requiring them to review and accept required and optional legal documents.
* **Right:** A **summary view** under `Settings > General > Terms and Policies`, showing which policies they’ve accepted and when.

<div align="center" data-full-width="true"><figure><img src="../.gitbook/assets/kit-legal-docs-modal (1).png" alt=""><figcaption><p>Legal Docs Modal</p></figcaption></figure> <figure><img src="../.gitbook/assets/kit-legal-docs-review-modal (1).png" alt=""><figcaption><p>Legal Docs Summary View</p></figcaption></figure></div>

***

{% hint style="info" %}
**Important**

Legal document agreements are tied to the **wallet address**, **document type**, **document version**, and the **url**.

* If the `version` of any document is updated, users will be prompted to accept it again.
* Agreements are stored in the browser’s **local storage**, meaning acceptance is **per browser and device**.
* As a result, users may be prompted again if they switch browsers, devices, or clear their local storage  even if they've previously agreed on another setup.
{% endhint %}

```typescript
import { VechainKitProvider } from '@vechain/vechain-kit';

export default function App({ Component, pageProps }: AppProps) {
    return (
        <VechainKitProvider
            legalDocuments={{
                allowAnalytics: true, // Enables optional consent for VeChainKit tracking

                cookiePolicy: [
                    {
                        displayName: 'MyApp Policy', // (Optional) Custom display label
                        url: 'https://www.myapp.com/cookie-policy',
                        version: 1, // Increment to re-prompt users
                        required: false, // Optional: User sees a checkbox to opt in
                    },
                ],

                privacyPolicy: [
                    {
                        url: 'https://www.myapp.com/privacy-policy',
                        version: 1, // Increment to re-prompt users
                        required: false, // Optional: can be skipped or rejected
                    },
                ],

                termsAndConditions: [
                    {
                        displayName: 'MyApp T&C',
                        url: 'https://www.myapp.com/terms-and-conditions',
                        version: 1, // Increment to re-prompt users
                        required: true, // Required: must be accepted to proceed
                    },
                ],
            }}
            // ... other props
        >
            {children}
        </VechainKitProvider>
    );
}

```

**Key Options**

<table><thead><tr><th width="201.77734375">Option</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>allowAnalytics</code></td><td><code>boolean</code></td><td>No</td><td>If <code>true</code>, prompts users with an optional tracking policy.</td></tr><tr><td><code>cookiePolicy</code></td><td><code>array</code></td><td>No</td><td>One or more cookie policy versions.</td></tr><tr><td><code>privacyPolicy</code></td><td><code>array</code></td><td>No</td><td>One or more privacy policy versions.</td></tr><tr><td><code>termsAndConditions</code></td><td><code>array</code></td><td>No</td><td>One or more T&#x26;C versions.</td></tr></tbody></table>

## 6) Show the login button

Once you set up the kit provider, you are good to go, and you can allow your users to login, customizing the login experience based on your needs. You can choose between many options, leaving you complete freedom on your design.

{% tabs %}
{% tab title="1) Use WalletButton component" %}
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

Read more [here](quickstart.md#wallet-button) on how to customize this button.
{% endtab %}

{% tab title="2) Create a custom button" %}
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
{% endtab %}

{% tab title="3) Call targeted login methods" %}
Only available for apps with self hosted Privy .

This is an example of doing login with Google custom button, for more in depth details read [here](hooks/login.md).

```typescript
// Example usage of Login hooks
import { 
    useLoginWithOAuth,
} from '@vechain/vechain-kit';

const ExampleComponent = () => {
    // OAuth authentication
    const {
        initOAuth,
    } = useLoginWithOAuth();

    const handleOAuthLogin = async (provider: OAuthProvider) => {
        try {
            await initOAuth({ provider });
            console.log(`${provider} OAuth login initiated`);
        } catch (error) {
            console.error("OAuth login failed:", error);
        }
    };

    return (
        <div>
            {/* OAuth Login Options */}
            <button onClick={() => handleOAuthLogin('google')}>
                Login with Google
            </button>
            <button onClick={() => handleOAuthLogin('twitter')}>
                Login with Twitter
            </button>
            <button onClick={() => handleOAuthLogin('apple')}>
                Login with Apple
            </button>
            <button onClick={() => handleOAuthLogin('discord')}>
                Login with Discord
            </button>
        </div>
    );
};

export default ExampleComponent;
```
{% endtab %}

{% tab title="4) Allow only DAppKit" %}
You can allow users connect to your app only with wallet by using the dapp-kit connect modal, as follows:

```typescript
import { useDAppKitWalletModal } from '@vechain/vechain-kit';

export const LoginComponent = () => {
  const { open: openWalletModal } = useDAppKitWalletModal();

  return (
    <Button onClick={openWalletModal}>
        Open only "Connect Wallet"
    </Button>
)}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
When your app is opened inside VeWorld mobile wallet, VeWorld is always enforced as a login choice.
{% endhint %}

## Support for devs

Are you having issues using the kit? Join our discord server to receive support from our devs or open an issue on our Github!

Check our [Troubleshooting section](troubleshooting/).

Contact us on Discord: [https://discord.gg/wGkQnPpRVq](https://discord.gg/wGkQnPpRVq)

Open an issue on Github: [https://github.com/vechain/vechain-kit/issues](https://github.com/vechain/vechain-kit/issues)
