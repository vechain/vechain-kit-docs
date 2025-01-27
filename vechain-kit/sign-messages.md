# Sign Messages

### `useSignMessage()`

Sign a string, eg "Hello VeChain".

```typescript
'use client';

import { ReactElement, useCallback } from 'react';
import {
    useSignMessage,
} from '@vechain/vechain-kit';

export function SigningExample(): ReactElement {
    const {
        signMessage,
        isSigningPending: isMessageSignPending,
        signature: messageSignature,
    } = useSignMessage();

   const handleSignMessage = useCallback(async () => {
        try {
            const signature = await signMessage('Hello VeChain!');
            toast({
                title: 'Message signed!',
                description: `Signature: ${signature.slice(0, 20)}...`,
                status: 'success',
                duration: 1000,
                isClosable: true,
            });
        } catch (error) {
            toast({
                title: 'Signing failed',
                description:
                    error instanceof Error ? error.message : String(error),
                status: 'error',
                duration: 1000,
                isClosable: true,
            });
        }
    }, [signMessage, toast]);

    return (
        <>
            <button
                onClick={handleSignTypedData}
                isLoading={isTypedDataSignPending}
            >
                Sign Typed Data
            </button>
            {typedDataSignature && (
                <h4>
                    {typedDataSignature}
                </h4>
            )}
        </>
    );
}
```

### `useSignTypedData()`

Hook to sign typed data using the connected wallet.&#x20;

Supports both Privy and VeChain wallets.

Return Object containing the signing function and status

```typescript
'use client';

import { ReactElement, useCallback } from 'react';
import {
    useWallet,
    useSignTypedData,
} from '@vechain/vechain-kit';

// Example EIP-712 typed data
const exampleTypedData = {
    domain: {
        name: 'VeChain Example',
        version: '1',
        chainId: 1,
    },
    types: {
        Person: [
            { name: 'name', type: 'string' },
            { name: 'wallet', type: 'address' },
        ],
    },
    message: {
        name: 'Alice',
        wallet: '0x0000000000000000000000000000000000000000',
    },
    primaryType: 'Person',
};

export function SigningTypedDataExample(): ReactElement {
    const {
        signTypedData,
        isSigningPending: isTypedDataSignPending,
        signature: typedDataSignature,
    } = useSignTypedData();

    const handleSignTypedData = useCallback(async () => {
        try {
            const signature = await signTypedData(exampleTypedData);
            alert({
                title: 'Typed data signed!',
                description: `Signature: ${signature.slice(0, 20)}...`,
                status: 'success',
                duration: 1000,
                isClosable: true,
            });
        } catch (error) {
            alert({
                title: 'Signing failed',
                description:
                    error instanceof Error ? error.message : String(error),
                status: 'error',
                duration: 1000,
                isClosable: true,
            });
        }
    }, [signTypedData, toast]);

    return (
        <>
            <button
                onClick={handleSignTypedData}
                isLoading={isTypedDataSignPending}
            >
                Sign Typed Data
            </button>
            {typedDataSignature && (
                <h4>
                    {typedDataSignature}
                </h4>
            )}
        </>
    );
}
```

## DAppKit

If you are sure you are interacting with VeWorld then you can request VeChain's classic certificate signing method allowed by connex and the SDK.

### Certificate Signing

Only supported on Wallet connections, as VeWorld and Sync2.

You can use Sign Typed Data instead.

If you need to authenticate an user using a smart account, you will need to make the embedded wallet sign a message where he confirms to be the owner of the smart account, and then verify the signature.

In the VeChainKitProvider add:

```typescript
return (
   <VeChainKitProvider
            //other props...
             
            network={{
                type: 'main',
                requireCertificate: true,
                connectionCertificate: {
                    message: {
                        purpose: 'identification',
                        payload: {
                            type: 'text',
                            content: 'Please sign this message to connect your wallet.',
                        },
                    },
                },
            }}
            
            //other props...
        >
            {children}
        </VeChainKitProvider>
)
```

### Invoke conditionally

#### Use connex

You can import the connex sdk and use it's functionalities when the user is connected through a wallet.

{% tabs %}
{% tab title="Sign" %}
```typescript
import { useConnex } from '@vechain/vechain-kit';

export default function Home(): ReactElement {
    const connex = useConnex();

    const handleSignWithConnex = () => {
        connex.vendor.sign(    
            'cert',
            {
                purpose: 'identification',
                payload: {
                    type: 'text',
                    content: 'Please sign this message to connect your wallet.',
                },
            }
        ).request();
    }
    
    return <button onClick={handleSignWithConnex}> Sign </button>
}
```
{% endtab %}

{% tab title="Connex Vendor Interface" %}
```typescript
declare namespace Connex {
    /** the vendor interface to interact with wallets */
    interface Vendor {
        /** create the tx signing service */
        sign(kind: 'tx', msg: Vendor.TxMessage): Vendor.TxSigningService

        /** create the cert signing service */
        sign(kind: 'cert', msg: Vendor.CertMessage): Vendor.CertSigningService
    }

    namespace Vendor {
        /** the interface is for requesting user wallet to sign transactions */
        interface TxSigningService {
            /** designate the signer address */
            signer(addr: string): this

            /** set the max allowed gas */
            gas(gas: number): this

            /** set another txid as dependency */
            dependsOn(txid: string): this

            /** 
             * provides the url of web page to reveal tx related information.
             * first appearance of slice '{txid}' in the given link url will be replaced with txid.
             */
            link(url: string): this

            /** set comment for the tx content */
            comment(text: string): this

            /**
             * enable VIP-191 by providing url of web api, which provides delegation service 
             * @param url the url of web api
             * @param signer hint of the delegator address
             */
            delegate(url: string, signer?: string): this

            /** register a callback function fired when the request is accepted by user wallet */
            accepted(cb: () => void): this

            /** send the request */
            request(): Promise<TxResponse>
        }

        /** the interface is for requesting user wallet to sign certificates */
        interface CertSigningService {
            /** designate the signer address */
            signer(addr: string): this

            /** 
             * provides the url of web page to reveal cert related information.
             * first appearance of slice '{certid}' in the given link url will be replaced with certid.
             */
            link(url: string): this

            /** register a callback function fired when the request is accepted by user wallet */
            accepted(cb: () => void): this

            /** send the request */
            request(): Promise<CertResponse>
        }

        type TxMessage = Array<Connex.VM.Clause & {
            /** comment to the clause */
            comment?: string
            /** as the hint for wallet to decode clause data */
            abi?: object
        }>

        type CertMessage = {
            purpose: 'identification' | 'agreement'
            payload: {
                type: 'text'
                content: string
            }
        }

        type TxResponse = {
            txid: string
            signer: string
        }

        type CertResponse = {
            annex: {
                domain: string
                timestamp: number
                signer: string
            }
            signature: string
        }
    }
}

```
{% endtab %}
{% endtabs %}

### Use SDK

Coming soon.
