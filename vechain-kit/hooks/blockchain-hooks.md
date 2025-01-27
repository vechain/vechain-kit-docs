---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Blockchain Hooks

### `useCurrentBlock()`

Fetches the current block from the blockchain with automatic updates.

```typescript
const { data, isLoading, error } = useCurrentBlock();
```

Features:

* Auto-refreshes every 10 seconds
* Caches data for 1 minute
* Returns the latest block information as Connex.Thor.Block

Example:

```typescript
function BlockInfo() {
  const { data: block } = useCurrentBlock();
  
  return <div>Current Block: {block?.number}</div>;
}
```

### `useTxReceipt()`

Polls the blockchain for a transaction receipt until it is found or times out.

```typescript
const { data, isLoading, error } = useTxReceipt(txId, blockTimeout);
```

Parameters:

* txId: (string) The transaction ID to monitor
* blockTimeout: (optional number) Number of blocks to wait before timing out (default: 5)

Returns:

* data: Transaction receipt (Connex.Thor.Transaction.Receipt)
* isLoading: Boolean indicating if the receipt is being fetched
* error: Error object if the operation fails

Example:

```typescript
function TransactionStatus({ txId }) {
  const { data: receipt, isLoading } = useTxReceipt(txId);
  
  if (isLoading) return <div>Loading...</div>;
  return <div>Transaction Status: {receipt?.reverted ? 'Failed' : 'Success'}</div>;
}
```

## Utility Functions

### **`getEvents()`**

Fetches events from the blockchain based on specified criteria.

```typescript
const events = await getEvents({
  nodeUrl,
  thor,
  filterCriteria,
  order: 'asc',
  offset: 0,
  limit: 1000,
  from: 0,
  to: undefined
});
```

### **`getAllEvents()`**

Iteratively fetches all events matching the criteria, handling pagination automatically.

```typescript
const allEvents = await getAllEvents({
  nodeUrl,
  thor,
  filterCriteria,
  order: 'asc',
  from: 0,
  to: undefined
});
```

Parameters for getEvents/getAllEvents:

* nodeUrl: VeChain node URL
* thor: Thor client instance
* filterCriteria: Array of event filter criteria
* order: Sort order ('asc' or 'desc')
* from: Starting block number
* to: Ending block number
* offset: (getEvents only) Number of events to skip
* limit: (getEvents only) Maximum number of events to return

These hooks and functions are designed to work with the VeChain blockchain and require the VeChain Kit context to be properly set up in your application.
