# Database Node

**Verida is the private data storage layer for the self-sovereign data economy. The first self-sovereign database network for storing private data.**

We believe it’s a fundamental human right that individuals have ownership and control over their personal data. We believe everyone has a right to control their identity through decentralization, giving netizens control and self-sovereignty over how they interact with the digital world.

Storage Nodes are where this data is stored.

## Current Status

{% hint style="info" %}
Currently operators can try out deployment and operating a node but it requires manual approval to be added to the Verida Testnet network.
{% endhint %}

Currently we are taking applications for storage node operators. [Read about storage node operation](https://news.verida.io/decentralizing-the-verida-network-become-a-node-operator-e31a6a31daf1) and [apply to be a Verida Storage Node operator.](https://www.verida.network/storage-node-providers)

Safety and stability of the Verida mainnet is paramount importance so we are currently progressively on-boarding node operators to our testnet to verify the stability.

To learn more join the node operator community in the [#storage-nodes channel on the Verida Discord](https://discord.gg/AzcRJvvWEF).

## Quick Links

* [Apply](https://www.verida.network/storage-node-providers) to be a Storage Node Operator.
* Join the [Verida Discord](https://discord.gg/AzcRJvvWEF).
* [Quickstart](setup.md) for Storage Node Operators
* [Storage Node Operator Documentation](operations.md)

***

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
