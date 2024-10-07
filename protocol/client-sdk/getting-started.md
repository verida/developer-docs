# Getting Started

In this article, we walk you through the process setting up the Verida Client SDK and guide you through the process of initializing and using the library in your applications.

You can interactively use the Verida client library in your browser using the [Verida Web Sandbox](https://web-sandbox.demos.testnet.verida.io/).

## Installation[​](https://developers.verida.network/docs/client-sdk/getting-started#installation) <a href="#installation" id="installation"></a>

Start by installing the Verida client protocol library and Verida Wallet authentication method:

```bash
npm install @verida/client-ts @verida/account-web-vault
```

You may receive compilation warnings when using typescript regarding `pouchdb`. If that happens, add the `pouchdb-core` type definitions:

```bash
npm install --dev @types/pouchdb-core
```

## Authentication[​](https://developers.verida.network/docs/client-sdk/getting-started#authentication) <a href="#authentication" id="authentication"></a>

Initialize a connection to the Verida network using a private key stored on the user’s mobile device using the Verida Wallet:

```typescript
import { Network as NetworkClient } from '@verida/client-ts';
import { Network } from '@verida/types';
import { VaultAccount } from '@verida/account-web-vault';

const VERIDA_NETWORK = Network.BANKSIA; // BANKSIA (testnet) or MYRTLE (mainnet)
const CONTEXT_NAME = 'My Application Context Name';

// The LOGO_URL should be a 170x170 PNG file
const LOGO_URL = "https://assets.verida.io/verida_login_request_logo_170x170.png";

const account = new VaultAccount({
    logoUrl: LOGO_URL
});

const context = await NetworkClient.connect({
    client: {
        network: VERIDA_NETWORK,
    },
    account: account,
    context: {
        name: CONTEXT_NAME,
    },
});
```

Note: Change `Network.BANKSIA` to `Network.MYRTLE` to use a mainnet network.

* `CONTEXT_NAME`: A string representing your decentralized application. By convention prefix it with your company name. ie: `Verida: My Application`.

[Learn more](getting-started.md#authentication) about different authentication methods.

## How to Use[​](https://developers.verida.network/docs/client-sdk/getting-started#how-to-use) <a href="#how-to-use" id="how-to-use"></a>

You can now verify the user has connected successfully to your application:

```typescript
const did = await account.did();
console.log('User is connected with Verida Account DID: ' + did);
```

You can open a database, save a record and then fetch it:

```typescript
const db = await context.openDatabase('test_db');
const item = await db.save({
    hello: 'world',
});
const items = await db.getMany();
console.log(items);
```

Learn more about building applications with decentralized [databases, datastores ](data.md)and [messaging](../concepts/messaging.md).
