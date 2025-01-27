# Embedded Wallets

Whenever the user connects with Privy, either direct or cross-app, a wallet is created for the user and id secured by Privy.

Privy embedded wallets are the simplest way to have users transact onchain. Privy’s key management system uses key splitting (Shamir’s secret sharing) to ensure your users have full custody of their wallets without needing to manage their secret keys. Neither Privy nor your application ever sees the user's keys; secrets are only ever reconstituted on the user's device when signing messages or transactions.

Users can manage their embedded wallet seamlessly with their account; they never need to handle any unnecessary technical complexity. Your application can even pregenerate wallets for an account, like an email address or phone number, before the user logs in. Users can also export the key for their embedded wallet, providing an escape hatch to leave Privy at any time.

Your application can easily guide users to use their wallet; Privy comes with simple abstractions to prompt users to fund, transact, and sign with their wallet. The wallet UIs are highly-customizable, allowing your application to communicate relevant context to the user or even abstract away the fact that a wallet is being used under the hood.

As Privy is always cross chain, your application can provision wallets on Solana and all EVM compatible chains.

### Using wallets across apps[​](https://docs.privy.io/guide/embedded-wallets#using-wallets-across-apps) <a href="#using-wallets-across-apps" id="using-wallets-across-apps"></a>

Privy embedded wallets are portable and can be used by any app––even apps not using Privy. Privy’s cross application wallet system supports interoperability with simple user experiences accessible to everyone, and seamlessly integrates with other Privy integrations and wallet connector solutions like RainbowKit and wagmi.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>Sample cross app wallet flow</p></figcaption></figure>

Embedded wallets can be backed up by the user through the VeChain Kit modal in the Settings page.

Many login methods can be attached to the Embedded Wallet.
