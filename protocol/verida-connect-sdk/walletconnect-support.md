# WalletConnect Support

The Verida Wallet supports [WalletConnect](https://walletconnect.com/) (v2) to connect crypto wallets with dApps. It enables seamless integration and communication between applications and supported blockchains.

Currently, the following blockchains and networks are supported:

* Ethereum Mainnet (mainnet) - ChainID `eip155:1`
* Ethereum Sepolia (testnet) - ChainID `eip155:11155111`
* Polygon PoS (mainnet) - ChainID `eip155:137`
* Polygon Amoy (testnet) - ChainID `eip155:80002`
* NEAR Testnet - ChainID `near:testnet`
* NEAR Mainnet - ChainID `near:mainnet`

## Verida Connect and WalletConnect Integration[​](https://developers.verida.network/docs/single-sign-on-sdk/wallet-connect#verida-connect-and-walletconnect-integration) <a href="#verida-connect-and-walletconnect-integration" id="verida-connect-and-walletconnect-integration"></a>

Verida Connect facilitates the connection between the application and the Verida Network, offering powerful storage capabilities. By integrating WalletConnect, Verida Connect enables a unified authentication flow for users, streamlining the process.

When users authorize a connection to your application in the Verida Wallet, they will also be prompted to establish a WalletConnect connection simultaneously.

### API[​](https://developers.verida.network/docs/single-sign-on-sdk/wallet-connect#api) <a href="#api" id="api"></a>

To combine Verida Connect and WalletConnect, you need to specify the `walletConnect` configuration when creating a `VaultAccount` instance. The `walletConnect` configuration requires the following property:

* `uri`: (required) URI of the WalletConnect request

### Example[​](https://developers.verida.network/docs/single-sign-on-sdk/wallet-connect#example) <a href="#example" id="example"></a>

Consider the following example code, which demonstrates the integration of WalletConnect with Verida Connect:

```typescript
// Example simplified for brevity
import { Network as NetworkClient } from '@verida/client-ts';
import AuthClient, { generateNonce } from "@walletconnect/auth-client";

// Initialise the AuthClient
const walletConnectClient = await AuthClient.init({
  projectId: process.env.PROJECT_ID,
  relayUrl: process.env.RELAY_URL,
  // ...
  // Refer to the WalletConnect AuthClient documentation for the full required configuration
});

// Set the listeners on the client
walletConnectClient.on("auth_response", () => {
  // logic to handle the response event
})

// Build the request URI
const walletConnectUri = await walletConnectClient.request({
  chainId: "eip155:5", // The Verida Wallet will reject blockchain network that are not supported
  // ...
  // Refer to the WalletConnect AuthClient documentation for the full required configuration
})

// Configure the `VaultAccount` instance with WalletConnect
const account = new VaultAccount({
  walletConnect: {
    uri: walletConnectUri,
  },
  // ...
  // Refer to the Verida Connect documentation for the full configuration
});

// Trigger the connection with Verida Connect
const context = await NetworkClient.connect({
  account: account,
  // ...
  // Refer to the Verida Connect documentation for the full configuration
});
```

Refer to the WalletConnect documentation and for using the `AuthClient` in your application.

For further guidance on using the `AuthClient` in your application, consult the WalletConnect [documentation](https://docs.reown.com/).
