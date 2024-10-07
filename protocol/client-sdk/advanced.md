# Advanced

## Closing a database[​](https://developers.verida.network/docs/client-sdk/advanced#closing-a-database) <a href="#closing-a-database" id="closing-a-database"></a>

Databases should be closed when you have finished using them. This closes socket connections associated with syncing and cleans up other internal resources such as caching.

```typescript
await database.close()
```

## Datastore v Database[​](https://developers.verida.network/docs/client-sdk/advanced#datastore-v-database) <a href="#datastore-v-database" id="datastore-v-database"></a>

You can access the underlying `database` object from a `datastore` object:

```typescript
const db = await datastore.getDb()
```

The Verida database object is a wrapper around a native [PouchDb instance](https://pouchdb.com/api.html). You can fetch this PouchDb instance from a `database` object:

```typescript
const pouchDb = await db.getDb()
```

## Database info[​](https://developers.verida.network/docs/client-sdk/advanced#database-info) <a href="#database-info" id="database-info"></a>

Additional information about a database can be retrieved:

```typescript
const dbInfo = await database.info()
console.log(dbInfo)
```

Here’s example output for an encrypted database:

```typescript
{
  type: 'VeridaDatabase',
  privacy: 'encrypted',
  did: 'did:vda:polyamoy:0x6B2a1bE81ee770cbB4648801e343E135e8D2Aa6F',
  dsn: 'https://<username>:<password>@db.banksia.verida.io:5984',
  storageContext: 'Verida Test: Test Application 1',
  databaseName: 'SyncTestDb',
  databaseHash: '<databaseHash>',
  encryptionKey: Uint8Array(32) [
    ...
  ],
  sync: {
    canceled: true,
    pull: { status: 'cancelled', canceled: undefined },
    push: { status: 'cancelled', canceled: undefined }
  }
}
```
