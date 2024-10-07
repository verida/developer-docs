# Verida URIs

Verida supports URI's that reference a specific record on the network.

### URI helper library[​](https://developers.verida.network/docs/extensions/verida-uris#uri-helper-library) <a href="#uri-helper-library" id="uri-helper-library"></a>

The `@verida/helpers` library provides tools to create and load Verida URI's.

#### Installation[​](https://developers.verida.network/docs/extensions/verida-uris#installation) <a href="#installation" id="installation"></a>

```
yarn add @verida/helpers
```

#### Generating a URI[​](https://developers.verida.network/docs/extensions/verida-uris#generating-a-uri) <a href="#generating-a-uri" id="generating-a-uri"></a>

```typescript
import { buildVeridaUri } from '@verida/helpers'

const did = 'did:vda:polamoy:0x...'
const contextName: 'Verida: Vault'
const databaseName: 'test_db'
const rowId: '123456'

const uri = buildVeridaUri(did, contextName, databaseName, rowId)
console.log(`Generated URI: ${uri}`)
```

#### Fetching a URI[​](https://developers.verida.network/docs/extensions/verida-uris#fetching-a-uri) <a href="#fetching-a-uri" id="fetching-a-uri"></a>

```typescript
import { fetchVeridaUri } from '@verida/helpers'

const context = client.openContext('Verida: Vault')

try {
    const row = fetchVeridaUri(uri, context)
    console.log('Fetched row')
    console.log(row)
} catch (err) {
    // May throw document not found if it doesn't exist
    // or no permission to access
    console.log(err)
}
```

#### Parsing a URI[​](https://developers.verida.network/docs/extensions/verida-uris#parsing-a-uri) <a href="#parsing-a-uri" id="parsing-a-uri"></a>

```typescript
import { explodeVeridaUri } from '@verida/helpers'

const {
    did,
    contextName,
    dbName,
    id,
    query
} = explodeVeridaUri(uri)
```
