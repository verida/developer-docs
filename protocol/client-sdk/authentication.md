# Authentication

There are multiple ways to connect a user’s account to the Verida protocol with different packages provided for different use cases.

Your application must install the appropriate account package and connect it to your client instance. The account package handles:

* Signing consent messages to unlock application contexts
* Creating a new decentralized identity if required
* Publishing endpoints to the account’s DID document for all the account’s application contexts

There are two ways to authenticate a user to your application:

## 1. Verida Wallet[​](https://developers.verida.network/docs/client-sdk/authentication#1-verida-wallet) <a href="#id-1-verida-wallet" id="id-1-verida-wallet"></a>

Designed _for secure web applications_

Your web application integrates the [Verida Connect SDK](https://developers.verida.network/docs/single-sign-on-sdk) which displays a QR code to a user. A user scans the QR code and is walked through an onboarding process of installing the Verida Wallet, creating a blockchain account and then authenticating with your application.

See the [Verida Connect SDK](https://developers.verida.network/docs/single-sign-on-sdk) for more details.

## 2. Private Key[​](https://developers.verida.network/docs/client-sdk/authentication#2-private-key) <a href="#id-2-private-key" id="id-2-private-key"></a>

Designed _for NodeJs and Mobile Apps_

An existing Verida account private key is used to authenticate a user to your application.

This approach is ideal for integrating the Verida protocol into a server side NodeJs application or embedding Verida into an existing mobile application using the [React Native client](https://developers.verida.network/docs/client-sdk/react-native)

### Example[​](https://developers.verida.network/docs/client-sdk/authentication#example) <a href="#example" id="example"></a>

```typescript
import { Network as NetworkClient } from '@verida/client-ts'
import { EnvironmentType } from '@verida/types';
import { AutoAccount } from '@verida/account-node'

const VERIDA_NETWORK = Network.BANKSIA // BANKSIA (testnet) or MYRTLE (mainnet)
const CONTEXT_NAME = 'My Application: Context Name'

// Configuration for the DID client
// `privateKey` must be a Polygon private key that has enough
// MATIC to perform a blockchain transaction to create your DID
// (If it doesn't exist)
const DID_CLIENT_CONFIG = {
    callType: 'web3',
    web3Config: {
        // Polygon private key
        privateKey: '0x...',
    }
}

// Create a connection to the network and open your context
const context = await NetworkClient.connect({
    context: {
        name: CONTEXT_NAME
    },
    client: {
        network: VERIDA_NETWORK
    },
    account: new AutoAccount({
        privateKey: '0x...' // or Verida mnemonic seed phrase
        network: VERIDA_NETWORK,
        didClientConfig: DID_CLIENT_CONFIG
    })
})
```

Note: Change `Network.BANKSIA` to `Network.MYRTLE` to use a mainnet network.

See the [@verida/account-node package](https://github.com/verida/verida-js/tree/main/packages/account-node) for more details.

### Web3Config[​](https://developers.verida.network/docs/client-sdk/authentication#web3config) <a href="#web3config" id="web3config"></a>

`DID_CLIENT_CONFIG.web3Config` supports additional options used when communicating with the blockchain. Here's the default configuration when using Verida Myrtle network (mainnet on the Polygon PoS blockchain):

```typescript
const DID_CLIENT_CONFIG = {
    callType: 'web3',
    web3Config: {
        // Polygon private key
        privateKey: '0x...',
        // Polygon network RPC URL
        rpcUrl: 'https://polygon-rpc.com/',
        eip1559Mode: 'fast',
        eip1559gasStationUrl: 'https://gasstation.polygon.technology/v2'
    }
}
```

Note: Only `privateKey` is required, the other values (`rpcUrl`, `eip1559xxx` will be populated with defaults in the protocol based on the Verida network selected)

### Generate private key[​](https://developers.verida.network/docs/client-sdk/authentication#generate-private-key) <a href="#generate-private-key" id="generate-private-key"></a>

Verida accounts use the same standard the same as Ethereum accounts, so Ethers.js can be used to generate a new seed phrase or private key.

```typescript
import { ethers } from 'ethers'
const wallet = new ethers.Wallet()
const privateKey = wallet.privateKey
```

## Advanced[​](https://developers.verida.network/docs/client-sdk/authentication#advanced) <a href="#advanced" id="advanced"></a>

The above examples initialize a connection to the Verida network and a single context. Sometimes it’s useful to connect to the network and then connect to multiple application contexts for the connected user. The code sample below generates a re-usable client instance, then uses that to open a specific application context.

In your application, include the dependency and create a new client network instance:

```typescript
import { Client } from '@verida/client-ts'
import { Network } from '@verida/types'
import { AutoAccount } from '@verida/account-node'

const VERIDA_NETWORK = Network.BANKSIA // BANKSIA (testnet) or MYRTLE (mainnet)
const CONTEXT_NAME = 'My Application: Context Name'

const DID_CLIENT_CONFIG = {
    callType: 'web3',
    web3Config: {
        privateKey: '0x...',
    }
}

// establish a network connection
const client = new Client({
    network: VERIDA_NETWORK
})

// create a Verida account instance that wraps the authorized Verida DID server connection
// The `AutoAccount` instance will automatically sign any consent messages
const account = new AutoAccount({
    privateKey: '0x...' // or Verida mnemonic seed phrase
    network: VERIDA_NETWORK,
    didClientConfig: DID_CLIENT_CONFIG
})

// Connect the Verida account to the Verida client
await client.connect(account)

// Open an application context (forcing creation of a new context if it doesn't already exist)
const context = await client.openContext(CONTEXT_NAME, true)

// Open a database
const database = await context.openDatabase('my_database')
```

### AccountNode Config[​](https://developers.verida.network/docs/client-sdk/authentication#accountnode-config) <a href="#accountnode-config" id="accountnode-config"></a>

The first parameter for `AutoAccount()` is an interface that meets the `AccountNodeConfig` definition:

```typescript
export interface AccountNodeConfig {
    privateKey: string;
    network: Network;
    didClientConfig: AccountNodeDIDClientConfig;
    options?: any;
    countryCode?: string;
}
```

* `privateKey` - Verida network private key for the account
* `network` - Verida network (`Network.BANKSIA` or `Network.MYRTLE`)
* `didClientConfig` - Instance of `AccountNodeDIDClientConfig`
* `countryCode` - (optional) Country to use for selecting storage and DID nodes on the network. If not specified, will choose random global nodes. If specified, will use nodes in that country. If not enough nodes are available in that country, it will fallback to selecting nodes in the same region as that country, then fallback to global nodes.
