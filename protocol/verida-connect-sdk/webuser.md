# WebUser

`WebUser` is a helper class that simplifies using the Verida Network in a web application. An instance of the class represents a User with a set of common methods available such as `connect`, `getDid`, `getPublicProfile`, etc.

## Usage[​](https://developers.verida.network/docs/single-sign-on-sdk/web-user#usage) <a href="#usage" id="usage"></a>

```typescript
// Example simplified for brevity. Notably, async/await should be handled appropriately

import { Network } from '@verida/types'
import { WebUser } from '@verida/web-helpers'

// Your application context name
const CONTEXT_NAME = '<Company name>: <Application name>'
const VERIDA_NETWORK = Network.BAKSIA

// Your logo URL (appears in the Wallet)
const LOGO_URL = ''

// Create a singleton instance of your user with network connection configuration
// Return this singleton instance so there is only ever one user object
// in your application.
export const user = new WebUser({
    accountConfig: {
        request: {
            logoUrl: LOGO_URL
        },
        network: VERIDA_NETWORK
    },
    clientConfig: {
        network: VERIDA_NETWORK
    },
    contextConfig: {
        name: CONTEXT_NAME
    },
    debug: true // Provides logs in the console
})
// Note: `accountConfig`, `clientConfig`, `contextConfig` are all configuration objects that match their respective `account-web-vault`, `client` and `context` configuration objects.

// Bind some event listeners
user.on('connected',() => { console.log('User connected!') })
user.on('disconnect',() => { console.log('User disconnected!') })
user.on('profileChanged',(newProfile) => { console.log('Profile changed!', newProfile) })

// Automatically connect the user if there's an existing local session
await autoConnectExistingSession()

// Check if the user is connected. Note: `autoConnectExistingSession` also returns the connection status
const isConnected = user.isConnected()

if (!isConnected) {
    // Use the `connect` method to explicitly prompt the user to connect with the Verida Wallet
    const success = await user.connect()
    if (!success) {
        throw Error('User cancelled the connection')
    }
}

// Get the DID of the user
const did = user.getDid()
console.log(`Logged in with ${did}`)

// Get the public name, avatar and description from the user's profile
const publicProfile = await user.getPublicProfile()
console.log(`Public profile:`, publicProfile)

// Send a message from the connected user to the specified DID (in this example to itself)
await user.sendMessage(did, 'Hello!')

// Open a private encrypted user database and save a row
const database = await user.openDatabase('test_db')
const row = await database.save({'hello': 'world'})
console.log(row)

// Open a private encrypted user datastore and save a row
const datastore = await user.openDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json')
const row = await datastore.save({
    firstName: 'Jane',
    lastName: 'Smith',
    email: 'jane_smith@example.com'
})
console.log(row)

// Disconnect the user
await user.disconnect()
```

{% hint style="warning" %}
It is recommended to avoid multiple instances of `WebUser` to prevent side effects on shared resources.
{% endhint %}

```typescript
import { WebUser, WebUserConfig } from '@verida/web-helpers'

// Define the configuration
const CONFIG: WebUserConfig = {
  accountConfig: {
    // ...
  },
  clientConfig: {
    // ...
  },
  contextConfig: {
    // ...
  },
}

// Implement a singleton class
export class SingletonWebUser extends WebUser {
  private static instance: VeridaUser

  private constructor(config: WebUserConfig) {
    super(config)
  }

  public static getInstance() {
    if (this.instance) {
      return this.instance
    }
    this.instance = new VeridaUser(CONFIG)
    return this.instance
  }
}

const user = SingletonWebUser.getInstance()
```
