# Table of contents

## Overview

* [What is SwapNet](README.md)
* [Modularized architecture](overview/aggregator-as-a-service.md)
* [Contact us](overview/contact-us.md)

## Get started

* [Get API key](get-started/get-api-key.md)
* [Call API](get-started/call-api.md)
* [Settle the trade](get-started/settle-the-trade.md)
* [Example with SDK](get-started/example-with-sdk.md)

## Aggregator API

* [/chains](aggregator-api/chains.md)
* [/chainInfo](aggregator-api/chaininfo.md)
* [/tokens](aggregator-api/tokens.md)
* [/swap](aggregator-api/api.md)

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
