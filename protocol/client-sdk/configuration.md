# Configuration

## Client configuration[​](https://developers.verida.network/docs/client-sdk/configuration#client-configuration) <a href="#client-configuration" id="client-configuration"></a>

An options parameter can be passed to the `Client` object:

```typescript
import { ClientConfig, Network } from '@verida/types'

const config: ClientConfig = {
  network: Network.BANKSIA,
  didClientConfig: {
    rpcUrl: "https://<your-custom-polygon-url>"
  }
}

const client = new Client(config)
```

Some key options are:

* `network` — Sets default configuration settings for the specified environment. Options are; `local`, `banksia`, `myrtle`
* `didClientConfig.rpcUrl` - While optional you may want to set your own RPC instead of using the default RPC of the Client (free and public but may not be reliable)

## Environment Variables[​](https://developers.verida.network/docs/client-sdk/configuration#environment-variables) <a href="#environment-variables" id="environment-variables"></a>

As you deploy your application on different environments (Production, test, staging), you may want to set a `VERIDA_NETWORK` and `POLYGON_RPC_URL` environment variable in your application to choose the network and RPC to use appropriately. Note that the client does not read the environment variables and so there is no syntax imposed on the naming of the variables. It is up to you to pass the environment variable to the `ClientConfig`.

For example:

```bash
VERIDA_NETWORK=banksia
POLYGON_RPC_URL=https://<your-custom-polygon-url>
```

{% hint style="info" %}
&#x20;Refer to the documentation of your framework, if any, for how to set and use encironment variables.
{% endhint %}
