# veDelegate

## VeDelegate Hooks

The hooks provide tools for interacting with VeDelegate token and functionality:

### Token Hooks

* `useGetVeDelegateBalance`: Retrieves the VeDelegate token balance for a given address

### Usage Example

```typescript
// Example usage of VeDelegate hooks
import { useGetVeDelegateBalance } from '@veDelegate';

const ExampleComponent = () => {
    const userAddress = "0x..."; // User's wallet address

    // Get VeDelegate token balance
    const { data: veDelegateBalance, isLoading } = useGetVeDelegateBalance(userAddress);

    console.log(
        'VeDelegate Balance:',
        veDelegateBalance,
        'Loading:',
        isLoading
    );

    return (
        // Your component JSX here
    );
};

export default ExampleComponent;

/*
Note: The VeDelegate balance query will only be enabled when:
- A valid thor connection is available
- A valid address is provided
- A valid network type is configured

The balance is returned as a string representing the token amount.
*/
```
