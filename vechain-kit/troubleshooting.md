# Troubleshooting

When installing the kit or migrating from DApp Kit, you may encounter some issues during the transition. We are committed to making this process as seamless as possible and appreciate your patience. To assist you, we've compiled a list of common problems and solutions below. If your issue is not addressed here, please reach out to us on our Discord channel or open an issue ticket on our GitHub repository for further assistance.

Discord: [https://discord.gg/wGkQnPpRVq](https://discord.gg/wGkQnPpRVq)

Github: [https://github.com/vechain/vechain-kit/issues](https://github.com/vechain/vechain-kit/issues)

## Known issues

### Peer dependencies

Ensure that your project's peer dependencies align with the specifications outlined by the kit. This might require updating or downgrading certain packages to maintain compatibility. In some cases, these issues can also be resolved by cleaning your project's build artifacts or deleting your `node_modules` directory and reinstalling your packages. If you still face challenges, consider reaching out to the community support forums or reviewing the kit's official documentation for further guidance.

{% hint style="info" %}
Coming from DApp Kit or SDK you could have issues with different versions of them installed across your project: **completly remove dapp-kit packages, if you need to use something from dapp-kit you can import it from vechain-kit as well.**
{% endhint %}

### Chakra styling

The VeChain Kit components are each wrapped in their own Chakra Provider (v2), ensuring consistent styling throughout the modal. Remember to utilize Chakra's theme options for styling your app. Be aware that conflicts may arise if you also use Chakra in your app.

Even if you do not use Chakra in your app it could still be necessary to install it as a peer dependency and add to the list of your providers. You can keep using whatever frontend framework you wish, the important is to define the provider.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>One common issue, solvable by installing Chakra and defining &#x3C;ColorModeScript /></p></figcaption></figure>

#### Chakra installation

```sh
yarn add @chakra-ui/react@2.8.2
```

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";
import { VeChainKitProvider } from "@vechain/vechain-kit";
import {
  ChakraProvider,
  ColorModeScript,
  useColorMode,
} from "@chakra-ui/react";
import { PersistQueryClientProvider } from "@tanstack/react-query-persist-client";
import { persister, queryClient } from "./utils/queryClient.ts";

export const AppWrapper = () => {
  const { colorMode } = useColorMode();

  return (
    <VeChainKitProvider
      ...
    >
      <App />
    </VeChainKitProvider>
  );
};

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <PersistQueryClientProvider
      client={queryClient}
      persistOptions={{ persister }}
    >
      <ChakraProvider theme={stargateStakingTheme}>
        <ColorModeScript initialColorMode="dark" />
        <AppWrapper />
      </ChakraProvider>
    </PersistQueryClientProvider>
  </React.StrictMode>
);

```

{% hint style="success" %}
The important part:

```typescript
<ChakraProvider theme={stargateStakingTheme}>
    <ColorModeScript initialColorMode="dark" />
    <AppWrapper />
</ChakraProvider>
```
{% endhint %}

### Fee Delegation

If you were already using fee delegation in your app you should remove that and use the one handled by the kit. You just need to provide the FEE\_DELEGATION\_URL in the provider.

If you want to delegate all transactions, also for veworld users, you need to set `delegateAllTransactions` to `true`.

{% hint style="danger" %}
If you do not remove your own delegation then transactions could fail because your transaction will delegate 2 times, will have 2 signatures and an incorrect format.
{% endhint %}
