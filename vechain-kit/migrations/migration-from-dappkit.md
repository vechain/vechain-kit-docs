# Migrate from DAppKit

1\) Install and overwrite the Provider.

2\) Search all imports of `@vechain/dapp-kit-react` `@vechain/dapp-kit` and `@vechain/dapp-kit-ui` with **`@vechain/vechain-kit.`**

3\) Wherever you use the `account` property from the `useWallet()` hook you need to access the user address differently:

```typescript
// dapp-kit
const { account } = useWallet()
console.log(account) // 0x000000dsadsa

// vechain-kit
const { account } = useWallet()
console.log(account.address, account.domain, account.image)
```

4\) Use the `useSendTransaction()` hook from `@vechain/vechain-kit` to send your transactions to the network. \
Read how to use the hook [here](../send-transactions.md).

6\) If you use useConnex() by importing from dapp-kit, import it from vechain-kit.

