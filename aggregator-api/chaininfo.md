# /chainInfo

The `/chainInfo` endpoint returns detailed information about a specific blockchain network, including supported routers, liquidity sources, and key token addresses.

## Endpoint

```
GET /api/v1.0/chainInfo
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `chainId` | integer | Yes | Integer ID of the blockchain |
| `apiKey` | string | Yes | Your API key |

## Example Request

```bash
curl "https://app.swap-net.xyz/api/v1.0/chainInfo?chainId=1&apiKey=<Your API Key>"
```

## Response

Returns detailed information about the specified blockchain network:

- `chainId`: Unique identifier for the blockchain network
- `name`: Human-readable name of the blockchain network
- `usdTokenAddress`: Address of the primary USD-pegged token on this chain
- `wrappedNativeTokenAddress`: Address of the wrapped native token (e.g., WETH, WHYPE)
- `routers`: Array of available router contracts and their configurations

### Router Information

Each router object contains:

- `name`: Router contract unique name (e.g., "swapnet-router", "universal-router", "default-router")
- `liquiditySources`: Array of supported liquidity sources for this router
- `deployedAddress`: Deployed address of the router contract
- `tokenProxyAddress`: Address of the token proxy contract (if applicable)
- `hasEncoder`: Whether the router has an available encoder for calldata generation

## Example Response

```json
{
  "chainId": 999,
  "name": "HyperEvm",
  "usdTokenAddress": "0xb8ce59fc3717ada4c02eadf9682a9e934f625ebb",
  "wrappedNativeTokenAddress": "0x5555555555555555555555555555555555555555",
  "routers": [
    {
      "name": "swapnet-router",
      "liquiditySources": [
        "HyperswapV2",
        "HyperswapV3",
        "ProjectxV3",
        "HybraV3",
        "KittenswapV4",
        "GliquidV4",
        "UpheavalV3"
      ],
      "deployedAddress": "0x616000e384Ef1C2B52f5f3A88D57a3B64F23757e",
      "hasEncoder": true
    }
  ]
}
```
