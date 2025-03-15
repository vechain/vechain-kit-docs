# Utils

## Utility Hooks

The hooks provide utility functions for interacting with the VeChain network and tokens:

### Network Utility Hooks

* `useGetChainId`: Retrieves the chain ID from the genesis block of the connected network
* `useGetNodeUrl`: Returns the node URL being used, either custom or default for the network

### Token Utility Hooks

* `useGetCustomTokenBalances`: Fetches balances for multiple custom tokens for a given address, returning original, scaled, and formatted values
* `useGetCustomTokenInfo`: Retrieves token information (name, symbol, decimals) for a custom token address

### Usage Example

```typescript
// Example usage of Utility hooks
import {
    useGetChainId,
    useGetNodeUrl,
    useGetCustomTokenBalances,
    useGetCustomTokenInfo
} from '@vechain/vechain-kit';

const ExampleComponent = () => {
    const address = "0x..."; // User's wallet address
    const tokenAddress = "0x..."; // Custom token address

    // Get network information
    const { data: chainId } = useGetChainId();
    const nodeUrl = useGetNodeUrl();

    // Get custom token information
    const { data: tokenInfo } = useGetCustomTokenInfo(tokenAddress);

    // Get token balances
    const { 
        data: tokenBalances,
        isLoading,
        error 
    } = useGetCustomTokenBalances(address);

    console.log(
        'Chain ID:', chainId,
        'Node URL:', nodeUrl,
        'Token Info:', tokenInfo,
        'Token Balances:', tokenBalances
    );

    return (
        // Your component JSX here
    );
};

export default ExampleComponent;

/*
Note: These hooks require:
- A valid thor connection
- Appropriate network configuration
- Valid input parameters where applicable

The token-related hooks work with ERC20 tokens and return:
- Original values (raw from contract)
- Scaled values (adjusted for decimals)
- Formatted values (human-readable)
*/
```
