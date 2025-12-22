# Should I use it?

VeChain provides a robust set of technologies to facilitate app development, including the VeChain Kit, DApp Kit, and SDKs. This section will guide you through these resources, helping you make informed decisions to choose the most suitable tool for your application's needs.

### VeChain [SDK](https://docs.vechain.org/developer-resources/sdks-and-providers/sdk)

* **Blockchain Integration:** Seamlessly integrate VeChain blockchain capabilities into your applications.
* **Flexible Environments:** Work in your preferred programming environment with ease.
* **Backend Focus:** Optimized for backend development and scripting tasks.
* **Custom Wallet Connections:** Establish wallet connections from scratch for full control and customization of transactions.

The SDK is ideal for developers seeking to harness the power of VeChainThor in their backend architecture or script-based solutions.

### [DApp Kit](https://docs.vechain.org/developer-resources/sdks-and-providers/dapp-kit)

* **Lightweight and Focused:** The DApp Kit is designed to be a lightweight library, handling only essential blockchain connections.
* Only allows connection to wallets
* Does not provide hooks for sending transactions, SDK must be used for that

DApp Kit is ideal to who wants a lightweight package, that handles only the essential.

### VeChain Kit

* It uses both dappkit and sdk, so you have all the functionalities of above
* You have social login
* You have out of the box components, hooks, and functionalities

## What to use?

### Do you care about social login?

If yes, you need to go with VeChain Kit, or build social login on top of your implementation with DAppKit or SDK.

### Do you care about users accessing the wallet modal?

If you want to allow your users to swap, send tokens, view balances, switch account out of the box then you need to use VeChain Kit.

### Do you care about pre-built hooks for sending transactions, signing messages, reading data, retrive avatars, etc.?

Then you can use the VeChain Kit, it has all the hooks you may need, and we will add more.

