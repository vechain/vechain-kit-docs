# VeBetterDAO

## VeBetterDAO Hooks

The hooks provide tools for interacting with VeBetterDAO's smart contracts and features:

### Galaxy Member Hooks

* `useB3trDonated`: Fetches the amount of B3TR tokens donated for a given token ID
* `useB3trToUpgrade`: Retrieves the amount of B3TR tokens required to upgrade a specific token
* `useB3trToUpgradeToLevel`: Gets the amount of B3TR needed to upgrade to a specific level
* `useGMBaseUri`: Retrieves the base URI for the Galaxy Member NFT metadata
* `useGMMaxLevel`: Fetches the maximum level possible for Galaxy Member NFTs
* `useGMbalance`: Gets the number of GM NFTs owned by an address
* `useGetNodeIdAttached`: Retrieves the Vechain Node Token ID attached to a given GM Token ID
* `useGetTokenIdAttachedToNode`: Gets the GM Token ID attached to a given Vechain Node ID
* `useGetTokensInfoByOwner`: Fetches token information for a specific owner with infinite scrolling support
* `useIsGMclaimable`: Determines if a user can claim a GM NFT
* `useLevelMultiplier`: Gets the reward multiplier for a specific GM level
* `useLevelOfToken`: Retrieves the level of a specified token
* `useParticipatedInGovernance`: Checks if an address has participated in governance
* `useSelectedGmNft`: Comprehensive hook for retrieving data related to a Galaxy Member NFT
* `useSelectedTokenId`: Gets the selected token ID for the selected galaxy member
* `useTokenIdByAccount`: Retrieves the token ID for an address given an index

### Node Management Hooks

* `useGetNodeManager`: Gets the address of the user managing a node ID (endorsement) either through ownership or delegation

### Rewards Hooks

* `useVotingRewards`: Manages voting rewards functionality
* `useVotingRoundReward`: Handles rewards for specific voting rounds

### Usage Example

```typescript
// Example usage of VeBetterDAO hooks
import {
    useGMbalance,
    useIsGMclaimable,
    useSelectedGmNft,
    useGetNodeManager,
    useParticipatedInGovernance,
} from '@vebetterdao';

const ExampleComponent = () => {
    const userAddress = "0x..."; // User's wallet address

    // Check GM NFT balance
    const { data: gmBalance } = useGMbalance(userAddress);

    // Check if user can claim a GM NFT
    const { isClaimable, isOwned } = useIsGMclaimable(userAddress);

    // Get comprehensive GM NFT data
    const {
        gmId,
        gmImage,
        gmName,
        gmLevel,
        gmRewardMultiplier,
        nextLevelGMRewardMultiplier,
        isGMLoading,
        isGMOwned,
        b3trToUpgradeGMToNextLevel,
        isEnoughBalanceToUpgradeGM,
        missingB3trToUpgrade,
        attachedNodeId,
        isXNodeAttachedToGM,
        maxGmLevel,
        isMaxGmLevelReached,
    } = useSelectedGmNft(userAddress);

    // Get node manager
    const { data: nodeManager } = useGetNodeManager(nodeId);

    // Check governance participation
    const { data: hasParticipated } = useParticipatedInGovernance(userAddress);

    console.log(
        'User GM NFTs:',
        gmBalance,
        'Can claim:',
        isClaimable,
        'Current GM Level:',
        gmLevel,
        'Node Manager:',
        nodeManager,
        'Has participated:',
        hasParticipated
    );

    return (
        // Your component JSX here
    );
};

export default ExampleComponent;
```
