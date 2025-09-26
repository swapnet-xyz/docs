# Using the SDK

The SwapNet TypeScript SDK provides a convenient wrapper around our REST API, making it easier to integrate swap functionality into your applications. This guide will walk you through installation, setup, and basic usage.

## Installation

Install the SwapNet SDK using npm:

```bash
npm i @swapnet-xyz/sdk
```

## Quick Start

### Basic Setup

First, create a SwapNet client instance with your API key:

```typescript
import { SwapnetClient, RouterUname } from '@swapnet-xyz/sdk';

const client = new SwapnetClient(apiKey);
```

### Performing a swap call

Here's how to perform a basic swap call with the SDK:

```typescript
// Configure swap parameters
const chainId = 1; // Ethereum mainnet
const sellTokenAddress = '0xA0b86a33E6A94...'; // Token you're selling
const buyTokenAddress = '0x6B175474E89094...'; // Token you're buying
const sellAmount = '1000000000000000000'; // Amount in wei (1 token with 18 decimals)
const userAddress = '0x742d35Cc5e98...'; // Your wallet address
const slippageTolerance = 0.005; // 0.5% slippage tolerance
const useRfq = true; // Use Request for Quote for better pricing

// Execute the swap query
const result = await client.swapAsync(
    chainId,
    sellTokenAddress,
    buyTokenAddress,
    sellAmount,
    undefined, // buyAmount - leave undefined for sell orders
    useRfq,
    RouterUname.Native,
    true, // includeCalldata
    userAddress,
    slippageTolerance,
);

// Handle the response
if (!result.succeeded) {
    throw new Error(`Swap request failed: ${result.error}`);
}

// Extract transaction details
const {
    routerAddress,
    calldata,
    gasLimit,
} = result.swapResponse;

// Use these values to execute the transaction on-chain
console.log('Router address:', routerAddress);
console.log('Transaction calldata:', calldata);
console.log('Estimated gas limit:', gasLimit);
```
