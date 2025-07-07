---
description: Core API changes when migrating from VeChain Kit 1.x to 2.0
---

# API Changes

### Update Dependencies

```bash
npm install @vechain/vechain-kit@^2.0.0
npm uninstall @thor-devkit
```

### Deprecated/Removed Hooks

```typescript
// Before (1.x)
import { useConnex } from '@vechain/vechain-kit';
const { thor } = useConnex();

// After (2.x)
import { useThor } from '@vechain/vechain-kit';
const thor = useThor();
```

<details>

<summary><strong>Complete list of removed hooks</strong></summary>

#### Utils Hooks:

* `useRoundAppVotes`
* `useSustainabilityActions`

#### Galaxy Member Hooks:

* `useGMbalance`
* `useB3trToUpgrade`
* `useB3trToUpgradeToLevel`
* `useGetNodeIdAttached`
* `useGetTokenIdAttachedToNode`
* `useGMMaxLevel`
* `useParticipatedInGovernance`
* `useTokenIdByAccount`
* `useNFTImage`
* `useB3trDonated`
* `useGMBaseUri`
* `useSelectedTokenId`
* `useIsGMClaimable`
* `useSelectedGmNft`
* `useLevelOfToken`
* `useNFTMetadataUri`

#### NodeManagement:

* `useGetNodeManager`
* `useIsNodeHolder`
* `useUserXNodes`

#### VeBetterPassport:

* `useAccountLinking`
* `usePassportChecks`
* `useUserDelegation`
* `useUserStatus`
* `useAppSecurityLevel`
* `useGetCumulativeScoreWithDecay`
* `useGetDelegatee`
* `useGetDelegator`
* `useGetEntitiesLinkedToPassport`
* `useGetPassportForEntity`
* `useGetPendingDelegationsDelegateePOV`
* `useGetPendingDelegationsDelegatorPOV`
* `useGetPendingLinkings`
* `useIsEntity`
* `useIsPassportCheckEnabled`
* `useIsPassport`
* `useParticipationScoreThreshold`
* `useSecurityMultiplier`
* `useThresholdParticipationScore`
* `useThresholdParticipationScoreAtTimepoint`
* `useIsBlacklisted`
* `useIsWhitelisted`
* `useUserRoundScore`

#### VBD VoterRewards:

* `useLevelMultiplier`

#### X2Earn Apps:

* `useUserVotesInAllRounds`
* `useUserTopVotedApps`
* `useXNode`
* `useAppAdmin`
* `useAppExists`
* `useAppsEligibleInNextRound`
* `useGetX2EarnAppAvailableFunds`
* `useXAppsMetadataBaseUri`
* `useXNodeCheckCooldown`

#### XAllocation Voting:

* `useAllocationAmount`
* `useXAppVotesQf`

</details>

### Update Imports

```typescript
// Before (1.x)
import { useConnex, useWallet, useTransaction } from '@vechain/vechain-kit';

// After (2.x)
import { useThor, useWallet, useBuildTransaction, useCallClause } from '@vechain/vechain-kit';
```

### Update Contract Calls

#### Reading Data

```typescript
// Before (1.x) - Manual pattern
const functionAbi = contractAbi.find((e) => e.name === "balanceOf");
const res = await thor.account(contractAddress).method(functionAbi).call(address);
const balance = ethers.formatEther(res.decoded[0]);

// After (2.x) - useCallClause
const { data } = useCallClause({
  abi: TokenContract__factory.abi,
  address: contractAddress as `0x${string}`,
  method: 'balanceOf' as const,
  args: [address],
  queryOptions: {
    enabled: !!address,
    select: (data) => ethers.formatEther(data[0]),
  },
});
```

#### Writing Data (Transactions)

```typescript
// Before (1.x) - Manual clause building
const functionAbi = contractAbi.find((e) => e.name === "approve");
const clause = thor.account(contractAddress).method(functionAbi).asClause(spender, amount);

// After (2.x) - Enhanced clauses
const { sendTransaction } = useBuildTransaction({
  clauseBuilder: () => {
    const { clause } = thor.contracts
      .load(contractAddress, ERC20__factory.abi)
      .clause.approve(spender, amount);
    
    return [{ ...clause, comment: 'Approve tokens' }];
  },
});
```

### Key Patterns

#### Type Safety

```typescript
const contractAddress = config.contractAddress as `0x${string}`;
const method = 'balanceOf' as const;
```

#### Query Enablement

```typescript
queryOptions: {
  enabled: !!address && !!tokenAddress,
}
```

#### Error Handling

```typescript
if (error?.message.includes('reverted')) {
  // Handle contract revert
} else if (error?.message.includes('network')) {
  // Handle network error
}
```

### Next Steps

1. Review [Contract Patterns](contract-patterns.md) for detailed interaction examples
2. Apply [Best Practices](best-practices.md) for optimal performance
3. Verify all functionality works as expected
4. Consult [Troubleshooting](troubleshooting.md) for common issues
