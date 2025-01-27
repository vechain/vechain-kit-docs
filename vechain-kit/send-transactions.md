# Send Transactions

This hook will take care of checking your connection type and handle the transaction submission between privy, cross-app and wallet connections.

When implementing VeChain Kit it is mandatory to use this hook to send transaction.

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
        progress,
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
                progress={progress}
                txId={txReceipt?.meta.txID}
                errorDescription={error?.reason ?? 'Unknown error'}
                showSocialButtons={true}
                showExplorerButton={true}
                onTryAgain={handleTransactionWithModal}
                showTryAgainButton={true}
            />
        </>
    );
}

```

You can build clauses with some of our available build functions, or with the [SDK](https://docs.vechain.org/developer-resources/sdks-and-providers/sdk) or connex.

{% hint style="warning" %}
Multiclause transaction are currently not supported when using Privy with **ecosystem cross-app connection**. Clauses will are currently separated into independent transactions.\
Use the `progress` property returned by the `useSendTransaction()` hook to track the progress of the transactions.\
\
Keep in mind if a transaction fails previous "clauses" are not reverted.\
\
Work is in progress to provide a quick fix for this.
{% endhint %}

