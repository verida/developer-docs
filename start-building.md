---
description: Get started building applications using private user data
---

# Start Building

The Verida Network allows your application to request access to private user data, store private user data or access confidential compute that performs operations on user data (including AI services).

## 1. Create a Verida Account <a href="#client-sdk" id="client-sdk"></a>

You first need to create a decentralized identity, which will become your account on the Verida Network.

There are currently two ways to do this:

1. [Download the Verida Wallet](https://www.verida.network/verida-wallet) (iOS and Android) to create a new account.
2. Use the [Verida Command Line](start-building.md#client-sdk) tools to create a new account.

## 2. Choose an Integration Method

Your application can integrate via the [Verida Client SDK](start-building.md#client-sdk-1) or [Verida APIs](broken-reference), each with their own benefits and trade offs.

### [Verida APIs](broken-reference) (currently invite only alpha)

\[ Database Storage ] \[ AI Services ] \[ Confidential Compute ]

A set of easy to use API's running on the Verida Confidential Compute infrastructure to ensure user data is kept private at all times.

These API's provide decrypted, fast access to user data while also providing other developer services such as AI prompt endpoints connected to user data and keyword search across datasets. We intend to extend the API services in the future to include other useful endpoints (ie: Vector databases)

Your application redirects a user to authorize access for your application with the [Broken link](broken-reference "mention")via OAuth. Once authorized, you obtain access and refresh tokens that can be used to access user data from confidential compute nodes on the Verida network.

[Learn more](broken-reference)

### [Verida Client SDK](protocol/client-sdk/)

\[ Database Storage ] \[ Identity Management ] \[ Verifiable Credentials ]

An open source implementation of a Verida network client enabling developers to build applications that access encrypted private storage, identity, messaging, and schemas capabilities.

The SDK offers a typescript library that abstracts the complexities behind these capabilities, thereby allowing developers to build self-sovereign applications. The library can run in the web-browser or in a Node.js server.

Your application integrates the [verida-connect-sdk](protocol/verida-connect-sdk/ "mention") to facilitate a QR code based sign in with users who have installed the [verida-wallet.md](verida-wallet.md "mention").

[Learn more](protocol/client-sdk/)

## 3. Learn more about the Verida Network

These resources will help you quickly learn about the Verida Network:

1. [concepts](protocol/concepts/ "mention")
2. [whitepapers.md](whitepapers.md "mention")
3. [Verida Token](https://www.verida.network/vda-token)
4. [Latest News](https://news.verida.network/)
5. [Community Portal](https://community.verida.network/)
6. [Network Explorer](https://explorer.verida.network/)
