# Run a Node

The Verida protocol provides an open-source [Storage Node](https://github.com/verida/storage-node) server that provides encrypted database storage for one or more Verida accounts.

The server provides a middleware that connects decentralized identities on the Verida network with authenticated users in a CouchDB database cluster.

## Use cases[​](https://developers.verida.network/docs/infrastructure/storage-node#use-cases) <a href="#use-cases" id="use-cases"></a>

1. Application developers can provide their own default storage nodes for their users
2. End users can have increased privacy and control by hosting storage nodes for their own personal data
3. Enterprise users can provide highly secure, private storage for all it's employees

## Key features[​](https://developers.verida.network/docs/infrastructure/storage-node#key-features) <a href="#key-features" id="key-features"></a>

* Ensuring all API requests come from verified Verida network users (via user-signed messages)
* Managing database users, linking them to valid DID's
* Managing permissions for individual databases
* Adding a second layer of security by managing per-database ACL validation rules
* Providing applications with user's database connection strings

### Learn more[​](https://developers.verida.network/docs/infrastructure/storage-node#learn-more) <a href="#learn-more" id="learn-more"></a>

Learn more at the [Storage Node GitHub repo](https://github.com/verida/storage-node).
