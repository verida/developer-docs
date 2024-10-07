# Permissions

Advanced data permissions is built into the core of the Verida storage architecture, providing for many different ways data can be secured:

* **Private data:** Only the user can read and write (ie: Birth certificate document)
* **Public read and write:** Any user can read and write data (ie: Public comment thread)
* **Public read and restricted write:** Only the user can write, but the public can read data (ie: Public blog)
* **Restricted read and restricted write:** Only an approved list of users can read and write (ie: Private group chat)

All non-public data is encrypted using keys only accessible by the user(s) who have access to that particular data.

## Types of permissions[​](https://developers.verida.network/docs/client-sdk/permissions#types-of-permissions) <a href="#types-of-permissions" id="types-of-permissions"></a>

When a database / datastore is created, you specify the `read` and `write` permissions, out of a possible 3 options:

* `public` — Everyone has permission
* `users` — Only the specific list of users have permission
* `owner` — Only the owner has permission

Here are some real world examples:

* For private health data, set `read=owner`, `write=owner` so the data is completely private.
* For a public comment thread, set `read=public`, `write=public` so anyone can read and write comments.
* For a public blog post, set `read=public`, `write=owner` so the blog owner can publish new posts and the public can read them.
* For a private group chat, set `read=users`, `write=users` so only a pre-determined set of users can read from and write to the group chat.

By default, if no permissions are specified, the default permissions are set as `read=owner`, `write=owner`.

## Setting permissions[​](https://developers.verida.network/docs/client-sdk/permissions#setting-permissions) <a href="#setting-permissions" id="setting-permissions"></a>

Permissions are specified when opening a `database` or `datastore`:

```typescript
import { ContextInterfaces } from @verida/client-ts
const PERMISSIONS = ContextInterfaces.PermissionOptionsEnum

let permissions = {
  read: PERMISSIONS.OWNER,
  write: PERMISSIONS.OWNER
}

// Open a database for the current user
const privateDb = await context.openDatabase('private_data', {
  permissions: permissions
})

// Open a datastore
const contacts = await context.openDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json', {
  permissions: permissions
})

permissions = {
  read: PERMISSIONS.PUBLIC,
  write: PERMISSIONS.OWNER
}

// Open a database for another user (assuming you have access)
const publicDb = await context.openExternalDatabase('public_data', externalDid, {
  permissions: permissions
})

// Open a datastore for another user (assuming you have access)
const publicDatastore = await context.openExternalDatastore('https://common.schemas.verida.io/social/contact/v0.1.0/schema.json', externalDid, {
  permissions: permissions
})
```

### Restrict access to specific users[​](https://developers.verida.network/docs/client-sdk/permissions#restrict-access-to-specific-users) <a href="#restrict-access-to-specific-users" id="restrict-access-to-specific-users"></a>

When specifying the `users` permission type, you must also specify the list of valid user DID’s with `userList`:

```typescript
const permissions = {
  read: PERMISSIONS.USERS,
  readList: ['did:vda:polyamoy:0xe613A46C48f3805B05500bF7dBff00A1dd3Ba0e6', 'did:vda:....'],
  write: PERMISSIONS.USERS,
  writeList: ['did:vda:polyamoy:0xe613A46C48f3805B05500bF7dBff00A1dd3Ba0e6', 'did:vda:....'],
}

// Open a database
const restrictedDb = await const.openExternalDatabase('restricted_data', 'did:vda:polamoy:0xe613A46C48f3805B05500bF7dBff00A1dd3Ba0e6', {
  permissions: permissions
})
```

{% hint style="info" %}
&#x20;Assigning a database \`write=public\` currently results in \`read=public\` also being applied. This is an issue caused by CouchDB not supporting a user having write access, but not read access. It’s expected a modified version of CouchDB will be used to work around this current limitation in the future.
{% endhint %}
