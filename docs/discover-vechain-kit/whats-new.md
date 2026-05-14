---
description: >-
  This update brings a refreshed interface, better performance, more
  flexibility, and an improved developer experience. It also transitions from
  Connex to the SDK, with V1 now deprecated.
---

# What's new?

### 🔑 Connect Modal v2.7 — custom UI for VeWorld / Sync2

The connect modal now owns the **entire VeWorld and Sync2 connection flow** end-to-end. No more hand-off to dapp-kit's native picker — clicking a wallet button drives `@vechain/dapp-kit` programmatically and renders the kit's own "Waiting for signature…" view. WalletConnect's QR modal is preserved.

Highlights:

* New granular `loginMethods` entries: `veworld`, `sync2`, `wallet-connect`, `apple`.
* New variation A layout — VeWorld primary filled (recommended), Google / Apple outline secondary, and a "More options ⌄" link footer that opens an in-modal sub-view with overflow wallets / socials / ecosystem apps.
* Themeable accent — `theme.accent` drives the spinner, focus rings and the "Waiting for signature…" headline.
* Zero breaking changes: pin `{ method: 'dappkit' }` in `loginMethods` to keep the legacy flow.

See [Login Customization](../quickstart/login-customization.md) for the full guide.

### ⚡ Faster Development

* We’ve introduced several optimizations that drastically reduce bundle size and speed up development builds.
* To support modular workflows, we’ve also released standalone packages like [**`@vechain/vechain-contract-types`**](https://www.npmjs.com/package/@vechain/vechain-contract-types) and [**`@vechain/contract-getters`**](https://www.npmjs.com/package/@vechain/contract-getters), which you can use independently.
* More improvements are coming soon.

### 🎨 More Customization

You now have much more control over the **vechain-kit modal**:

* Open specific flows in isolation (e.g., only _Receive_ or _Send_, without exposing the rest of the modal).
* Customize colors and fonts to match your brand.
* Use Bottomsheets instead of Modals on mobile!

Check the [Customization](../customization/theming.md) section for all available options.

### **✨ Redesigned UI**

In version 2, we have completely overhauled the user interface to simplify navigation and enhance user experience. The new design focuses on clarity and usability, placing a stronger emphasis on wallet features to streamline user interactions. This redesign aims to provide an intuitive and efficient workflow, allowing users to access essential functionalities effortlessly.

### 🔑 Quick Wallet Switching

Easily switch between wallets without the need to log out and log back in, enhancing the fluidity of your transactions and interactions.

### 🔄 Built-In Token Swap

Swap tokens directly inside the kit—no need to send users to external websites.

Powered by **BetterSwap** and **VeTrade**, swaps are now:

* Seamless
* Secure
* Efficient

Users can manage assets more conveniently than ever.

### 🆓 Smarter Fee Delegation

No more configuring delegation services manually.

V2 includes:

* **Automatic transaction sponsorship** for social-login users (via Generic Delegator)
* An improved `useSendTransaction()` hook that lets you sponsor specific transactions selectively

Even if you’re not required to cover fees, you might still sponsor some transactions—for example, onboarding new users or showing fee costs to social-login accounts. Head over to the [fee-delegation.md](../social-login/fee-delegation.md "mention") section to learn more about this.

### **🛠️ Easier Installation**

A cleaner, more streamlined setup minimizes the time spent troubleshooting.

### **👋 Goodbye Connex, Hello SDK**

Connex is deprecated.\
V2 now uses the **SDK**, offering more developer capabilities and a more modern foundation.

### **📱 Better VeWorld Mobile Integration**

We now use the new VeWorld endpoints, delivering:

* Smoother logins
* Easier wallet switching

### **📘 Improved Documentation**

Clearer, more complete documentation helps you transition and integrate features with confidence.

### **🚀 A Big Roadmap Ahead**

Coming soon (and exclusive to V2):

* Revamped login flow
* Better cross-app connections
* Transaction history
* NFTs
* DeFi integrations
* And more

{% hint style="danger" %}
**Breaking Changes**

V2 may have some breaking changes based on your V1 integration. Please read more in details what changed from V1 and how to migrate to version 2 in [the following section](https://github.com/vechain/vechain-kit/blob/feat/docs/docs/migrations/upgrade-vechain-kit-from-1.x-to-2.x).
{% endhint %}

{% hint style="warning" %}
**Version 1 Deprecation**\
Support for version 1 will cease, and updates will target version 2, encouraging users to migrate to leverage new features.
{% endhint %}
