# Table of contents

## Overview

* [What is SwapNet](README.md)
* [Aggregator as a service](overview/aggregator-as-a-service.md)

## Get started

* [Get API key](get-started/get-api-key.md)
* [Call API](get-started/call-api.md)
* [Settle the trade](get-started/settle-the-trade.md)
* [Example with SDK](get-started/example-with-sdk.md)

## Customization

* [DEX integration](customization/dex-integration.md)
* [Customize the route](customization/customize-the-route.md)
* [Use your own router contract](customization/use-your-own-router-contract.md)
* [Chain integration](customization/chain-integration.md)
* [Contact us](customization/contact-us.md)

## Reference

* [API](reference/api/README.md)
  * [The aggregator API](reference/api/readme.md)
  * ```yaml
    type: builtin:openapi
    props:
      models: false
    dependencies:
      spec:
        ref:
          kind: openapi
          spec: swapnet-openapi
    ```
* [Contract](reference/contract/README.md)
  * [Deployment addresses](reference/contract/deployment-addresses.md)
* [SDK](reference/sdk.md)
* [Liquidity source interfaces](reference/liquidity-source-interfaces.md)
