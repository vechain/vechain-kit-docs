# Setup Privy (optional)

{% hint style="success" %}
**You do not need a Privy account to ship social login.** By default, VeChain Kit routes Google, Apple, X, Discord, GitHub, TikTok, LINE, and the multi-provider "Continue with VeChain" button through VeChain's whitelabel cross-app popup at [`connect.vechain.org`](https://connect.vechain.org) — branded for VeChain, free, and the user keeps a single identity across every kit-integrated dApp.

Self-host Privy only if you need email / passkey / SMS login, additional OAuth providers, no popup, or branding-inside-your-dApp transaction prompts. See _Pros / Cons_ at the bottom of this page.
{% endhint %}

If you have your own Privy app, you can pass an additional prop with your settings.

```javascript
import { VeChainKitProvider } from '@vechain/vechain-kit';

export default function App({ Component, pageProps }: AppProps) {
    return (
        <VeChainKitProvider
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
        </VeChainKitProvider>
    );
}
```

Go to[ privy.io](https://privy.io) and create an app. You will find the APP ID and the CLIENT ID in the **App Settings** tab.

For further information on how to implement Privy SDK please refer to their docs: [https://docs.privy.io/](https://docs.privy.io/)

This project uses:

* [_@privy-io/react-auth_](https://www.npmjs.com/package/@privy-io/react-auth) for Privy connection type
* [_@privy-io/cross-app-connect_](https://www.npmjs.com/package/@privy-io/cross-app-connect) for ecosystem cross app connection

You can import privy from the kit as

```typescript
import { usePrivy } from "@vechain/vechain-kit";

const { user } = usePrivy();
```

{% hint style="warning" %}
If you setup your own Privy be sure to go over the recommended security settings provided by Privy:\
[https://docs.privy.io/guide/security/implementation/](https://docs.privy.io/guide/security/implementation/) and [https://docs.privy.io/guide/security/implementation/csp](https://docs.privy.io/guide/security/implementation/csp)
{% endhint %}

{% hint style="info" %}
**Pros of self hosting Privy:**

* No UI confirmations on user transactions (no popup window per signature)
* Allow your users to back up their keys and update security settings directly in your app
* Email / passkey / SMS login (these can't run via the whitelabel popup)
* Additional OAuth providers Privy supports but VeChain's whitelabel doesn't enable (LinkedIn, Spotify, …)
* Your dApp's branding throughout the login modal

**Cons:**

* Price (Privy pricing tiers)
* Responsibility to correctly secure your Privy account, since it contains access to users' wallet settings
* Users have their own wallet scoped to your dApp instead of a single VeChain-wide identity (cross-app linking still possible via ecosystem mode)
{% endhint %}
