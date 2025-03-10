# Connection Types

VeChain Kit supports 3 types of connections:

## 1) Privy

Connecting with Privy means that the developer has created his own app on Privy and is using his personal APP\_ID and CLIENT\_ID.&#x20;

When connected this way the user can backup his embedded wallet, sign transactions without confirmation modals, add login methods, etc.

This type of connection is typically used by: VeBetterDAO, Cleanify, Greencart, etc.

## 2) Privy Cross App

Privy embedded wallets can be made interoperable across apps. In this setup, embedded wallets foster a **cross-app ecosystem** where users can easily port their embedded wallets from one app to another.

Using **cross-app wallets**, users can seamlessly move assets between different apps and can easily prove ownership of, sign messages, or send transactions with their existing embedded wallets.

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

With this type of connection you can have social login without actually create an app on Privy, and you can also allow logins from apps like Cleanify, EVEarn, Mugshot, Greencart, etc.&#x20;

This connection type is also crucial for allowing users to "Login with VeChain".

Typically, all apps that do not have their own Privy will use this connection type.

## 3) Wallet (VeWorld, Sync2, Wallet Connect)

The last type of connection is for wallets, which is using dapp-kit under the hood.

You can allow users connect to your app only with wallet by using the dapp-kit connect modal, as follows:

```typescript
import { useDAppKitWalletModal, DAppKitWalletButton } from '@vechain/vechain-kit';

export const LoginComponent = () => {
  const { open: openWalletModal } = useDAppKitWalletModal();

  return (
    <Button onClick={openWalletModal}>
        Open only "Connect Wallet"
    </Button>
    
    // or

    <DAppKitWalletButton>
)}
```

{% hint style="warning" %}
When your app is opened inside VeWorld mobile wallet, VeWorld is always enforced as a login choice.
{% endhint %}
