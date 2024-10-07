# Events

Your application can listen to events for when data changes in your application.

## Data changes[​](https://developers.verida.network/docs/client-sdk/events#data-changes) <a href="#data-changes" id="data-changes"></a>

Once a `datastore` or `database` is opened, you can listen to changes to automatically update your application:

```typescript
const filter = {
    status: 'active'
}

const listener = await database.changes(function(row) {
  console.log("Row changed", row)
}, filter)
```

Try this out with your own profile data in the browser in our [tutorial](https://developers.verida.network/docs/tutorial/events).

Options:

* `filter`: An optional JSON object. The listener will only raise events if the data in the object matches the supplied filter.

You can cancel the event listener:

```typescript
listener.cancel()
```

Similarly you can call:

```typescript
const listener = await datastore.changes(function(row) { ... })
```

## Inbox messages[​](https://developers.verida.network/docs/client-sdk/events#inbox-messages) <a href="#inbox-messages" id="inbox-messages"></a>

You can be notified when a new inbox message arrives for your application:

```typescript
const messaging = await context.getMessaging()
messaging.onMessage(function(message) { console.log('New message!', message)})
```

## Database sync changes[​](https://developers.verida.network/docs/client-sdk/events#database-sync-changes) <a href="#database-sync-changes" id="database-sync-changes"></a>

Data is automatically synchronized from remote encrypted servers to the local client. It’s possible to listen to events related to this syncing behavior:

```typescript
const listener = database.onSync('error', (err) => { console.log(err) })
listener.cancel()
```

A full list of events is available via the [PouchDB Documentation](https://pouchdb.com/api.html#sync)
