---
description: >-
  Learn how Verida APIs enforce privacy and security of user data within a
  decentralized environment
---

# Privacy & Security

Verida APIs are built on a first iteration of the Verida Confidential Compute infrastructure. They are designed to find the optimum balance between decentralization, security, privacy and performance.

### Confidential Compute

Verida APIs are running within a confidential computation environment. This means that no-one, not even the underlying infrastructure provider running the API server can access any user data or view the computation occurring on the node.

The first iteration of Verida's Confidential Compute nodes are running inside [Marlin Oyster](https://www.marlin.org/oyster) Trusted Execution Environments (TEE). These nodes provide numerous security guarantees and capabilities:

* Computation occurs within a secure enclave where the node operator has zero visibility
* SSL terminates within the secure enclave, eliminating man-in-the-middle attacks
* Server code is verified to be the expected code
* No data is stored to external disks. All data is secured in memory.

The Verida Foundation is operating the first cohort of Confidential Compute nodes and will open up to node operators in the future.

{% hint style="info" %}
Learn more:

* [Self-Sovereign Confidential Compute Litepaper](../whitepapers.md)
* [Marlin Oysterin depth](https://docs.marlin.org/learn/oyster/introduction)
{% endhint %}

### Confidential Storage

Verida APIs integrate the [Verida Client SDK](../protocol/client-sdk/) within the secure enclave on each confidential compute node. User data is syncronized from the Verida network, decrypted and then loaded into memory for rapid access via API endpoints.

As such, user data retains all the security and privacy benefits of the Verida Network and user data never leaves the secure enclave, accept via user authorized API requests.

{% hint style="info" %}
Learn more:

* [Core Concepts](../protocol/concepts/)
* [Verida Whitepaper](../whitepapers.md)
{% endhint %}

### Source Code

The source code for the User Data APIs are open source and are contained within the [Data Connector Server Github repo](https://github.com/verida/data-connector-server).
