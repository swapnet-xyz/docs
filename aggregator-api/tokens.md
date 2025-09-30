# /tokens

The `/tokens` endpoint returns a list of example tokens for trading on the specified chain. Note that this is not a full list of tradable tokens, but rather a curated list of popular tokens for reference.

## Endpoint

```
GET /api/v1.0/tokens
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chainId` | integer | Yes | Integer ID of the blockchain |
| `apiKey` | string | Yes | Your API key |

## Example Request

```bash
curl "https://app.swap-net.xyz/api/v1.0/tokens?chainId=1&apiKey=<Your API Key>"
```

## Response

Returns an array of token information objects, each containing:

- `address`: Token contract address
- `name`: Token name
- `symbol`: Token symbol
- `decimals`: Number of decimal places for the token (0-18)
- `metadata`: Additional token metadata including logo URI

## Example Response

```json
[
  {
    "address": "0x853d955acef822db058eb8505911ed77f175b99e",
    "name": "Frax",
    "symbol": "FRAX",
    "decimals": 18,
    "metadata": {
      "logoURI": "https://example.com/frax.png"
    }
  },
  {
    "address": "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984",
    "name": "Uniswap",
    "symbol": "UNI", 
    "decimals": 18,
    "metadata": {
      "logoURI": "https://example.com/uni.png"
    }
  }
]
```
