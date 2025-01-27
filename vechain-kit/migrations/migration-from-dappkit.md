# Migration from DAppKit

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

## Troubleshooting

### Peer dependencies

Coming from DApp Kit or SDK you could have issues with different versions of them installed accross your project.

Same goes for Chakra (v2) and React Query, be sure to have the proper versions

### Chakra styling

You could have conflicts with styling if you use Chakra also in your app.&#x20;

VeChain Kit components are wrapped in their own Chakra Provider ensuring a consistent style accross the modal. Be sure to style you app with Chakra's theme options.

