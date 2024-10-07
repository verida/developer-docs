# Data

### Introduction[​](https://developers.verida.network/docs/client-sdk/data#introduction) <a href="#introduction" id="introduction"></a>

The Verida Client SDK supports two types of data constructs:

* Databases with no schema (`database`)
* Databases with an enforced schema (`datastore`)

It’s recommended to use `datastores` wherever possible to ensure your application creates validated data that can easily be shared between applications. You can read more about creating or using existing datastore [schemas](https://developers.verida.network/docs/concepts/schemas).

### Databases[​](https://developers.verida.network/docs/client-sdk/data#databases) <a href="#databases" id="databases"></a>

All databases in the Verida protocol are _**User Databases**_. They are owned by a specific Verida account that controls the database permissions. These databases can be private (encrypted using private keys only known by the user) or public (not encrypted).

As such, application owners don’t have access to this data. This ensures user data is private, owned and controlled entirely by the user.

Applications can have an unlimited number of databases.

As applications have per-user databases, unique database names are generated based on a hash of:

* Account owner's `did`
* Application name
* Human readable database name

There is no concept of a central database, however many applications need to access aggregated data. Traditional API’s and databases can be used for this purpose, however it must be made clear to a user when their data is being duplicated and used in that way. Also see the [shared databases](about:blank#shared-databases) section below for an alternative approach.

We have some early thoughts on how to provide privacy preserving aggregated data for applications, but they are not a current priority.

💡 It is \*\*NOT\*\* recommended using databases directly. You should use datastores to ensure your data is validated against a schema and can be interoperable with other applications.

#### Opening Databases[​](https://developers.verida.network/docs/client-sdk/data#opening-databases) <a href="#opening-databases" id="opening-databases"></a>

**User owned database**[**​**](https://developers.verida.network/docs/client-sdk/data#user-owned-database)

You can open a new database owned by the currently connected account:

```typescript
const options = {}
const db = await context.openDatabase('test_db', options)
```

There are many options you can provide when opening a database. These include:

* Database permissions ([see below](about:blank#permissions))
* Encryption key (`options.encryptionKey` as a string)

**External database**[**​**](https://developers.verida.network/docs/client-sdk/data#external-database)

You can open an `external` database which is owned by different Verida account or is owned by the same Verida account in a different application context.

Here we are opening a database with `PUBLIC` read and write permissions owned by another account:

```typescript
import { ContextInterfaces } from @verida/client-ts

const otherAccountDid = 'did:vda:polamoy:0x5e8fdBaAA46E4Bfa914e206e9415Aa05d4CC6722'
const options = {
  permissions: {
    read: ContextInterfaces.PermissionOptionsEnum.PUBLIC,
    write: ContextInterfaces.PermissionOptionsEnum.PUBLIC
  }
}

const db = await context.openExternalDatabase('test_external_db', otherAccountDid, options)
```

**External database with external context**[**​**](https://developers.verida.network/docs/client-sdk/data#external-database-with-external-context)

You can also open an external database using the `Client` class in the `@verida/client-ts` package .

```typescript

import { Client } from '@verida/client-ts';
import { Network } from '@verida/types';

const clientConfig = {
  network: Network.BANKSIA,
}

const context = await new Client(clientConfig).openExternalContext(
  'contextName',
  'did:vda:polyamoy:0x5e8fdBaAA46E4Bfa914e206e9415Aa05d4CC6722'
);

const db = await context.openExternalDatabase('test_external_db')


```

#### Using Databases[​](https://developers.verida.network/docs/client-sdk/data#using-databases) <a href="#using-databases" id="using-databases"></a>

Open a user database and fetch some rows:

```typescript
const db = await context.openDatabase('test_db')
const item = await db.save({
  hello: 'world'
})
const items = await db.getMany()
console.log(items)
```

The database will be created if it doesn’t exist.

#### Closing Databases[​](https://developers.verida.network/docs/client-sdk/data#closing-databases) <a href="#closing-databases" id="closing-databases"></a>

You can close an open database:

```typescript
await db.close()
```

This closes any connection to remote CouchDB server(s), releases sockets and disconnects event listeners.

You can optionally specify an option to `clearLocal`:

```typescript
await db.close({
  clearLocal: true
})
```

In a Node.js environment,`clearLocal` will delete any local files created to cache a remote database. This occurs when connecting to remote encrypted databases and a local, decrypted, cache is created.

#### Deleting Databases[​](https://developers.verida.network/docs/client-sdk/data#deleting-databases) <a href="#deleting-databases" id="deleting-databases"></a>

Databases can be permanently deleted from the Verida network:

```typescript
await db.destroy()
```

You can optionally specify an option to only delete local cache copies of databases:

```typescript
await db.close({
  localOnly: true
})
```

### Datastores[​](https://developers.verida.network/docs/client-sdk/data#datastores) <a href="#datastores" id="datastores"></a>

In a world where users own their own data, it’s important their data is portable between applications. Otherwise we end up with the current situation of data silos, where user data is scattered across lots of different applications.

Verida solves this problem by creating databases with a defined schema, called `datastores`. This ensures data interoperability between applications (and users).

Using schemas also ensures data is validated before saving. This ensures data is of the correct format and required fields are defined.

See [schemas](https://developers.verida.network/docs/concepts/schemas) to learn about the existing schemas or how to build your own.

#### Opening Datastores[​](https://developers.verida.network/docs/client-sdk/data#opening-datastores) <a href="#opening-datastores" id="opening-datastores"></a>

**User owned datastore**[**​**](https://developers.verida.network/docs/client-sdk/data#user-owned-datastore)

Lets demonstrate by opening a datastore using the [https://schemas.verida.io/social/contact/schema.json](https://common.schemas.verida.io/social/contact/v0.1.0/schema.json) schema, saving a row and fetching the results:

```typescript
const contacts = await context.openDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json')
const contact = {
  lastName: 'Smith',
  email: 'john@smith.com'
}
let success = contacts.save(contact)

if (!success) {
  console.error(contacts.errors);
} else {
  console.log("Contact saved");
}

contact.firstName = 'John'
success = contacts.save(contact)

const contactList = await contacts.getMany()
console.log(contactList)
```

In the above example, the `firstName` field is required in the `social/contact` schema so the new record fails to save. The validation errors can be found in `contacts.errors`.

The record can be saved succesfully after the record has the required `firstName` field added.

**External datastore**[**​**](https://developers.verida.network/docs/client-sdk/data#external-datastore)

Just like databases, it’s also possible to open an external datastore:

```typescript
import { ContextInterfaces } from @verida/client-ts

const otherAccountDid = 'did:vda:polyamoy:0x5e8fdBaAA46E4Bfa914e206e9415Aa05d4CC6722'
const options = {
  permissions: {
    read: ContextInterfaces.PermissionOptionsEnum.PUBLIC,
    write: ContextInterfaces.PermissionOptionsEnum.PUBLIC
  }
}
const datastore = await context.openExternalDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json', otherAccountDid, options)
```

### CRUD operations[​](https://developers.verida.network/docs/client-sdk/data#crud-operations) <a href="#crud-operations" id="crud-operations"></a>

#### Creating data[​](https://developers.verida.network/docs/client-sdk/data#creating-data) <a href="#creating-data" id="creating-data"></a>

Data is created by calling the `save()` method. If the `save()` fails, you can find an array of errors in the `.errors` property.

```typescript
const contacts = await app.openDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json')
const contact = {
  firstName: 'John',
  email: 'john@smith.com'
}
let success = await contacts.save(contact)

if (!success) {
  console.error(contacts.errors)
} else {
  console.log("Contact saved")
}
```

There is an optional second `save()` parameter called `options` that produces a set of options that are passed through to [PouchDB.put()](https://pouchdb.com/api.html#create\_document).

There are also two advanced options available (`forceInsert` and `forceUpdate`). See the API docs for details.

#### Updating data[​](https://developers.verida.network/docs/client-sdk/data#updating-data) <a href="#updating-data" id="updating-data"></a>

Data is updated by also updating an existing record and calling the `save()` method:

```typescript
const recordId = 'abc123'
const row = await contacts.get(recordId)
row.firstName = 'Jane'

await contacts.save(row)
```

It’s critical that you fetch the existing record and update its values before saving. The underlying database expects data updates to have two properties set:

* `_id`: The unique record identifier (string)
* `_rev`: A unique revision identifier for the current row (string)

The `_id` field is used to detect we are expecting to update an existing record. The `_rev` field is used to match against the currently known latest revision in the database to ensure we don’t override with a stale version of the data. This is critical in a decentralized environment and also allows for some fancy merging techniques in the future.

#### Deleting data[​](https://developers.verida.network/docs/client-sdk/data#deleting-data) <a href="#deleting-data" id="deleting-data"></a>

You can delete a record using the full record or just its `_id`:

```typescript
const recordId = 'abc123'
const row = await contacts.get(recordId)

// option 1
await contacts.delete(row)

// option 2
await contacts.delete(row._id)
```

In order to delete a row, the revision (`_rev`) is required. If you delete just using the record ID, behind the scenes the latest `_rev` value is fetched from the database to enable the delete.
