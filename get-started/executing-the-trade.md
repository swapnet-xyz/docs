
# Executing the Trade

Once you have the API response with the `calldata`, executing the trade is straightforward. Follow these steps to prepare, sign, and send the transaction using your wallet.

## Implementation Example (TypeScript)

```typescript
const {
    routerAddress,
    calldata,
    gasLimit,
} = swapResponse;

// Approve the router contract to spend your sell tokens
await approveAsync(sellToken, wallet, routerAddress);

// Build the transaction
const unsignedTx = await wallet.populateTransaction({
    to: routerAddress,
    data: calldata,
    gasLimit: gasLimit,
    // Configure gas pricing for EIP-1559 transactions
    type: 2,
    maxFeePerGas,
    maxPriorityFeePerGas,
});

// Sign and send the transaction
const tx = await wallet.sendTransaction(unsignedTx);
```

## Prerequisites

Before executing the trade, ensure that:
1. Your wallet has sufficient balance of the sell token
2. You've approved the router contract to spend your tokens
3. Your wallet has enough ETH (or native token) to cover gas fees