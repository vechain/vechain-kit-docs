---
description: New contract interaction patterns in VeChain Kit 2.0
---

# Contract Patterns

### Single Contract Call Pattern

```typescript
import { VOT3__factory } from '@vechain/vechain-kit/contracts';
import { useCallClause, getCallClauseQueryKeyWithArgs } from '@vechain/vechain-kit';

const abi = VOT3__factory.abi;
const method = 'convertedB3trOf' as const;

export const useTokenBalance = (address?: string) => {
  const { network } = useVeChainKitConfig();
  const contractAddress = getConfig(network.type).contractAddress as `0x${string}`;

  return useCallClause({
    abi,
    address: contractAddress,
    method,
    args: [address ?? ''],
    queryOptions: {
      enabled: !!address,
      select: (data) => ({
        balance: ethers.formatEther(data[0]),
        formatted: humanNumber(ethers.formatEther(data[0])),
      }),
    },
  });
};
```

### Multiple Contract Calls Pattern

```typescript
import { useQuery } from '@tanstack/react-query';
import { executeMultipleClausesCall } from '@vechain/vechain-kit';

export const useMultipleTokenData = (addresses: string[]) => {
  const thor = useThor();

  return useQuery({
    queryKey: ['MULTIPLE_TOKENS', addresses],
    queryFn: async () => {
      const results = await executeMultipleClausesCall({
        thor,
        calls: addresses.map((address) => ({
          abi: ERC20__factory.abi,
          functionName: 'balanceOf',
          address: address as `0x${string}`,
          args: [userAddress],
        })),
      });

      return addresses.map((address, index) => ({
        address,
        balance: ethers.formatEther(results[index][0]),
      }));
    },
    enabled: !!addresses.length,
  });
};
```

### Transaction Building Pattern

```typescript
import { useBuildTransaction, useWallet } from '@vechain/vechain-kit';

export const useTokenTransfer = () => {
  const { account } = useWallet();
  const thor = useThor();

  return useBuildTransaction({
    clauseBuilder: (recipient: string, amount: string) => {
      if (!account?.address) return [];

      const { clause } = thor.contracts
        .load(tokenAddress, ERC20__factory.abi)
        .clause.transfer(recipient, ethers.parseEther(amount));

      return [{
        ...clause,
        comment: `Transfer ${amount} tokens to ${recipient}`,
      }];
    },
    onTxConfirmed: () => {
      queryClient.invalidateQueries({ queryKey: ['TOKEN_BALANCE'] });
    },
  });
};
```

### Multi-Clause Transactions

```typescript
const useApproveAndSwap = () => {
  const { account } = useWallet();
  const thor = useThor();

  return useBuildTransaction({
    clauseBuilder: (tokenAddress: string, amount: string) => {
      if (!account?.address) return [];

      return [
        // Approve
        {
          ...thor.contracts
            .load(tokenAddress, ERC20__factory.abi)
            .clause.approve(swapAddress, ethers.parseEther(amount)).clause,
          comment: 'Approve token spending',
        },
        // Swap
        {
          ...thor.contracts
            .load(swapAddress, SwapContract__factory.abi)
            .clause.swap(tokenAddress, ethers.parseEther(amount)).clause,
          comment: 'Execute swap',
        },
      ];
    },
  });
};
```

### Error Handling

```typescript
const useContractCall = (address: string) => {
  return useCallClause({
    abi: ContractABI,
    address: address as `0x${string}`,
    method: 'getData',
    args: [],
    queryOptions: {
      enabled: !!address,
      retry: (failureCount, error) => {
        if (error.message.includes('reverted')) return false;
        return failureCount < 3;
      },
    },
  });
};
```
