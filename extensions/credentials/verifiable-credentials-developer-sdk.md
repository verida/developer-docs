# Verifiable Credentials Developer SDK

The [@verida/verifiable-credential](https://npmjs.com/@verida/verifiable-credentials) package contains all the necessary capabilities to issue, verify and share Verifiable Credentials on the Verida Network.

### Issuing a Credential[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#issuing-a-credential) <a href="#issuing-a-credential" id="issuing-a-credential"></a>

A credential is issued by creating a JSON object (`credentialData`) containing data matching the schema of the credential to be created. This `credentialData` object is then passed to `credentialSDK.createCredentialJWT()` which populates a `didJwtVc` attribute containing a signed, verified DID JWT Verifiable Credential.

The code below creates a credential that matches a predefined credential schema ([https://common.schemas.verida.io/health/pathology/tests/covid19/pcr/v0.1.0/schema.json](https://common.schemas.verida.io/health/pathology/tests/covid19/pcr/v0.1.0/schema.json)).

```typescript
import Credentials from "@verida/verifiable-credentials";

// Note: `context` should already be obtained by connecting to the Verida Network
const credentialSDK = new Credentials();

// The Verida DID that is the subject of this credential (who is being verified with this credential?)
const subjectDid = 'did:vda:polyamoy:0x....';

const credentialSchema = 'https://common.schemas.verida.io/identity/kyc/FinClusive/individual-basic/latest/schema.json'

// Data for the credential that matches the credential schema
const credentialData = {
  "finClusiveId": "12345",
  "gender": "male",
  "firstName": "Chris",
  "lastName": "Tester",
  "streetAddress1": "123 Four Ave",
  "suburb": "Adelaide",
  "state": "SA",
  "postcode": "5000",
  "dateOfBirth": "2000-01-01"
}

const title = 'KYC Credential'
const summary = 'Credential issued by <signer> on <date>'
const options = {}

const credentialRecord = await credentials.createVerifiableCredentialRecord({
  context: context as any,
  data: credentialData,
  subjectId: subjectDid,
  schema: credentialSchema,
  options
}, title, summary);
```

This generates a `credentialRecord` matching the Verida Credential data storage schema ([https://common.schemas.verida.io/credential/base/v0.2.0/schema.json](https://common.schemas.verida.io/credential/base/v0.2.0/schema.json)):

```typescript
{
  name: 'KYC Credential',
  summary: 'Credential issued by <signer> on <date>',
  schema: 'https://common.schemas.verida.io/credential/base/v0.2.0/schema.json',
  credentialData: {
    "finClusiveId": "12345",
    "gender": "male",
    "firstName": "Chris",
    "lastName": "Tester",
    "streetAddress1": "123 Four Ave",
    "suburb": "Adelaide",
    "state": "SA",
    "postcode": "5000",
    "dateOfBirth": "2000-01-01"
  },
  credentialSchema: 'https://common.schemas.verida.io/identity/kyc/FinClusive/individual-basic/latest/schema.json',
  didJwtVc: 'eyJhbGciOiJFUzI1NksiLCJ0eXAiOiJKV1QifQ.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vd3d3LnczLm9yZy8yMDE4L2NyZWRlbnRpYWxzL2V4YW1wbGVzL3YxIl0sInN1YiI6ImRpZDp2ZGE6MHhDMjYyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIiwidHlwZSI6WyJWZXJpZmlhYmxlQ3JlZGVudGlhbCJdLCJpc3N1ZXIiOiJkaWQ6dmRhOjB4QjM3Mjk5ODJBMjU4NTU0NEZENzJjOTlDRjM3NzNhOWM2YmFCRDU1YyIsImlzc3VhbmNlRGF0ZSI6IjIwMjItMDMtMDhUMjM6MTQ6MjUuNzUxWiIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7ImZ1bGxOYW1lIjoiSmFuZSBEb2UiLCJkYXRlT2ZCaXJ0aCI6IjE5OTItMDctMDMiLCJwYXRpZW50SWQiOiJBQkMxMjMiLCJ0ZXN0VGltZXN0YW1wIjoiMjAyMi0wMy0wMVQxMDozMDowNS40MzVaIiwicmVzdWx0IjoiTmVnYXRpdmUiLCJzY2hlbWEiOiJodHRwczovL2NvbW1vbi5zY2hlbWFzLnZlcmlkYS5pby9oZWFsdGgvcGF0aG9sb2d5L3Rlc3RzL2NvdmlkMTkvcGNyL3YwLjEuMC9zY2hlbWEuanNvbiJ9LCJjcmVkZW50aWFsU2NoZW1hIjp7ImlkIjoiaHR0cHM6Ly9jb21tb24uc2NoZW1hcy52ZXJpZGEuaW8vaGVhbHRoL3BhdGhvbG9neS90ZXN0cy9jb3ZpZDE5L3Bjci92MC4xLjAvc2NoZW1hLmpzb24iLCJ0eXBlIjoiSnNvblNjaGVtYVZhbGlkYXRvcjIwMTgifX0sImlzcyI6ImRpZDp2ZGE6MHhCMzcyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIn0.F9rk2VRz042tQOygU6VuI9NOyTQYYqPcbdfqUPYTvvZ1vx8gIqllSKqZHivpqnWoCB3zyzXKiG1KEHSpHrZYlg'
}
```

`didJwtVc` is a DID-JWT Verifiable Credential generated using the [did-jwt-vc library](https://github.com/decentralized-identity/did-jwt-vc/).

### Verify a Credential[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#verify-a-credential) <a href="#verify-a-credential" id="verify-a-credential"></a>

It is possible to manually verify the `didJwtVc` string to extract the verifiable credential object.

```typescript
import Credentials from "@verida/verifiable-credentials";

// Note: `context` should already be obtained by connecting to the Verida Network
const credentialSDK = new Credentials(context);

const decodedCredential = await credential.verifyCredential(jwt.didJwtVc)
```

Returned `decodedCredential` object:

```typescript
{
  payload: {
    vc: {
      '@context': [Array],
      sub: 'did:vda:polamoy:0xC2629982A2585544FD72c99CF3773a9c6baBD55c',
      type: [Array],
      issuer: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c',
      issuanceDate: '2022-03-08T23:25:31.097Z',
      credentialSubject: [Object],
      credentialSchema: [Object]
    },
    iss: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c'
  },
  didResolutionResult: {
    didDocument: {
      id: 'did:vda:polamoy:0xb3729982a2585544fd72c99cf3773a9c6babd55c',
      controller: 'did:vda:polamoy:0xb3729982a2585544fd72c99cf3773a9c6babd55c',
      assertionMethod: [Array],
      verificationMethod: [Array],
      service: [Array],
      keyAgreement: [Array],
      proof: [Object]
    },
    didDocumentMetadata: {},
    didResolutionMetadata: { contentType: 'application/did+json' }
  },
  issuer: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c',
  signer: {
    id: 'did:vda:polamoy:0xb3729982a2585544fd72c99cf3773a9c6babd55c?context=0x046184d17aeff3bfc319b38e4fc38a922a4021c6a21fd478268d33a3a8fa2b01#sign',
    type: 'EcdsaSecp256k1VerificationKey2019',
    controller: 'did:vda:polamoy:0xb3729982a2585544fd72c99cf3773a9c6babd55c',
    publicKeyHex: '0x0219d9d875c4349ed4420ccd681e91defc796ca7c9a0ba39024c8743ccd4372442'
  },
  jwt: 'eyJhbGciOiJFUzI1NksiLCJ0eXAiOiJKV1QifQ.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vd3d3LnczLm9yZy8yMDE4L2NyZWRlbnRpYWxzL2V4YW1wbGVzL3YxIl0sInN1YiI6ImRpZDp2ZGE6MHhDMjYyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIiwidHlwZSI6WyJWZXJpZmlhYmxlQ3JlZGVudGlhbCJdLCJpc3N1ZXIiOiJkaWQ6dmRhOjB4QjM3Mjk5ODJBMjU4NTU0NEZENzJjOTlDRjM3NzNhOWM2YmFCRDU1YyIsImlzc3VhbmNlRGF0ZSI6IjIwMjItMDMtMDhUMjM6MjU6MzEuMDk3WiIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7ImZ1bGxOYW1lIjoiSmFuZSBEb2UiLCJkYXRlT2ZCaXJ0aCI6IjE5OTItMDctMDMiLCJwYXRpZW50SWQiOiJBQkMxMjMiLCJ0ZXN0VGltZXN0YW1wIjoiMjAyMi0wMy0wMVQxMDozMDowNS40MzVaIiwicmVzdWx0IjoiTmVnYXRpdmUiLCJzY2hlbWEiOiJodHRwczovL2NvbW1vbi5zY2hlbWFzLnZlcmlkYS5pby9oZWFsdGgvcGF0aG9sb2d5L3Rlc3RzL2NvdmlkMTkvcGNyL3YwLjEuMC9zY2hlbWEuanNvbiJ9LCJjcmVkZW50aWFsU2NoZW1hIjp7ImlkIjoiaHR0cHM6Ly9jb21tb24uc2NoZW1hcy52ZXJpZGEuaW8vaGVhbHRoL3BhdGhvbG9neS90ZXN0cy9jb3ZpZDE5L3Bjci92MC4xLjAvc2NoZW1hLmpzb24iLCJ0eXBlIjoiSnNvblNjaGVtYVZhbGlkYXRvcjIwMTgifX0sImlzcyI6ImRpZDp2ZGE6MHhCMzcyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIn0.j8JpTTndAUGEGEx0BcnXBjCIOCDPBuY48H8nj_S_og80ainE7gYNLNBd-I7Xdz73P-yEjFO01kY6tgd5wc0p4w',
  verifiableCredential: {
    vc: {
      sub: 'did:vda:polamoy:0xC2629982A2585544FD72c99CF3773a9c6baBD55c',
      issuer: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c',
      issuanceDate: '2022-03-08T23:25:31.097Z'
    },
    credentialSubject: {
      "finClusiveId": "12345",
      "gender": "male",
      "firstName": "Chris",
      "lastName": "Tester",
      "streetAddress1": "123 Four Ave",
      "suburb": "Adelaide",
      "state": "SA",
      "postcode": "5000",
      "dateOfBirth": "2000-01-01"
    },
    issuer: { id: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c' },
    type: [ 'VerifiableCredential' ],
    credentialSchema: {
      id: 'https://common.schemas.verida.io/identity/kyc/FinClusive/individual-basic/latest/schema.json',
      type: 'JsonSchemaValidator2018'
    },
    '@context': [
      'https://www.w3.org/2018/credentials/v1',
      'https://www.w3.org/2018/credentials/examples/v1'
    ],
    proof: {
      type: 'JwtProof2020',
      jwt: 'eyJhbGciOiJFUzI1NksiLCJ0eXAiOiJKV1QifQ.eyJ2YyI6eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSIsImh0dHBzOi8vd3d3LnczLm9yZy8yMDE4L2NyZWRlbnRpYWxzL2V4YW1wbGVzL3YxIl0sInN1YiI6ImRpZDp2ZGE6MHhDMjYyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIiwidHlwZSI6WyJWZXJpZmlhYmxlQ3JlZGVudGlhbCJdLCJpc3N1ZXIiOiJkaWQ6dmRhOjB4QjM3Mjk5ODJBMjU4NTU0NEZENzJjOTlDRjM3NzNhOWM2YmFCRDU1YyIsImlzc3VhbmNlRGF0ZSI6IjIwMjItMDMtMDhUMjM6MjU6MzEuMDk3WiIsImNyZWRlbnRpYWxTdWJqZWN0Ijp7ImZ1bGxOYW1lIjoiSmFuZSBEb2UiLCJkYXRlT2ZCaXJ0aCI6IjE5OTItMDctMDMiLCJwYXRpZW50SWQiOiJBQkMxMjMiLCJ0ZXN0VGltZXN0YW1wIjoiMjAyMi0wMy0wMVQxMDozMDowNS40MzVaIiwicmVzdWx0IjoiTmVnYXRpdmUiLCJzY2hlbWEiOiJodHRwczovL2NvbW1vbi5zY2hlbWFzLnZlcmlkYS5pby9oZWFsdGgvcGF0aG9sb2d5L3Rlc3RzL2NvdmlkMTkvcGNyL3YwLjEuMC9zY2hlbWEuanNvbiJ9LCJjcmVkZW50aWFsU2NoZW1hIjp7ImlkIjoiaHR0cHM6Ly9jb21tb24uc2NoZW1hcy52ZXJpZGEuaW8vaGVhbHRoL3BhdGhvbG9neS90ZXN0cy9jb3ZpZDE5L3Bjci92MC4xLjAvc2NoZW1hLmpzb24iLCJ0eXBlIjoiSnNvblNjaGVtYVZhbGlkYXRvcjIwMTgifX0sImlzcyI6ImRpZDp2ZGE6MHhCMzcyOTk4MkEyNTg1NTQ0RkQ3MmM5OUNGMzc3M2E5YzZiYUJENTVjIn0.j8JpTTndAUGEGEx0BcnXBjCIOCDPBuY48H8nj_S_og80ainE7gYNLNBd-I7Xdz73P-yEjFO01kY6tgd5wc0p4w'
    }
  }
}
```

### Present a Credential[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#present-a-credential) <a href="#present-a-credential" id="present-a-credential"></a>

There is a specific process to "present" a credential, which wraps the verifiable credential data within a presentation object. The standard allows a credential holder to present multiple credentials at once.

```typescript
// Note: `context` should already be obtained by connecting to the Verida Network
const credentialSDK = new Credentials(context);

// Create a presentation with one or more `didJwtVc` strings
const vcPresentation = credentialSDK.createVerifiablePresentation([credential.didJwtVc])
```

### Sharing a Credential[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#sharing-a-credential) <a href="#sharing-a-credential" id="sharing-a-credential"></a>

A credential holder can share a credential stored in their Verida Account in several ways:

1. Generate a shareable URL (this is implemented in the Verida Wallet when viewing a credential where the URL is embedded in a QR code)
2. Send the credential via the Verida Messaging system (this is implemented in the Verida Wallet via request and send an inbox message)
3. Make the credential public via their public `credential` Datastore

#### **URL credential sharing**[**​**](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#url-credential-sharing)

The `SharingCredential` library takes a private `credential` object and makes it available via a public Verida URI for any third party to access. In order to protect privacy, the credential is encrypted so any third party must be supplied the encryption key in order to access the credential.

```typescript
import { SharingCredential } from "@verida/verifiable-credentials";

// `credential` is a credential previously created with credentialSDK.createCredentialJWT()
const shareData = await shareCredential.issueEncryptedPresentation(credential);
```

Returned `shareData` object:

```typescript
{
  // Encrypted credential that is stored in the public credential database located at `veridaUri`
  item: {
    content: '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',
    schema: 'https://common.schemas.verida.io/credential/public/encrypted/v0.1.0/schema.json'
  },
  // The result object returned from the public credential database
  result: {
    ok: true,
    id: '876ec340-9f3c-11ec-a4d8-35d2dca04a1c',
    rev: '1-b1b26bc16dbac1ee0b1db2afffe7daaa'
  },
  // The DID of the Verida Account that is sharing the credential
  did: 'did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c',
  // The Verida URI endpoint pointing to where the encrypted credential is stored
  // verida://<did>/base64(<contextName>)/<databaseName>/recordId?key=<encryptionKey>
  veridaUri: 'verida://did:vda:polamoy:0xB3729982A2585544FD72c99CF3773a9c6baBD55c/GpNDePvWU4RQud9jaZJiNwFyAS/credential_public_encrypted/876ec340-9f3c-11ec-a4d8-35d2dca04a1c?key=c8fd78a9baa44c8ade5e269e0310dcf1139d0374980110b4802b5db6e0820543',
  // A public URI that can be opened in a web browser that can decode a base64 encoded veridaUri, fetch the data, decrypt it and display to the viewer
  publicUri: 'https://explorer.verida.network/credential?uri=dmVyaWRhOi8vZGlkOnZkYToweEIzNzI5OTgyQTI1ODU1NDRGRDcyYzk5Q0YzNzczYTljNmJhQkQ1NWMvR3BORGVQdldVNFJRdWQ5amFaSmlOd0Z5QVMvY3JlZGVudGlhbF9wdWJsaWNfZW5jcnlwdGVkLzg3NmVjMzQwLTlmM2MtMTFlYy1hNGQ4LTM1ZDJkY2EwNGExYz9rZXk9YzhmZDc4YTliYWE0NGM4YWRlNWUyNjllMDMxMGRjZjExMzlkMDM3NDk4MDExMGI0ODAyYjVkYjZlMDgyMDU0Mw=='
}
```

`publicUri` can be shared with anyone who can then open the credential in their web browser.

`veridaUri` can be shared with other systems to access the underlying credential data.

#### **Send via Verida Messaging**[**​**](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#send-via-verida-messaging)

The `credential` object created by an issuer can be sent to a Verida Account via their Vault.

See [Client SDK: Messaging](../../protocol/client-sdk/messaging.md)

### Blockchain proofs[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#blockchain-proofs) <a href="#blockchain-proofs" id="blockchain-proofs"></a>

The credential SDK supports the concept of `Blockchain proofs` that are certain attributes of a credential that are collectively signed in such a way as they can be quickly and cheaply verified on a blockchain.

For example, it is possible to issue a Verifiable Credential proving someones credit score, but then provide a blockchain proof specifying the value of the credit score. This would allow the credit score to be used in a smart contract (or issued as a Soulbound token) without requiring the full verifiable credential to be parsed on chain (which is very expensive).

Blockchain proofs are defined in the credential schema.

See this example from [FinClusive/individual-basic](https://github.com/verida/schemas-common/blob/e1aa68c9ae068e15f30433a9351e70d218d833d7/identity/kyc/FinClusive/individual-basic/v0.1.0/schema.json#L9) which defines a `finclusive/individual-kyc` proof:

```typescript
"veridaProofs": {
    "finclusive/individual-kyc": ["finclusive/individual-basic-kyc", "$finClusiveId"]
},
```

Here is the proof named `finclusive/individual-kyc` consists of a proof string that combines two attributes:

1. The unaltered string `finclusive/individual-basic-kyc`
2. The value of the credential for the variale `finClusiveId`

These two attributes will be joined with the `-` character.

The `$` parameter indicates the value of the credential should be included.

The proof string that will be signed (usingthe example credential above):

```
finclusive/individual-kyc-12345
```

When the Verida SDK generates a credential, it will include a `proofs` attribute in the `payload` containing an array of signatures for each `veridaProof`.

These proofs can then be verified on chain using the [Verida Personal Data Bridge](https://developers.verida.network/docs/extensions/personal-data-bridge)

## Credential Schemas[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#credential-schemas) <a href="#credential-schemas" id="credential-schemas"></a>

Within the Verida network each credential is a data object (represented as JSON) that matches a particular credential schema.

Here is an example of a working credential schema [Identity / KYC / FinClusive / Individual (Basic)](https://github.com/verida/schemas-common/blob/develop/identity/kyc/FinClusive/individual-basic/v0.1.0/schema.json)

You can create your own schemas.

info

Schemas should be hosted on a publicly available web host that sets the `Access-Control-Allow-Origin` CORS header. [Read more here](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors/CORSMissingAllowOrigin).

## Verida Wallet[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#verida-wallet) <a href="#verida-wallet" id="verida-wallet"></a>

Credentials stored with the [credential schema](https://common.schemas.verida.io/credential/base/v0.2.0/schema.json) will automatically be rendered in the Verida Wallet as a credential. This custom display includes:

1. A scannable QR code
2. The profile icon of the Verida DID that issued/signed the credential
3. A tick of approval indicating the credential has been verified

### Setting the signer avatar[​](https://developers.verida.network/docs/extensions/credentials/verifiable-credentials#setting-the-signer-avatar) <a href="#setting-the-signer-avatar" id="setting-the-signer-avatar"></a>

The Verida Wallet will automatically show the public profile icon of the Verida Account that signed the credential. You will need to set this icon for your Verida account so the icon displays correctly.

You could add the signing Verida Account to the Verida Wallet and use the mobile app to set a profile avatar and name. This will then be the default for every inbox message and credential issued across the Verida network. Alternatively, you could manually set the profile information for the application context generating the credential. See[ Account Profiles ](broken-reference)for more information on how to achieve this.
