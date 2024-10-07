---
description: Overview of the Verida Private Data Data Bridge
---

# Introduction

{% hint style="warning" %}
The Verida Private Data Bridge is currently in private alpha to early builders.
{% endhint %}

The Verida Private Data Bridge is a collection of technologies and infrastructure that enable end users to extract their data from existing centralized platforms (ie: Google, Apple, Telegram, Discord), store it encrypted on the Verida Network and then authorize their data to be used with AI and other third party applications.

<figure><img src="../.gitbook/assets/Verida Data Bridge - Data Flow.png" alt=""><figcaption><p>Verida Data Bridge: Flow of user data from existing platforms to users to third party applications</p></figcaption></figure>

The Verida Private Data Bridge consists of:

1. [verida-vault.md](verida-vault.md "mention"): Web interface for end users to connect and extract their data from centralized platforms
2. [private-data-apis.md](../apis/private-data-apis.md "mention"): A collection of APIs for developers to authorize, authenticate, query and run AI prompts against private user data.
3. [data-connections.md](data-connections.md "mention"): A framework for writing standardized code, with unit tests, for each connector. These data connectors are responsible for authenticating, authorizing and syncronizing user data from centralized platforms.
4. [data-bridge-server.md](data-bridge-server.md "mention"): Backend server, running on Verida's Confidential Compute infrastructure, that facilitates data connections and syncronization of data for end users
5. [blockchain-bridge.md](blockchain-bridge.md "mention"): A set of smart contracts and client side libraries that enable user data to be submitted on-chain and verified.
