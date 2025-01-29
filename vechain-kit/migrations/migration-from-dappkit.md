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
Read how to use the hook [here](../send-transactions.md).\
\


```typescriptreact
 const { open } = useWalletModal()
```

## Troubleshooting

### Peer dependencies

Coming from DApp Kit or SDK you could have issues with different versions of them installed accross your project.

Same goes for Chakra (v2) and React Query, be sure to have the proper versions

### Chakra styling

You could have conflicts with styling if you use Chakra also in your app.&#x20;

VeChain Kit components are wrapped in their own Chakra Provider ensuring a consistent style accross the modal. Be sure to style you app with Chakra's theme options.

### Fee Delegation

If you were already using fee delegation in your app you should remove that and use the one handled by the kit. You just need to provide the FEE\_DELEGATION\_URL in the provider.

If you want to delegate all transactions, also for veworld users, you need to also set `delegateAllTransactions` to `true`.

If you do not remove your own delegation then transactions could fail because your transaction will delegate 2 times, will have 2 signatures and an incorrect format.

