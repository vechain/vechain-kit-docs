# Smart Accounts

Our smart accounts are a simplified version of the [Account Abstraction pattern](https://github.com/eth-infinitism/account-abstraction) updated to simplify their implementation, increase developer experience and take advantage of VeChain unique features, such as fee delegation.

There are 2 contracts:

* **SimpleAccount**: Is the abstracted account of the user.
* **SimpleAccountFactory**: Factory contract to create SimpleAccount contracts on demand.

You can fork the contracts and deploy them on your own, but we recommend using the contracts deployed by us for a better cross-app compatibility.

Owner of the Simple Account can execute transactions by directly calling the `execute()` function on his Smart Account contract or by authorizing a transaction by signing it and allow a third party to broadcast the transaction by calling the `executeWithAuthorization()` function.

The contracts are deployed on the following networks:

* **Mainnet**: 0xC06Ad8573022e2BE416CA89DA47E8c592971679A
* **Testnet**: 0x7EABA81B4F3741Ac381af7e025f3B6e0428F05Fb

You can look at the code of the contracts in the[ Smart Accounts Factory](https://github.com/vechain/smart-accounts-factory) repository.

{% hint style="info" %}
Every wallet on VeChain can own a smart account. The address of your smart account is deterministic, and it can be deployed at any time, and receive tokens even if it is not deployed yet.
{% endhint %}

{% hint style="danger" %}
Currently VeChain Kit does not support the handling of a smart account by a VeWorld wallet.
{% endhint %}

