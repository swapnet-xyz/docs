# /swap

## Overview

Unlike most DEX aggregators, our swap endpoint provides graph-based routing plans with optional on-demand calldata generation. Each routing plan specifies the exact amounts and tokens to trade with various liquidity sources, including AMMs and market maker liquidity.

When calldata is not requested, clients can use an encoder to convert the routing plans into transaction calldata for any compatible router contract. This flexibility allows you to choose the most suitable router for your specific use case.

By establishing a standardized format for routing plans, our approach decouples the aggregator API from specific router contracts. This standardization enables seamless encoding and settlement of routing results from any compatible aggregator using any supported router contract.

## API schema

### Swap endpoint

```bash
https://app.swap-net.xyz/api/v1.0/swap
```

### Parameters

| Name                | Required?                        | Description                                 | Valid values                                                                                                                                              |
| ------------------- | -------------------------------- | ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `chainId`           | Yes                              | Integer ID of the blockchain                | Chain IDs defined [here](https://github.com/swapnet-xyz/swapnet-sdk/blob/3ebb3511cd6abcf672b6f11742278b1ae1ba4e21/src/common/unames.ts#L3-L16)            |
| `sellToken`         | Yes                              | Address of sell token                       | Blockchain addresses                                                                                                                                      |
| `buyToken`          | Yes                              | Address of buy token                        | Blockchain addresses                                                                                                                                      |
| `sellAmount`        | Yes\*                            | Amount of sellToken to sell in base unit    | Positive numbers                                                                                                                                          |
| `buyAmount`         | Yes\*                            | Amount of buyToken to buy in base unit      | Positive numbers                                                                                                                                          |
| `useRfq`            | No (default to `false`)          | Whether to use market makers' RFQ liquidity | `boolean`                                                                                                                                                 |
| `router`            | No (default to `default-router`) | Name of router contract                     | Router unique names defined [here](https://github.com/swapnet-xyz/swapnet-sdk/blob/3ebb3511cd6abcf672b6f11742278b1ae1ba4e21/src/common/unames.ts#L64-L74) |
| `includeCalldata`   | No (default to `false`)          | Whether to include calldata in the response | `boolean`                                                                                                                                                 |
| `userAddress`       | Yes if `includeCalldata=true`    | User’s wallet address                       | Blockchain addresses                                                                                                                                      |
| `slippageTolerance` | Yes if `includeCalldata=true`    | Slippage tolerance (`0.01` for 1%)          | Numbers between 0 and 1                                                                                                                                   |
| `apiKey`            | Yes                              | Your API Key                                | `string`                                                                                                                                                  |

\*Notice that one and only one of `sellAmount` and `buyAmount` will be accepted!

### Simple query example

```bash
curl "https://app.swap-net.xyz/api/v1.0/swap\
?chainId=1\
&sellToken=0x853d955acef822db058eb8505911ed77f175b99e\
&buyToken=0x1f9840a85d5af5bf1d1762f925bdaddc4201f984\
&sellAmount=10000000000000000000000\
&apiKey=<Your API Key>"
```

Response includes:

| Field name            | Description                                                                                                                                                                         |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tokens`              | Metadata of a list of tokens involved in the routing result. Including token’s `address`, `name`, `symbol`, `decimals`, `usdPrice`, and a `referenceId` used to refer to the token. |
| `sell`                | Sell token’s `referenceId` and `amount` to sell.                                                                                                                                    |
| `buy`                 | Buy token’s `referenceId` and `amount` to buy.                                                                                                                                      |
| `nativeTokenUsdPrice` | Current price of native token on this chain.                                                                                                                                        |
| `routes`              | A collection of swaps forming a directed acyclic graph that converts all sell tokens into buy tokens. Notice that the orders of the routes are not guaranteed.                      |
| `route.name`          | Name of liquidity source used by this route (swap).                                                                                                                                 |
| `route.address`       | Address of liquidity pool.                                                                                                                                                          |
| `route.fromToken`     | `referenceId` and `amount` of token to swap from.                                                                                                                                   |
| `route.toToken`       | `referenceId` and `amount` of token to swap to.                                                                                                                                     |
| `route.details`       | Additional info about this route (swap) which is used during encoding.                                                                                                              |

```bash
{
  "aggregator": "swapnet",
  "router": "default-router",
  "sell": {
    "referenceId": 7,
    "amount": "10000000000000000000000"
  },
  "buy": {
    "referenceId": 6,
    "amount": "1034955025987043448356"
  },
  "nativeTokenUsdPrice": 4622.638914277432,
  "tokens": [
    {
      "referenceId": 7,
      "address": "0x853d955acef822db058eb8505911ed77f175b99e",
      "name": "Frax",
      "symbol": "FRAX",
      "decimals": 18,
      "usdPrice": 0.9985832349359104
    },
    {
      "referenceId": 1,
      "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
      "name": "USDCoin",
      "symbol": "USDC",
      "decimals": 6,
      "usdPrice": 1
    },
    {
      "referenceId": 0,
      "address": "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2",
      "name": "Wrapped Ether",
      "symbol": "WETH",
      "decimals": 18,
      "usdPrice": 4622.638914277432
    },
    {
      "referenceId": 2,
      "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
      "name": "Tether USD",
      "symbol": "USDT",
      "decimals": 6,
      "usdPrice": 1.0005286652107792
    },
    {
      "referenceId": 6,
      "address": "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984",
      "name": "Uniswap",
      "symbol": "UNI",
      "decimals": 18,
      "usdPrice": 9.632281172169868
    }
  ],
  "routes": [
    {
      "address": "0xc63b0708e2f7e69cb8a1df0e1389a98c35a76d52",
      "name": "UniswapV3",
      "details": {
        "fee": 500
      },
      "fromTokens": [
        {
          "referenceId": 7,
          "amount": "10000000000000000000000"
        }
      ],
      "toTokens": [
        {
          "referenceId": 1,
          "amount": "9980414364"
        }
      ]
    },
    {
      "address": "0xc7bbec68d12a0d1830360f8ec58fa599ba1b0e9b",
      "name": "UniswapV3",
      "details": {
        "fee": 100
      },
      "fromTokens": [
        {
          "referenceId": 2,
          "amount": "9973989690"
        }
      ],
      "toTokens": [
        {
          "referenceId": 0,
          "amount": "2162884170453897195"
        }
      ]
    },
    {
      "address": "0x3416cf6c708da44db2624d63ea0aaef7113527c6",
      "name": "UniswapV3",
      "details": {
        "fee": 100
      },
      "fromTokens": [
        {
          "referenceId": 1,
          "amount": "9980414291"
        }
      ],
      "toTokens": [
        {
          "referenceId": 2,
          "amount": "9973989608"
        }
      ]
    },
    {
      "address": "0x8626be23f128f5985a5d76359b10bf3db84a7306",
      "name": "RingswapV2",
      "details": {
        "feeInBps": 30,
        "fromFewWrappedTokenAddress": "0xa250cc729bb3323e7933022a67b52200fe354767",
        "toFewWrappedTokenAddress": "0xe8e1f50392bd61d0f8f48e8e7af51d3b8a52090a"
      },
      "fromTokens": [
        {
          "referenceId": 0,
          "amount": "2162884172944070813"
        }
      ],
      "toTokens": [
        {
          "referenceId": 6,
          "amount": "1034955025987043448356"
        }
      ]
    }
  ]
}
```

### Example with `includeCalldata=true`

```bash

curl "https://app.swap-net.xyz/api/v1.0/swap\
?chainId=999\
&sellToken=0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb\
&buyToken=0x5555555555555555555555555555555555555555\
&sellAmount=10000000000\
&useRfq=true\
&includeCalldata=true\
&userAddress=0x11b86991c6218b36c1d19d4a2e9eb0ce3606eb49\
&slippageTolerance=0.01\
&router=swapnet-router\
&apiKey=<Your API Key>"
```

Response includes three more fields:

| Field name      | Description                                                   |
| --------------- | ------------------------------------------------------------- |
| `routerAddress` | Address of router contract                                    |
| `calldata`      | Calldata to settle the trade with the router contract         |
| `gasLimit`      | An estimate of upper bound of gas cost, used to config the TX |

```bash
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
  "tokens": [
    {
      "referenceId": 0,
      "address": "0x5555555555555555555555555555555555555555",
      "name": "Wrapped HYPE",
      "symbol": "WHYPE",
      "decimals": 18,
      "usdPrice": 58.75669929845798
    },
    {
      "referenceId": 1,
      "address": "0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb",
      "name": "USD₮0",
      "symbol": "USD₮0",
      "decimals": 6,
      "usdPrice": 1
    },
    {
      "referenceId": 2,
      "address": "0xfd739d4e423301ce9385c1fb8850539d657c296d",
      "name": "Kinetiq Staked HYPE",
      "symbol": "kHYPE",
      "decimals": 18,
      "usdPrice": 58.87736177694538
    }
  ],
  "routes": [
    {
      "address": "0xbd19e19e4b70eb7f248695a42208bc1edbbfb57d",
      "name": "ProjectxV3",
      "details": {
        "fee": 500
      },
      "fromTokens": [
        {
          "referenceId": 1,
          "amount": "2791261914"
        }
      ],
      "toTokens": [
        {
          "referenceId": 0,
          "amount": "47522431692592024830"
        }
      ]
    },
    {
      "address": "0x337b56d87a6185cd46af3ac2cdf03cbc37070c30",
      "name": "HyperswapV3",
      "details": {
        "fee": 500
      },
      "fromTokens": [
        {
          "referenceId": 1,
          "amount": "3589899622"
        }
      ],
      "toTokens": [
        {
          "referenceId": 0,
          "amount": "61123086039943950988"
        }
      ]
    },
    {
      "address": "0x09de938d3e788af06fd6d0819db9226150d4a0f3",
      "name": "ProjectxV3",
      "details": {
        "fee": 500
      },
      "fromTokens": [
        {
          "referenceId": 1,
          "amount": "3618838464"
        }
      ],
      "toTokens": [
        {
          "referenceId": 2,
          "amount": "61487009617903382007"
        }
      ]
    },
    {
      "address": "0x7a37621a2a490214f03e6ce7199aeca9b7c565cc",
      "name": "KittenswapV4",
      "fromTokens": [
        {
          "referenceId": 2,
          "amount": "16811648469114239656"
        }
      ],
      "toTokens": [
        {
          "referenceId": 0,
          "amount": "16846708650432222804"
        }
      ]
    },
    {
      "address": "0x5a716e045421b0977ca5bfb4f3394139c6a069bc",
      "name": "HybraV3",
      "details": {
        "fee": 25
      },
      "fromTokens": [
        {
          "referenceId": 2,
          "amount": "44675364046738217721"
        }
      ],
      "toTokens": [
        {
          "referenceId": 0,
          "amount": "44768806274520912829"
        }
      ]
    }
  ],
  "routerAddress": "0x616000e384Ef1C2B52f5f3A88D57a3B64F23757e",
  "calldata": "0x87395540000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000001c00000000000000000000000000000000000000000000000092337d00c9669a93100000000000000000000000011b86991c6218b36c1d19d4a2e9eb0ce3606eb490000000000000000000000000000000000000000000000000000000000000003000000000000000000000000b8ce59fc3717ada4c02eadf9682a9e934f625ebb00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000fd739d4e423301ce9385c1fb8850539d657c296d00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000001000000000000000000000000555555555555555555555555555555555555555500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000005000000000000000000000000000000000000000000000000000000000000000500000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000026000000000000000000000000000000000000000000000000000000000000003400000000000000000000000000000000000000000000000000000000000000420000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d7b317c000000000000000000000000009de938d3e788af06fd6d0819db9226150d4a0f3000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000030001f40000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a65f46da000000000000000000000000bd19e19e4b70eb7f248695a42208bc1edbbfb57d000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000030001f40000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d5f98566000000000000000000000000337b56d87a6185cd46af3ac2cdf03cbc37070c30000000000000000000000000000000000000000000000000000000000000000d00000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000030001f400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000e94ef935f24b3aa80000000000000000000000007a37621a2a490214f03e6ce7199aeca9b7c565cc000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000005a716e045421b0977ca5bfb4f3394139c6a069bc000000000000000000000000000000000000000000000000000000000000000f00000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000030000190000000000000000000000000000000000000000000000000000000000",
  "gasLimit": "1200000"
}
```
