---
description: >-
  Learn how Verida APIs enforce privacy and security of user data within a
  decentralized environment
---

# Privacy & Security

Verida APIs are built on a first iteration of the Verida Confidential Compute infrastructure. They are designed to find the optimum balance between decentralization, security, privacy and performance.

## Confidential Compute

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
* [Marlin Oyster in depth](https://docs.marlin.org/learn/oyster/introduction)
{% endhint %}

## Confidential Storage

Verida APIs integrate the [Verida Client SDK](../protocol/client-sdk/) within the secure enclave on each confidential compute node. User data is syncronized from the Verida network, decrypted and then loaded into memory for rapid access via API endpoints.

As such, user data retains all the security and privacy benefits of the Verida Network and user data never leaves the secure enclave, accept via user authorized API requests.

{% hint style="info" %}
Learn more:

* [Core Concepts](../protocol/concepts/)
* [Verida Whitepaper](../whitepapers.md)
{% endhint %}

## LLM Privacy \[alpha]

{% hint style="warning" %}
Important privacy notice for the alpha release
{% endhint %}

The large language models (LLM) currently used in the Verida APIs are _not_ currently running in a Verida Confidential Compute secure enclave. Secure enclaves do not currently support GPU access which is necessary for performant LLM operations.

The alpha release provides the option of using [Amazon Web Services Bedrock](https://aws.amazon.com/bedrock/), [Groq](https://groq.com/) or your own LLM.

This is a temporary solution as we are collaborating with partners to enable LLM's to run efficiently and cost effectively within secure enclaves. While this is not perfect, we believe the [AWS Bedrock privacy architecture and security model](https://aws.amazon.com/bedrock/security-compliance/) and the [Groq privacy policy](https://groq.com/privacy-policy/) provide adequate protections for this alpha release, while those with highly sensitive requirements can still provide their own custom LLM.

## Custom LLM

You can provide your own OpenAI compatible LLM endpoint and API key through the [Private Data APIs](https://user-apis.verida.network/#61bf5cf2-cb2e-43af-b592-f89d0b0d291a) and the Private Data Bridge developer interface.

## Source Code

The source code for the User Data APIs are open source and are contained within the [Data Connector Server Github repo](https://github.com/verida/data-connector-server).
