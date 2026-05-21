---
description: How to install and set up VeChain Kit in your project
---

# Installation

### 🚀 Start with AI (recommended)

The fastest path: hand the prompt below to your coding agent (Claude Code, Cursor, or any agent). The agent will read the relevant [VeChain AI Skill](https://github.com/vechain/vechain-ai-skills) first, then scaffold or wire up the project for you.

{% tabs %}
{% tab title="Start a new dApp" %}
{% code title="Paste into your agent" overflow="wrap" %}
```text
Before doing anything, read these VeChain AI Skills so you follow current conventions:
- create-vechain-dapp: https://github.com/vechain/vechain-ai-skills/tree/main/skills/create-vechain-dapp
- vechain-kit: https://github.com/vechain/vechain-ai-skills/tree/main/skills/vechain-kit

Now the task:

Scaffold a new VeChain dApp for me using create-vechain-dapp, with:
- VeChain Kit pre-wired (Privy social login: Google + email, plus VeWorld and WalletConnect)
- Chakra UI v3 (with next-themes) and dark mode by default — follow the next-chakra-v3 example in the vechain-kit repo for wiring the kit's `theme` prop via `useChakraContext().token.var(...)` so theme tokens stay reactive
- A landing page that shows the connected user's address, B3TR balance, and a "Send B3TR" button
- A GitHub Pages deploy workflow ready to use

Name the project "my-vechain-dapp". When done, run `yarn dev` and tell me the URL.
```
{% endcode %}
{% endtab %}

{% tab title="Add to existing project" %}
{% code title="Paste into your agent" overflow="wrap" %}
```text
Before doing anything, read this VeChain AI Skill so you follow current conventions:
- vechain-kit: https://github.com/vechain/vechain-ai-skills/tree/main/skills/vechain-kit

Now the task:

I already have a Next.js app and I want to add VeChain Kit to it.

1. Install @vechain/vechain-kit and any required peer deps.
2. Find my root layout (app/layout.tsx for App Router or pages/_app.tsx for Pages Router) and wrap it with VeChainKitProvider.
3. Enable Privy social login (Google + email), VeWorld and WalletConnect.
4. Read Privy keys from NEXT_PUBLIC_PRIVY_* and the WalletConnect projectId from NEXT_PUBLIC_WC_PROJECT_ID.
5. Add a <WalletButton /> to my existing header.
6. Don't change my existing Chakra theme.

If you hit peer-dependency conflicts, stop and tell me before applying any fix.
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Install the skills once to give every future prompt automatic context: `npx skills add vechain/vechain-ai-skills`. Try them live at the [VeKit Playground](https://playground.vechainkit.vechain.org/ai-skills).
{% endhint %}

### Get the template

Prefer a one-shot scaffold without an agent? The CLI has you covered.

```bash
$ npx create-vechain-dapp@latest

? Select template ›
    ❯ VeChain Kit Next.js Template (Chakra, React Query, SDK)
```

Or install the kit into your existing package as shown below.

### Quick-start (manual)

{% stepper %}
{% step %}
#### Install `@vechain/vechain-kit` and peer dependencies

VeChain Kit builds on a few carefully chosen libraries to deliver a better overall experience, bringing powerful tools together while keeping the integration on your side as simple as possible.

{% code fullWidth="true" expandable="true" %}
```shellscript
npm install --legacy-peer-deps @vechain/vechain-kit @chakra-ui/react@^2.8.2 \
  @emotion/react@^11.14.0 \
  @emotion/styled@^11.14.0 \
  @tanstack/react-query@^5.64.2 \
  @vechain/dapp-kit-react@2.1.0-rc.1 \
  framer-motion@^11.15.0
```
{% endcode %}
{% endstep %}

{% step %}
#### Setup provider

Wrap your app with the `VechainKitProvider` at the root of your application.

This provider brings together VeWorld’s native VeChain integration (web3) and Privy’s social login wallet support (web2).

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
{% endstep %}

{% step %}
#### Enjoy!

With VeChain Kit’s snippets and primitive components, you can plug in wallet login and fetch key data much faster.

<pre class="language-typescript"><code class="lang-typescript"><strong>"use client";
</strong>import { WalletButton } from "@vechain/vechain-kit";


const Demo = () => {
  return (
    &#x3C;div>
       &#x3C;WalletButton /> {/* Login Button */}
       &#x3C;p>{account?.address}&#x3C;/p> {/* Address of the connected account */}
    &#x3C;/div>
  )
}
</code></pre>
{% endstep %}
{% endstepper %}

{% hint style="danger" %}
Only supported on React and Next.js
{% endhint %}

{% hint style="warning" %}
React query, chakra and dapp-kit are peer dependencies.
{% endhint %}
