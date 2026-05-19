# Connection Types

VeChain Kit supports 3 types of connections:

## 1) Privy

This connection type is often used by organizations like VeBetterDAO, Cleanify, and Greencart. When connected, users can back up their embedded wallets, sign transactions without confirmation prompts, and add login methods. By connecting with Privy, developers use their personal APP\_ID and CLIENT\_ID to create their own app on Privy.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

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

## 2) Privy Cross App (VeChain whitelabel)

This is the default connection type when your app **does not** pass a `privy` prop to `VeChainKitProvider`. Social logins (Google, Apple, X, Discord, GitHub, TikTok, LINE, and the multi-provider "Continue with VeChain" picker) run through VeChain's whitelabel popup at [`connect.vechain.org`](https://connect.vechain.org). The popup handles the OAuth handshake, decodes the transaction in plain language for the user, and posts the signed result back to your dApp.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Why this is the default in v2.7+:

* **Free social login** — no Privy account, no API keys, no billing. Drop a `{ method: 'google' }` button and it works.
* **One identity across the ecosystem** — the user's VeChain address is the same in every kit-integrated dApp, so balances, NFTs and domains follow them.
* **VeChain-branded** — the popup is whitelabeled (logo, palette, dark/light, 17 languages matching the kit). No Privy chrome leaks through.
* **Smart-contract-aware transaction review** — instead of raw hex, the user sees "Send 10 B3TR to vechain.vet" or "Vote FOR on a VeBetterDAO proposal" with verified-contract chips.
* **Auto-recovery from stale sessions** — if the connection record expires, the kit logs the user out and re-opens the login modal automatically.

Use this option unless you have a specific reason to self-host Privy (see option 1).

## 3) Self-custody wallets

This connection type allows login with self custody wallets, and  is using the dapp-kit package under the hood.&#x20;

The available wallets are: VeWorld mobile, VeWorld extension, Sync2, and Wallet Connect for VeWorld mobile.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Other  wallets are available when login in with VeChain, such as Metamask, Rabby, Phantom, Coinbase Wallet, and Ranibow. \
That will use Privy under the hood though, which means the connection type will be "privy-cross-app".
{% endhint %}

{% hint style="success" %}
If you want to use vechain-kit but do not care about social login then you can skip the first login modal and directly show the "Connect Wallet" modal like this:

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
{% endhint %}

{% hint style="warning" %}
When your app is opened inside VeWorld mobile wallet, VeWorld is always enforced as a login choice.
{% endhint %}
