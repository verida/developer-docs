# Network Overview

Verida is a network of user controlled decentralized identities that can access decentralized services (Verida Network). These services include private database storage, confidential compute, encrypted P2P messaging and notifications. In time, this will be expanded to include other services such as; block storage, blockchain access, AI services, trust services etc.

Verida provides a reference implementation of a “Data Wallet” ([Verida Wallet](verida-wallet.md)), a mobile application for end users to create decentralized identities, securely store their private keys, manage their crypto and interact with the Verida network.

Verida is also developing a web based "Data Vault" ([Verida Vault](private-data-bridge/verida-vault.md)) as a web interface for users to connect data sources, sync third party data, browse their data and connect their data with AI services.

Verida has a road map of network extensions that expand its capabilities towards a global network of trusted, private, verifiable, self-sovereign data that can power a personalized Web3 future.

## Network Architecture

<figure><img src=".gitbook/assets/Verida Self-Sovereign Infrastructure.jpg" alt=""><figcaption></figcaption></figure>

The Verida Network consists of the following infrastructure:

1. [verida-vault.md](private-data-bridge/verida-vault.md "mention"): A web interface for end users to connect data sources, browse their data and authorize third party applications to access their data and AI services.
2. [verida-wallet.md](verida-wallet.md "mention"): A mobile application to create and self-custody Verida accounts, manage blockchain wallets, sign into decentralized apps and manage user data.
3. [private-data-apis.md](apis/private-data-apis.md "mention"): Enable developers to access user data, with consent.
4. [Broken link](broken-reference "mention"): Enable users to take ownership of their data from third party platforms.
5. [Verida Confidential Storage](protocol/concepts/data-storage.md): Encrypted, permissioned, highly performant decentralized database storage infrastructure for user applications.
6. [Verida Confidential Compute](protocol/concepts/confidential-compute.md): Private, secure, confidential compute infrastructure that runs the Private Data APIs, Private LLMs and (in the future) AI Agents and other user focused APIs built by developers in the Verida ecosystem.
7. [Verida Self-Sovereign Identity](protocol/concepts/decentralized-identity.md): A low cost, high performance, standards compliant (DID-Core) decentralized identifier implementation.
