# Smart Account

### Hooks

The hooks offer tools for efficient smart account management:

* **`useGetAccountAddress`**: Retrieves the smart account's address linked to the wallet.
* **`useGetAccountVersion`**: Retrieces the smart account's version even if the account is not deployed yet
* **`useSmartAccountVersion`**: Retrieves the smart account's version, but only if it is already deployed, and it will revert for v1 smart accounts (because function was not implemented in that smart contract)
* **`useSmartAccount`**: Retrieves the the account of an owner, returning additional information
* **`useAccountImplementationAddress`**: Returns the implementation address of a specific version; this hook is useful when upgrading the smart account of the user
* **`useCurrentAccountImplementationVersion`**: Returns the latest (and currently used) implementation version of the smart accounts used by the factory
* **`useUpgradeRequired`**: Returns if a smart account needs an upgrade (even if it's not yet deployed)
* **`useUpgradeRequiredForAccount`**: As above but only if the account is not yet deployed
* **`useIsSmartAccountDeployed`**: Verifies the deployment status of the smart account.
* **`useHasV1SmartAccount`**: Checks for a legacy version's existence.
* **`useUpgradeSmartAccount`**: Hook to create a transaction to upgrade the smart account to a specific version

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
} from '@vechain/vechain-kit';

// connectedWallet is the owner of the smart account, can be the Privy embedded wallet or VeWorld
const { connectedWallet } = useWallet();

const { data: smartAccountAddress } = useGetSmartAccountAddress(
    connectedWallet?.address,
);

const { data: upgradeRequired } = useUpgradeRequired(
    smartAccountAddress,
    ownerAddress: connectedWallet?.address ?? "",
    version: 3
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
} = useUpgradeSmartAccount({
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
    'upgradeRequired',
    upgradeRequired,
    'current implementation version',
    smartAccountImplementationVersion,
    'v3 implementation address',
    smartAccountImplementationAddress,
);
```
