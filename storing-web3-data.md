---
hidden: true
---

# Storing Web3 Data

## Where should Web3 Data be stored?

A challenge all web3 developers need to address when building applications is where the data is stored. Historically, most web3 developers have chosen to use either blockchain storage or IPFS and sometimes resort to a traditional centralized database (eg MongoDB, Postgres) for some types of data. Verida provides a decentralized replacement for those traditional database and a better option for some of the things developers are currently using blockchain storage or IPFS for.

## Choosing data storage <a href="#choosing-data-storage" id="choosing-data-storage"></a>

In the flowchart below we show some of the key decisions a developer must make when choosing storage. Firstly, look at your data and decide if it is structured or unstructured. Examples of structured data include data that you would keep in a spreadsheet, a JSON or XML file or a Notion database. Unstructured data includes images, movies or other multimedia.

Secondly, is your data public or private? By public we mean it can be accessed without access control mechanisms. Note that encryption on its own is not an access control mechanism because access cannot be revoked if a key leaks.

Finally, do you need to update or delete data? Be careful here - some storage systems implement deletion as the removal of a reference to data that is stored elsewhere. In these cases, the data is still there and anyone who recorded that reference can easily access it. Both deletion and updates should be implemented in such a way that all copies of the data are irrevocably modified.

## Limitations of Current Web3 Data Storage Solutions <a href="#limitations-of-current-web3-data-storage-solutions" id="limitations-of-current-web3-data-storage-solutions"></a>

Blockchain storage is the first type of storage most web3 developers use. However, it doesn't take long to realize this is best suited for very specific types of data such as links to external data like NFTs. The nature of a blockchain makes it expensive and inefficient to store more than small amounts of data and the immutable, public nature of the data means it is unsuitable for many applications.

Many developers discover these limitations and switch to using [IPFS](https://ipfs.io/). IPFS is a globally accessible content addressable file storage network, where files are referenced by the hash of their content. This makes it very well suited to "flat file" type storage, and it has had great success as storage for NFT images. In broad terms, IPFS is used in a similar way to how AWS S3 is used in Web2 development - to make files available to people and applications.

The next limitation Web3 developers find is around private data and managing access permissions to that data. Users want self-sovereignty over personal data so they can control their own information. How do you store things like a user's interests or even things like health information in a way that doesn't make it publicly available to anyone for data mining or identity theft?

Many developers resort to trying to encrypt data and store it either on-chain or in encrypted files on IPFS. There are a number of serious problems with this:

* Most developers are not cryptographers and often make mistakes in how they implement their encryption, putting user data at risk.
* Both blockchains and IPFS are immutable storage systems, meaning that any mistake either in implementation or by a user accidentally leaking an encryption key means their data is globally public forever with no way to fix it.
* The lack of a proper query mechanism leads to badly performing apps. Either applications load large files into memory and developers find data inside them or there are many small files each of which must be requested each time it is needed.
* Updates and deletes are implemented by copying all data to a new version of the file and re-saving it. This is both expensive and slow.
* No control over the physical storage location of data which is often important for legal or policy reasons.

Verida exists to solve these problems. The [Verida DbStore](https://github.com/verida/documentation/blob/feature/where-to-store-web3-data/docs/concepts/data-storage) is a decentralized, end-to-end (E2E) encrypted document database.

Because it is a database, developers get strong query capabilities. Data updates are trivial and fast which means correcting any errors in the data is simple. Databases are both encrypted (so storage node operators can never see the data) and protected by an authentication system (so they are inaccessible to non-authorized users).

open-sourceVerida [storage nodes](https://github.com/verida/documentation/blob/feature/where-to-store-web3-data/docs/network/storage-node) are open source and can be self-hosted. Users can choose what node to store their data on, meaning that the legal and policy issues around data sovereignty are easily addressed. Data can be replicated to multiple non-related nodes for better availability and performance.

The table below is an easy to use reference to look at when comparing the three methods:

|                     | On Chain                                  | IPFS                           | Verida                                                                                  |
| ------------------- | ----------------------------------------- | ------------------------------ | --------------------------------------------------------------------------------------- |
| Web 2 Analogy       | N/A                                       | AWS S3                         | MongoDB                                                                                 |
| Storage Type        | Immutable, globally replicated blockchain | Content Addressable File Store | Encrypted Decentralized Document Database                                               |
| Storage Abstraction | Write-once data field                     | Write-once flat file           | Database API with create, read, update and delete mechanisms                            |
| Storage Location    | Replicated everywhere                     | Replicated everywhere          | Controlled by user                                                                      |
| Privacy             | Never Private                             | Never Private                  | Private by default (data can selectively be made public or shared with specific users)  |
| Encryption          | None                                      | None                           | Built in end to end (E2E) encryption                                                    |
| Authentication      | None                                      | None                           | Per-user authentication                                                                 |
| Access Management   | None                                      | None                           | A user-centric consent based framework for identity, access management and data sharing |

## Conclusion <a href="#conclusion" id="conclusion"></a>

Verida DbStore is a new type of Web3 storage that provides capabilities that are important to many applications. It is fast, resilient and easy to use.

## Additional Resources <a href="#additional-resources" id="additional-resources"></a>

* [Self-sovereign data storage concepts](protocol/concepts/data-storage.md)
* [Data sharing technical documentation](protocol/concepts/data-sharing.md)
* [Running a Verida Storage Node](protocol/run-a-node/database-node/)
* [Interactive tutorial to learn about Verida storage](protocol/interactive-tutorial.md)
* [Join our Discord](https://discord.verida.io/)
* [Follow us on X (Twitter)](https://twitter.com/Verida\_io)

### Video walk-through of Verida Data Storage <a href="#video-walk-through-of-verida-data-storage" id="video-walk-through-of-verida-data-storage"></a>

{% embed url="https://www.youtube.com/watch?v=jmWtvvkk4WE" %}
