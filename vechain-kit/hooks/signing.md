# Signing

## Signing Hooks

The hooks provide tools for signing messages and typed data on VeChain:

### Core Signing Hooks

* `useSignMessage`: Hook for signing plain text messages, supporting both Privy and VeChain wallets
* `useSignTypedData`: Hook for signing typed data (EIP-712), supporting both Privy and VeChain wallets

### Types

```typescript
// Signing Types
interface SignTypedDataParams {
    domain: {
        name?: string;
        version?: string;
        chainId?: number;
        verifyingContract?: string;
        salt?: string;
    };
    types: Record<string, Array<{ name: string; type: string }>>;
    message: Record<string, any>;
}

interface UseSignMessageReturnValue {
    signMessage: (message: string) => Promise<string>;
    isSigningPending: boolean;
    signature: string | null;
    error: Error | null;
    reset: () => void;
}

interface UseSignTypedDataReturnValue {
    signTypedData: (data: SignTypedDataParams) => Promise<string>;
    isSigningPending: boolean;
    signature: string | null;
    error: Error | null;
    reset: () => void;
}
```

### Usage example

```typescript
// Example usage of Signing hooks
import { useSignMessage, useSignTypedData } from '@vechain/vechain-kit';

const ExampleComponent = () => {
    // Example of signing a message
    const { 
        signMessage,
        isSigningPending: isMessagePending,
        signature: messageSignature,
        error: messageError,
        reset: resetMessage
    } = useSignMessage();

    // Example of signing typed data
    const {
        signTypedData,
        isSigningPending: isTypedDataPending,
        signature: typedDataSignature,
        error: typedDataError,
        reset: resetTypedData
    } = useSignTypedData();

    const handleMessageSign = async () => {
        try {
            const signature = await signMessage("Hello VeChain!");
            console.log("Message signature:", signature);
        } catch (error) {
            console.error("Message signing error:", error);
        }
    };

    const handleTypedDataSign = async () => {
        try {
            const typedData = {
                domain: {
                    name: 'MyDApp',
                    version: '1',
                    chainId: 1,
                    verifyingContract: '0x...'
                },
                types: {
                    Person: [
                        { name: 'name', type: 'string' },
                        { name: 'age', type: 'uint256' }
                    ]
                },
                message: {
                    name: 'John Doe',
                    age: 30
                }
            };
            
            const signature = await signTypedData(typedData);
            console.log("Typed data signature:", signature);
        } catch (error) {
            console.error("Typed data signing error:", error);
        }
    };

    return (
        <div>
            <button 
                onClick={handleMessageSign} 
                disabled={isMessagePending}
            >
                Sign Message
            </button>
            <button 
                onClick={handleTypedDataSign} 
                disabled={isTypedDataPending}
            >
                Sign Typed Data
            </button>

            {/* Display signatures */}
            {messageSignature && (
                <div>Message Signature: {messageSignature}</div>
            )}
            {typedDataSignature && (
                <div>Typed Data Signature: {typedDataSignature}</div>
            )}

            {/* Display errors */}
            {messageError && (
                <div>Message Error: {messageError.message}</div>
            )}
            {typedDataError && (
                <div>Typed Data Error: {typedDataError.message}</div>
            )}

            {/* Reset buttons */}
            <button onClick={resetMessage}>Reset Message Signing</button>
            <button onClick={resetTypedData}>Reset Typed Data Signing</button>
        </div>
    );
};

export default ExampleComponent;

/*
Note: These hooks require:
- Valid wallet connection (Privy or VeChain)
- For typed data signing:
  - Valid EIP-712 typed data structure
  - Proper domain specification
  - Correct types definition
*/
```
