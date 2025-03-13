# Send Transactions

This hook will take care of checking your connection type and handle the transaction submission between privy, cross-app and wallet connections.

When implementing VeChain Kit it is mandatory to use this hook to send transaction.

Use our pre built `TransactionModal` or `TransactionToast` components to show your users the progress and outcome of the transaction, or build your own UX and UI.

```typescript
'use client';

import {
    useWallet,
    useSendTransaction,
    useTransactionModal,
    TransactionModal,
    getConfig
} from '@vechain/vechain-kit';
import { IB3TR__factory } from '@vechain/vechain-kit/contracts';
import { humanAddress } from '@vechain/vechain-kit/utils';
import { useMemo, useCallback } from 'react';

export function TransactionExamples() {
    const { account } = useWallet();
    const b3trMainnetAddress = getConfig("main").b3trContractAddress;
    
    const clauses = useMemo(() => {
        const B3TRInterface = IB3TR__factory.createInterface();

        const clausesArray: any[] = [];
        clausesArray.push({
            to: b3trMainnetAddress,
            value: '0x0',
            data: B3TRInterface.encodeFunctionData('transfer', [
                "0x0, // receiver address
                '0', // 0 B3TR (in wei)
            ]),
            comment: `This is a dummy transaction to test the transaction modal. Confirm to transfer ${0} B3TR to ${humanAddress("Ox0")}`,
            abi: B3TRInterface.getFunction('transfer'),
        });

        return clausesArray;
    }, [connectedWallet?.address]);

    const {
        sendTransaction,
        status,
        txReceipt,
        resetStatus,
        isTransactionPending,
        error,
    } = useSendTransaction({
        signerAccountAddress: account?.address ?? '',
    });

    const {
        open: openTransactionModal,
        close: closeTransactionModal,
        isOpen: isTransactionModalOpen,
    } = useTransactionModal();

    // This is the function triggering the transaction and opening the modal
    const handleTransaction = useCallback(async () => {
        openTransactionModal();
        await sendTransaction(clauses);
    }, [sendTransaction, clauses, openTransactionModal]);
    
    const handleTryAgain = useCallback(async () => {
        resetStatus();
        await sendTransaction(clauses);
    }, [sendTransaction, clauses, resetStatus]);

    return (
        <>
            <button
                onClick={handleTransactionWithModal}
                isLoading={isTransactionPending}
                isDisabled={isTransactionPending}
            >
                Send B3TR
            </button>

            <TransactionModal
                isOpen={isTransactionModalOpen}
                onClose={closeTransactionModal}
                status={status}
                txReceipt={txReceipt}
                txError={error}
                onTryAgain={handleTryAgain}
                uiConfig={{
                    title: 'Test Transaction',
                    description: `This is a dummy transaction to test the transaction modal. Confirm to transfer ${0} B3TR to ${
                        account?.address
                    }`,
                    showShareOnSocials: true,
                    showExplorerButton: true,
                    isClosable: true,
                }}
            />
        </>
    );
}

```

You can build clauses with some of our available build functions, with our [SDK](https://docs.vechain.org/developer-resources/sdks-and-providers/sdk) or  with [connex](https://docs.vechain.org/developer-resources/sdks-and-providers/connex).

If you want to interact directly with the user's smart account read the [Smart Accounts](../social-login/smart-accounts.md#multiclause-transactions) section.

{% hint style="warning" %}
**Important**

When implementing transaction logic within your application, it's crucial to ensure that all necessary data Data Pre-fetching for Transactions.

Ensuring data is pre-fetched before initiating a transaction is crucial to avoid browser pop-up blocking for users using social login, which can adversely affect user experience.

```javascript
// ✅ Good: Pre-fetch data
const { data } = useQuery(['someData'], fetchSomeData);
const sendTx = () => sendTransaction(data);

// ❌ Bad: Fetching data during the transaction
const sendTx = async () => {
  const data = await fetchSomeData();
  return sendTransaction(data);
};
```
{% endhint %}

