# cheqd Credential Service

The Verida Wallet has full support for [Verifiable Credentials](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials)[ ](verifiable-credentials-developer-sdk.md)issued by [cheqd’s Credential Service](https://docs.cheqd.io/identity/credential-service/get-started), a simple way of issuing Credentials over REST API.

Through this integration, the Verida Wallet can support storing, managing and verifying Verifiable Credentials signed by cheqd’s DID Method ([did:cheqd](https://docs.cheqd.io/identity/architecture/adr-list/adr-001-cheqd-did-method)), verifying credential revocation status using cheqd’s approach to status lists on-ledger (see [DID-Linked Resources](https://docs.cheqd.io/identity/architecture/adr-list/adr-002-did-linked-resources)) and soon, charging for revocation status checks.

Below is a diagram that explains how the cheqd and Verida stacks fit together.

<figure><img src="../../.gitbook/assets/Verida_X_cheqd_diagram-f95dc9584bb7a5fb53c54e771ad53cb3 (1).png" alt=""><figcaption></figcaption></figure>

## Getting Started[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#getting-started) <a href="#getting-started" id="getting-started"></a>

If you want to get started with cheqd’s Credential Service, you can follow the tutorials below.

### Set up your account[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#set-up-your-account) <a href="#set-up-your-account" id="set-up-your-account"></a>

Sign up or log in to access cheqd’s Credential Service APIs.

[Get started here](https://docs.cheqd.io/identity/credential-service/set-up-account)

### Issue a Credential[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#issue-a-credential) <a href="#issue-a-credential" id="issue-a-credential"></a>

Issue, verify and revoke W3C Verifiable Credentials, using did:cheqd for the issuer and did:vda for the subject.

[Get started here](https://docs.cheqd.io/identity/credential-service/credentials)

### Charge for Credentials[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#charge-for-credentials) <a href="#charge-for-credentials" id="charge-for-credentials"></a>

Create incentivised payment flows for Credentials, providing a new revenue stream to issuers

[Get started here](https://docs.cheqd.io/identity/credential-service/payments)

### Create DIDs[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#create-dids) <a href="#create-dids" id="create-dids"></a>

Create, update and deactivate on-ledger DIDs with cheqd’s DID method (did:cheqd).

[Get started here](https://docs.cheqd.io/identity/credential-service/dids)

## Learn more about cheqd[​](https://developers.verida.network/docs/extensions/credentials/cheqd-credential-service#learn-more-about-cheqd) <a href="#learn-more-about-cheqd" id="learn-more-about-cheqd"></a>

[Read the announcement of Verida's collaboration with cheqd](https://news.verida.io/verida-and-cheqd-release-mobile-wallet-credential-solution-for-enterprises-2a2aeeef1f8a)

To learn more about cheqd, head over to their [website](https://cheqd.io/) and [developers documentation](https://docs.cheqd.io/identity/getting-started/readme).
