# Table of contents

## Overview

* [What is SwapNet](README.md)
* [Competitive performance](overview/competitive-performance.md)
* [Modularized design](overview/modularized-architecture.md)

## Get started

* [Getting your API key](get-started/getting-your-api-key.md)
* [Using the REST API](get-started/using-the-rest-api.md)
* [Using the SDK](get-started/using-the-sdk.md)
* [Executing the trade](get-started/executing-the-trade.md)
* [Next steps](get-started/next-steps.md)

## Aggregator API

* [/swap](aggregator-api/swap.md)
* [/chains](aggregator-api/chains.md)
* [/chainInfo](aggregator-api/chaininfo.md)
* [/tokens](aggregator-api/tokens.md)

## Extension & Customization

* [DEX integration](extension-and-customization/dex-integration.md)
* [Customize the route](extension-and-customization/customize-the-route.md)
* [Use your own router contract](extension-and-customization/use-your-own-router-contract.md)
* [Chain support](extension-and-customization/chain-integration.md)

## Reference

* [API](reference/api/README.md)
  * ```yaml
    props:
      models: false
    type: builtin:openapi
    dependencies:
      spec:
        ref:
          kind: openapi
          spec: swapnet-openapi
    ```
* [Contract](reference/contract/README.md)
  * [Deployment addresses](reference/contract/deployment-addresses.md)
* [SDK](reference/sdk.md)
