---
cover: ../../.gitbook/assets/Screenshot 2024-08-05 130145.png
coverY: 0
---

# Client SDK

## What is the Verida Client SDK?[​](https://developers.verida.network/docs/client-sdk#what-is-the-verida-client-sdk) <a href="#what-is-the-verida-client-sdk" id="what-is-the-verida-client-sdk"></a>

The Verida Client SDK is an open source implementation of a Verida network client providing encrypted storage, identity, messaging, and schemas capabilities. It is an easy-to-use library that abstracts the complexities behind these capabilities, thereby allowing developers to build self-sovereign applications.

This Verida Client SDK is the main library used by developers to add Verida protocol support into an application. There are two implementations for the Client SDK:

* [NodeJs Client](https://github.com/verida/verida-js/tree/main/packages/client-ts) — A package that can be used in a web browser or NodeJs environment.
* [React Native client](https://github.com/verida/client-rn) — A package that can be used in React Native mobile applications ([Learn more](react-native.md)).

The Client SDK provides the following core capabilities:

* Authenticate a user via a known private key or a Web3 modal popup
* Access encrypted database storage for your application for the authenticated user
* Send/receive messages between different users and applications

A user can have multiple[ Application Contexts](../concepts/application-contexts.md) that provide completely separate databases, messaging and storage.

## More about the Client SDK[​](https://developers.verida.network/docs/client-sdk#more-about-the-client-sdk) <a href="#more-about-the-client-sdk" id="more-about-the-client-sdk"></a>

[Getting Started](getting-started.md)

[Authentication](authentication.md)

[Data](data.md)

[Queries](queries.md)

[Permissions](permissions.md)

[Messaging](../concepts/messaging.md)

[Profiles](account-profiles.md)

[Events](events.md)

[Configuration](configuration.md)

[React Native](react-native.md)

[Advanced](advanced.md)
