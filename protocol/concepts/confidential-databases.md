# Confidential Databases

In today’s digital world, data storage is a crucial component for all applications, regardless of their scope or complexity. While traditional Web2 offers a wealth of storage options tailored to various use cases, such as SQL, NoSQL, File storage, S3, and FTP, the transition to Web3 demands a similar expansion of storage infrastructure to accommodate its unique requirements.

Verida provides secure database storage and encryption for private user data. No user data or personally identifiable information (PII) is stored on-chain.

## Verida Storage Nodes

Verida has developed a PouchDB/CouchDB database implementation with open source middleware (Verida Storage Node). This solution is designed for storing private user data, incorporates access controls with flexible encryption and permissioned data synchronization.

The solution is region aware and meetsGDPR requirements. The storage node provides a middleware authentication layer that connects traditional database technology with decentralized identifiers and public blockchains.

The current landscape of decentralized database storage technologies is rapidly evolving. As such, it’s possible other decentralized database storage options will soon reach maturity where they meet the security expectations, access controls and features to a level they can be supported by the Verida network.

User data is stored in isolated [application contexts](application-contexts.md), where the data from one application can not be accessed by another application without the explicit consent of a user. This ensures users can selectively disclose their personal data to different applications on an as-needs basis, providing enhanced privacy and data security.

[Learn more about the key problems the Verida Confidential Database solution solves](https://news.verida.network/revolutionizing-web3-storage-a-deep-dive-into-ipfs-and-verida-dbstore-7a15745dd118) or watch the explainer video below.

{% embed url="https://youtu.be/bCJAk44lzXg" %}
A deep dive into the Verida Confidential Database Infrastructure
{% endembed %}
