# Smart Account

### Hooks

The hooks offer tools for efficient smart account management:

* **`useGetSmartAccountAddress`**: Retrieves the smart account's address linked to the wallet.
* **`useIsSmartAccountDeployed`**: Verifies the deployment status of the smart account.
* **`useSmartAccountVersion`**: Obtains the current version of the smart account.
* **`useHasV1SmartAccount`**: Checks for a legacy version's existence.
* **`useSmartAccountNeedsUpgrade`**: Identifies if an upgrade is necessary.
* **`useSmartAccountImplementationAddress`**: Fetches target version's implementation details.
* **`useUpgradeSmartAccountVersion`**: Manages the upgrade process for a

### Usage

```typescript
import {
    useSmartAccountVersion,
    useGetSmartAccountAddress,
    useIsSmartAccountDeployed,
    useSmartAccountNeedsUpgrade,
    useWallet,
    useSmartAccountImplementationAddress,
    useUpgradeSmartAccountVersion,
    useHasV1SmartAccount,
} from '@/hooks';

// connectedWallet is the owner of the smart account, can be the Privy embedded wallet or VeWorld
const { connectedWallet } = useWallet();

const { data: smartAccountAddress } = useGetSmartAccountAddress(
    connectedWallet?.address,
);

const { data: hasV1SmartAccount } = useHasV1SmartAccount(
    connectedWallet?.address,
);

const { data: smartAccountDeployed } =
    useIsSmartAccountDeployed(smartAccountAddress);

const { data: currentSmartAccountVersion } =
    useSmartAccountVersion(smartAccountAddress);

const { data: smartAccountNeedsUpgrade } = useSmartAccountNeedsUpgrade(
    smartAccountAddress,
    3,
);

const { data: smartAccountImplementationVersion } =
    useCurrentAccountImplementationVersion();

const { data: smartAccountImplementationAddress } =
    useSmartAccountImplementationAddress(3);

// Use the new hook for upgrading
const {
    sendTransaction: upgradeSmartAccount,
    isTransactionPending,
    error: upgradeError,
} = useUpgradeSmartAccountVersion({
    fromAddress: connectedWallet?.address ?? '',
    smartAccountAddress: smartAccountAddress ?? '',
    targetVersion: 3,
    onSuccess: () => {
        console.log('Smart Account upgraded successfully!');
    },
    onError: () => {
        console.log('Error upgrading Smart Account.');
    },
});

console.log(
    'Smart Account',
    smartAccountAddress,
    'deployed',
    smartAccountDeployed,
    'version',
    currentSmartAccountVersion,
    'has v1 smart account',
    hasV1SmartAccount,
    'needs upgrade',
    smartAccountNeedsUpgrade,
    'current implementation version',
    smartAccountImplementationVersion,
    'v3 implementation address',
    smartAccountImplementationAddress,
);
```
