# Building with APIs

{% hint style="warning" %}
Verida Private Data APIs are currently in private alpha to early builders.
{% endhint %}

As a developer, you can use private user data in your applications. You have the ability to query, search, execute AI requests with user data.

## Authentication

The current alpha currently requires a user to provide their Verida Account private key as a `key` header in all requests. Read about [privacy-and-security.md](privacy-and-security.md "mention")to learn how confidential compute is used to ensure the user's key remains private.

In October 2024 we will be releasing an OAuth based authentication flow as part of our release of the [verida-vault.md](../private-data-bridge/verida-vault.md "mention").

<figure><img src="../.gitbook/assets/Third Party Application Architecture.png" alt=""><figcaption></figcaption></figure>

The process is as follows:

1. Your application requests access from the user and specifies permissions to request (via scopes)
2. The user is redirected to the [verida-vault.md](../private-data-bridge/verida-vault.md "mention") to authorize access (similar to Google or Facebook OAuth)
3. The user grants consent and a token is returned to your application
4. The token can then be used to make API requests to [private-data-apis.md](private-data-apis.md "mention")

{% hint style="info" %}
**Coming soon:** You will be able to build custom API endpoints, deploy them on Verida's Confidential Compute infrastructure and set a price, enabling you to charge for use of your bespoke user focused capabilities (ie: AI Agents, Private AI Models etc.)
{% endhint %}
