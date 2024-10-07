# Messaging

Every application has a built-in inbox for receiving messages and an outbox for sending messages. This allows users and applications to send data between each other knowing nothing than the other user’s DID and application name.

Learn more about the messaging architecture.

## Open messaging[​](https://developers.verida.network/docs/client-sdk/messaging#open-messaging) <a href="#open-messaging" id="open-messaging"></a>

You can simply open the messaging capabilities for the currently connected application `context` as follows:

```typescript
const messaging = await context.getMessaging()
```

## Fetching messages (Inbox)[​](https://developers.verida.network/docs/client-sdk/messaging#fetching-messages-inbox) <a href="#fetching-messages-inbox" id="fetching-messages-inbox"></a>

### Get messages[​](https://developers.verida.network/docs/client-sdk/messaging#get-messages) <a href="#get-messages" id="get-messages"></a>

You can retrieve the 20 most recent messages:

```typescript
const messages = await messaging.getMessages()
```

Alternatively, specify optional `filter` and `options` parameters:

```typescript
const filter = {
    type: 'inbox/dataRequest'
}
const options = {
  limit: 20,
  skip: 0,
  sort: [{ sentAt: 'desc' }]
}
const messages = await messaging.getMessages(filter, options)
```

### Listen for messages[​](https://developers.verida.network/docs/client-sdk/messaging#listen-for-messages) <a href="#listen-for-messages" id="listen-for-messages"></a>

Your application can register a callback function to listen for new inbox messages:

```typescript
const listener = await messaging.onMessage(function(inboxEntry) {
    console.log('New inbox message received', inboxEntry)
})
```

The `inboxEntry` object utilizes the [inbox/entry schema](https://core.schemas.verida.io/inbox/entry/latest/schema.json). The two most important properties are:

* `type` — The type of inbox entry. This references a full schema URL.
* `data` — The data contained in the inbox entry. This will be an object adhering to the schema specified in `type`. [See a list of currently supported inbox types](https://github.com/verida/schemas/tree/master/schemas/inbox/type).

You can stop listening:

```typescript
listener.cancel()
```

## Sending messages (Outbox)[​](https://developers.verida.network/docs/client-sdk/messaging#sending-messages-outbox) <a href="#sending-messages-outbox" id="sending-messages-outbox"></a>

Your application can send messages to other accounts on the Verida network.

This example sends a contact record to a user’s Verida Wallet:

```typescript
const did = 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F'
const type = 'inbox/type/dataSend'

// Generate an inbox message containing an array of data
const data = {
    data: [
        {
            firstName: 'Verida',
            lastName: 'Example',
            email: 'verida.example@example.com',
            schema: 'https://common.schemas.verida.io/social/contact/v0.1.0/schema.json'
        }
    ]
}
const message = 'Sending you a contact'
const config = {
    recipientContextName: 'Verida: Vault'
}

messaging.send(did, type, data, message, config)
```

This could be used in two scenarios:

* A user sending their own data from one application they control to another
* A user sending data to another user within the same application

{% hint style="info" %}
The Verida account must have already logged in and created the application context before you can send it an inbox message. For example, assume you are sending an inbox message to an application context called “Verida: Documents”. If the recipient account has never logged into that context the inbox message will fail because that account has no inbox available.
{% endhint %}

### Opening your app[​](https://developers.verida.network/docs/client-sdk/messaging#opening-your-app) <a href="#opening-your-app" id="opening-your-app"></a>

It can be helpful if the Wallet opens your application in a web browser after the user accepts a message. You can enable this by providing an optional parameter to the `config`.

ie:

```typescript
const config = {
    recipientContextName: 'Verida: Vault',
    openUrl: 'https://www.myapp.com/custom-page'
}
```

### Setting avatar and name[​](https://developers.verida.network/docs/client-sdk/messaging#setting-avatar-and-name) <a href="#setting-avatar-and-name" id="setting-avatar-and-name"></a>

When sending an inbox message, the sender avatar and name are automatically loaded from the public profile of the sending Verida Account. You will need to set these for your account so they can be loaded by applications across the network (including the Verida Wallet).

You could add your Verida Account to the Verida Wallet and use the mobile app to set a profile avatar and name. This will then be the default for every application used by the sending Verida Account. Alternatively, you could manually set the profile information for the application context sending the inbox message. See [Account Profiles](broken-reference) for more information on how to achieve this.

## Built-in Message Types[​](https://developers.verida.network/docs/client-sdk/messaging#built-in-message-types) <a href="#built-in-message-types" id="built-in-message-types"></a>

See [core schemas repository](https://github.com/verida/schemas-core) for details on each supported inbox message type.

* [dataSend](https://github.com/verida/schemas-core/tree/develop/inbox/type/dataSend) — Send one or many pieces of data to a user
* [dataRequest](https://github.com/verida/schemas-core/tree/develop/inbox/type/dataRequest) — Request data from a user, supports optional filters and conditions around user’s selecting data
* [message](https://github.com/verida/schemas-core/tree/develop/inbox/type/message) — Send a generic message / notification to a user

## Examples[​](https://developers.verida.network/docs/client-sdk/messaging#examples) <a href="#examples" id="examples"></a>

### Sending data[​](https://developers.verida.network/docs/client-sdk/messaging#sending-data) <a href="#sending-data" id="sending-data"></a>

Data can be sent to an account (see Outbox example above)

### Requesting data[​](https://developers.verida.network/docs/client-sdk/messaging#requesting-data) <a href="#requesting-data" id="requesting-data"></a>

Data can be requested from an account:

```typescript
const did = 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F'
const type = 'inbox/type/dataRequest'

// Generate an inbox message containing an array of data
const data = {
    requestSchema: 'https://common.schemas.verida.io/social/contact/v0.1.0/schema.json',
    filter: {},
    userSelect: true
}
const message = 'Please share your contact details'
const config = {
    recipientContextName: 'Verida: Vault'
}

messaging.send(did, type, data, message, config)
```

Options:

* `filter`: A JSON filter to apply to the search when locating suitable data to share
* `userSelect`: Boolean value indicating if the user can select the data to be returned. If `false` all matching data will be returned.
* `userSelectLimit`: Integer limiting how many records a user can select.
