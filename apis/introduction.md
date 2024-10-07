# Introduction

{% hint style="warning" %}
Verida Private Data APIs are currently in private alpha to early builders.
{% endhint %}

Verida API's are designed to provide a simple integration pathway for third party applications to access private user data.

They enable applications to:

* Query user data
* Perform fast keyword search
* Live stream / watch changes to user data
* Access AI endpoints that are connected to user data

{% hint style="info" %}
Learn how to use [private-data-apis.md](private-data-apis.md "mention")
{% endhint %}

## API Objectives

Decentralized networks operate on a model where a user has self-custody over a private key which they use to sign transactions to perform operations on a network. This differs from centralized platforms where a user authenticates and the platform can perform any operation on behalf of a user without any further input.

Verida's API infrastructure is designed to mitigate the challenges associated with developers integrating SDK's, by providing a secure APIs to access decentralized user storage and compute infrastructure.

We are starting with the [User Data API](private-data-apis.md) which supports:

1. Read access to user data (Emails, Documents, Telegram, Slack etc.)
2. Confidential AI prompts (Llama LLM with guaranteed privacy)
3. Confidential Personalized AI prompts (connected to user data via RAG)

### SDK Challenges

Building applications with the decentralized model introduces significant challenges for developers. It requires integrating a Software Development Kit (SDK) into the application, add support for crypto wallets and ask user to sign transactions on a regular basis.

Integrating SDK's is challenging. They often have a very large number of dependencies that require regular updating, they increase the download and runtime size of the application. Crypto in particular has lots of complex encryption and signing libraries and often incorporate bleeding edge technology. More often than not, a typical application needs multiple SDK's, so these problems quickly compound. This can be a blocker for larger enterprises that need to carefully manage the requirements of their client applications.

These SDK's operate in a client-side environment (web browser, mobile device). These environments introduce their own challenges such as limited bandwidth and unknown CPU capabilities. There are many different browsers, different versions, mobile devices etc. that all introduce more complexities with complex SDK's.

### API Benefits

On the other hand, centralized applications have APIs that can be upgraded at any time to keep up-to-date with the latest versions of dependencies. They can have large amounts of CPU and bandwidth added on demand to meet the needs of the application and user demand.

They provide a simple integration path for third party applications to access the business logic and capabilities of the application via a consumable API. This introduces no SDK dependencies to existing code; decreasing code size, increasing compatibility, reducing code injection risks.

### Verida APIs

The Verida APIs provider a set of helper API methods that wrap around the Verida SDK. This enables developers to integrate their applications with the Verida SDK's instead of incorporating the Verida SDK, providing a simpler and more flexible integration approach.

It also enables the Verida APIs to provide other services which wouldn't be possible via a SDK, such as confidential and personalized AI interfaces.

Verida APIs are part of Verida's Self-Sovereign Compute Infrastructure (See [Litepaper](../whitepapers.md) for details). This compute infrastructure is operating on TEE hardware running secure enclaves running on the [Marlin Oyster](https://www.marlin.org/oyster) network.

See [Privacy & Security](privacy-and-security.md) to learn more.
