# Using the REST API

## Getting Your API Key

### Option 1: Telegram Bot (Recommended)

If you have a Telegram account, you can quickly obtain your API key through our [demo bot](https://t.me/SwapNetDemoBot):

1. Open the bot and navigate to the main menu
2. Select "More..."
3. Click "Get API key"

**Note:** Each Telegram account can create one API key. If you've already created a key, the bot will return your existing one.

### Option 2: Email Request

If you don't use Telegram or need additional API keys, please [request an API key](mailto:info@swap-net.xyz?subject=Request%20for%20API%20key) via email.


## Making API Calls

To get a swap quote, make a GET request to the swap endpoint with the required parameters:

```bash
curl "https://app.swap-net.xyz/api/v1.0/swap?chainId=999&sellToken=0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb&buyToken=0x5555555555555555555555555555555555555555&sellAmount=10000000000&useRfq=true&includeCalldata=true&userAddress=0x11b86991c6218b36c1d19d4a2e9eb0ce3606eb49&slippageTolerance=0.01&router=swapnet-router&apiKey=<Your API Key>"
```

### Response Format

The API returns a JSON response with the following structure:
```json
{
    "aggregator": "swapnet",
    "router": "swapnet-router",
    "sell": {
        "referenceId": 1,
        "amount": "10000000000"
    },
    "buy": {
        "referenceId": 0,
        "amount": "170261032657489111452"
    },
    "nativeTokenUsdPrice": 58.75669929845798,
    "tokens": [...],
    "routes": [...],
    "routerAddress": "0x616000e384Ef1C2B52f5f3A88D57a3B64F23757e",
    "calldata": "0x873955400000000000...",
    "gasLimit": "1200000"
}
```

### Understanding the Response

The response contains several key components:

- **Routing Information**: Fields like `sell`, `buy`, `tokens`, and `routes` construct the graph-based routing plan, which can be encoded for any router contract.
- **Ready-to-Use Data**: When `includeCalldata` is set to `true` in the request, the API provides:
  - `calldata`: Pre-encoded transaction data for the specified router
  - `routerAddress`: Target contract address  
  - `gasLimit`: Suggested gas limit for the transaction

## Executing the Trade

Once you have the API response with the `calldata`, executing the trade is straightforward. Follow these steps to prepare, sign, and send the transaction using your wallet.

### Implementation Example (TypeScript)

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

### Prerequisites

Before executing the trade, ensure that:
1. Your wallet has sufficient balance of the sell token
2. You've approved the router contract to spend your tokens
3. Your wallet has enough ETH (or native token) to cover gas fees