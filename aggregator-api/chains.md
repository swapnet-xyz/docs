# /chains

The `/chains` endpoint returns a list of blockchain networks supported by the SwapNet aggregator.

## Endpoint

```
GET /api/v1.0/chains
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `apiKey` | string | Yes | Your API key |

## Example Request

```bash
curl "https://app.swap-net.xyz/api/v1.0/chains?apiKey=<Your API Key>"
```

## Response

Returns an array of supported blockchain networks, each containing:

- `chainId`: Unique identifier for the blockchain network
- `name`: Human-readable name of the blockchain network

## Example Response

*The following response shows the list of supported chains as of September 2025:*

```json
[
  {
    "chainId": 999,
    "name": "HyperEvm"
  },
  {
    "chainId": 81457,
    "name": "Blast"
  },
  {
    "chainId": 747474,
    "name": "Katana"
  },
  {
    "chainId": 130,
    "name": "Unichain"
  },
  {
    "chainId": 1,
    "name": "Ethereum"
  },
  {
    "chainId": 42161,
    "name": "ArbitrumOne"
  },
  {
    "chainId": 8453,
    "name": "Base"
  },
  {
    "chainId": 101,
    "name": "Sui"
  },
  {
    "chainId": 900,
    "name": "Solana"
  },
  {
    "chainId": 137,
    "name": "Polygon"
  },
  {
    "chainId": 10,
    "name": "Optimism"
  },
  {
    "chainId": 56,
    "name": "Bsc"
  }
]
```
