# SDK

## SDK encoding for trade settlement

\<aside> 💡 Currently on L2, SwapNet uses [an forked version of Uniswap’s universal router](https://github.com/swapnet-xyz/universal-router) for trade settlement, which uses permit2 for delegation and approvals. As a result, the user need to set both **ERC20 allowanc**e to permit2 contract, and **permit2 allowance** to this universal router.

On Ethereum mainnet, SwapNet developed a more gas efficient router contract. Both the router contract code and corresponding SDK code will be open-sourced soon.

\</aside>

#### Prerequisites

1. The `userAccount` has enough balance (`sellAmount`) of `sellToken` to trade, and sufficient native token to pay gas cost.
2. The `userAccount` has enough allowance of `sellToken` to [Uniswap’s permit2 contract](https://docs.uniswap.org/contracts/permit2/overview) (`0x000000000022d473030f116ddee9f6b43ac78ba3`). The approval can be done through ERC20 standard method `sellToken.approve()`.
3. The `userAccount` has enough [permit2 allowance](https://docs.uniswap.org/contracts/permit2/reference/allowance-transfer#approve) assigned to our universal router contract. The approval could be done through a pre-approval (by calling `permit2.approve()`), or providing a signed permit (see [encode options](https://github.com/swapnet-xyz/swapnet-sdk/blob/6c8e4b481af3ffdc2ed7fa9fdbf2a48b9d2e8fd9/src/routers/universalRouter/index.ts#L65) in SDK).

#### Settlement

1. After querying SwapNet’s API, use [SwapNet SDK](https://www.npmjs.com/package/@swapnet-xyz/sdk) to [parse](https://github.com/swapnet-xyz/swapnet-sdk/blob/6c8e4b481af3ffdc2ed7fa9fdbf2a48b9d2e8fd9/src/parser.ts#L84) the response and [generate calldata](https://github.com/swapnet-xyz/swapnet-sdk/blob/6c8e4b481af3ffdc2ed7fa9fdbf2a48b9d2e8fd9/src/routers/universalRouter/index.ts#L61).
2. Wrap calldata into a Tx, sign it with your `userAccount` and submit to blockchain for settlement.
