# Blockchain Bridge

This Verida extension allows private off-chain data to be securely used within smart contracts in a cryptographically trusted manner.

## Use case examples[​](https://developers.verida.network/docs/extensions/personal-data-bridge#use-case-examples) <a href="#use-case-examples" id="use-case-examples"></a>

This enables use cases such as:

1. A credit rating agency signing a credit score linked to a Verida identity (DID). The identity controller (end user) submits this credit score to a smart contract to access an under-collateralized loan.
2. A trusted entity signing a proof that a DID controls a social media account (ie: Facebook). The end user submits this social media ownership proof to a smart contract to mint a Soulbound token to a blockchain wallet the identity controls.
3. A trusted entity completes KYC verification of an end user and signs a message indicating the end-user has passed KYC to generate a "KYC proof" associated with the user's DID. The end user submits this KYC proof to a smart contract in order to access a decentralized exchange that requires users to be KYC'd.

## How it works[​](https://developers.verida.network/docs/extensions/personal-data-bridge#how-it-works) <a href="#how-it-works" id="how-it-works"></a>

1. A trusted DID (`trustedDid`) signs off-chain data and sends it to the user
2. The user generates a smart contract request and signs it
3. The smart contract verifies the smart contract request and verifies the signed off-chain data can be trusted
4. The smart contract uses the trusted data within the smart contract

## Usage: Submit a verified request to a smart contract[​](https://developers.verida.network/docs/extensions/personal-data-bridge#usage-submit-a-verified-request-to-a-smart-contract) <a href="#usage-submit-a-verified-request-to-a-smart-contract" id="usage-submit-a-verified-request-to-a-smart-contract"></a>

We will walk through an end-to-end example of submitting a request to a smart contract that is verified to have originated from a Verida DID.

### Signing data (client side)[​](https://developers.verida.network/docs/extensions/personal-data-bridge#signing-data-client-side) <a href="#signing-data-client-side" id="signing-data-client-side"></a>

When a record is saved into a Verida `datastore` or `database`, the full JSON record is automatically signed by the protocol. The record is signed by a "signing key" that is controlled by the Verida DID that is saving the data. It's possible for other DID's to also sign this data, producing multiple signatures.

It's also possible to sign any data, for example:

```typescript
const CONTEXT_NAME = 'ACME: Test Application'
const keyring = account.keyring(CONTEXT_NAME)   // account is an instance of `@verida/account` (likely `account-web-vault` if in a web environment)

const data = 'data to sign'
const signature = keyring.sign(data)
```

### Generate smart contract request (client side)[​](https://developers.verida.network/docs/extensions/personal-data-bridge#generate-smart-contract-request-client-side) <a href="#generate-smart-contract-request-client-side" id="generate-smart-contract-request-client-side"></a>

We will submit a message ("hello world") to a smart contract that originates from a Verida DID and is protected from replay attacks:

```typescript
const CONTEXT_NAME = 'ACME: Test Application'
const keyring = account.keyring(CONTEXT_NAME)
// Fetch the DID of the account
const did = await account.did()
// Strip the address part of the DID for increased on-chain efficiency
const didAddress = getAddressFromDid(did)

// Fetch the next nonce from the smart contract, to include in our signed request
const userDidNonce = (await contract.nonce(didAddress)).toNumber()

// Fetch the proof that verifies the current keyring signing context is controlled by the DID
const didDocument = await didClient.get(signerDid)
const userContextProof = didDocument.locateContextProof(CONTEXT_NAME)

// The message to send to the smart contract
const message = 'hello world'

// Generate the parameters that will be sent to the smart contract
const requestParams = ethers.utils.AbiCoder.prototype.encode(
    ['string'],
    [message]
)

// Sign the request parameters with the nonce
const requestParamsWithNonce = ethers.utils.solidityPack(
    ['bytes', 'uint'],
    [requestParams, userDidNonce]
)
const signedRequest = await keyring.sign(requestParamsWithNonce)

// Submit the request to the smart contract
await contract.verifyStringRequest(didAddress, requestParams, signedRequest, userContextProof)
```

We assume the smart contract supports `nonce(didAddress)` method for fetching the next `nonce` for a given DID (this will be defined later).

We assume there is a method `getAddressFromDid(did: string)` that converts `did:vda``:polamoy:0x...` to `0x...`.

### Verify and use the smart contract request[​](https://developers.verida.network/docs/extensions/personal-data-bridge#verify-and-use-the-smart-contract-request) <a href="#verify-and-use-the-smart-contract-request" id="verify-and-use-the-smart-contract-request"></a>

Your smart contract will require `@verida/vda-verification-contract`.

Let's define the basic smart contract that will define the `verifyStringRequest()` method:

```typescript
// Import the Verida Verification Base Contract
import "@verida/vda-verification-contract/contracts/VDAVerificationContract.sol";

contract TestContract is VDAVerificationContract {
    function initialize() public initializer {
        __VDAVerificationContract_init();   
    }

    function verifyStringRequest(
        address did, 
        bytes calldata params, 
        bytes calldata signature, 
        bytes calldata proof
    ) external {
        // Verify the `params` were signed by the requesting `did` and has the correct nonce to prevent replay attacks
        // Uses the VDAVerificationContract.verifyRequest() method that handles the verification
        verifyRequest(did, params, signature, proof);

        // If we progressed this far the request can be trusted
        // We can now decode the request parameters and use them in the smart contract
        string memory message;
        (message) = abi.decode(params, (string));

        // Custom code that uses `message`
    }
}
```

NOTE: `VDAVerificationContract.sol` supports `nonce()` method and the `verifyRequest()` always checks the signed nonce is the next value.

## Usage: Submit a verified request to a smart contract WITH trusted off-chain data[​](https://developers.verida.network/docs/extensions/personal-data-bridge#usage--submit-a-verified-request-to-a-smart-contract-with-trusted-off-chain-data) <a href="#usage--submit-a-verified-request-to-a-smart-contract-with-trusted-off-chain-data" id="usage--submit-a-verified-request-to-a-smart-contract-with-trusted-off-chain-data"></a>

Let's improve on the above example by also including signed data that can then be verified and used on-chain.

### Signing credit score data[​](https://developers.verida.network/docs/extensions/personal-data-bridge#signing-credit-score-data) <a href="#signing-credit-score-data" id="signing-credit-score-data"></a>

In this example, let's assume a credit rating agency has a Verida DID and generates a signed proof that a user has a credit rating of 9:

```typescript
const CONTEXT_NAME = 'Credit Rating Company: Credit Score Verifier'
const creditCompanyKeyring = creditCompanyVeridaAccount.keyring(CONTEXT_NAME)   // account is a DID controlled by the credit rating company
const creditCompanyDid = await account.did()

const creditScore = 9

// The Verida DID of the customer. This will be sent by the customer when
// requesting their proof of credit score, likely through a Verida inbox message
// or by the user logging into a dApp managed by the credit rating company
const customerDid = 'did:vda:polamoy:0x...'

// Generate a proof string that specifies the credit score for the customer DID
const proofString = `${customerDid}-${creditScore}`

// Credit rating company signs the proof string which can be independently verified later
const customerCreditRatingProof = creditCompanyKeyring.sign(proofString)
```

At this point, the `customerCreditRatingProof` and `creditScore` would be sent back to the customer for them to store off-chain, typically in their Verida Wallet. The customer could privately send the proof to a third party (via the Verida network or any other communication channel) or submit it to a smart contract.

### Generate smart contract request (client side)[​](https://developers.verida.network/docs/extensions/personal-data-bridge#generate-smart-contract-request-client-side-1) <a href="#generate-smart-contract-request-client-side-1" id="generate-smart-contract-request-client-side-1"></a>

We will now submit `customerCreditRatingProof` to a smart contract that trusts `creditCompanyDid` and will verify they can trust the credit rating data.

Lets assume the customer is now logged into the `No Deposit Loans` dApp run by `Onchain Loan Company` that trusts `Credit Rating Company`. The customer will generate a verified request to the smart contract that includes the necessary data for the smart contract to verify their credit score on-chain.

```typescript
////////////////////////////////////////////////////////////////////////////////////////////////////
// First, assume we have the data previously generated by `Credit Rating Company` (see above)
const creditScore = 9 (see above)
const customerCreditRatingProof = '0x...' (see above)
const creditCompanyDid = 'did:vda:polamoy:0x...'
const creditCompanyDidAddress = getAddressFromDid(creditCompanyDid)
const CONTEXT_NAME_CREDIT_SCORE_VERIFIER = 'Credit Rating Company: Credit Score Verifier'

// The smart contract will need to verify the credit company signed the proof, so we need
// the credit company proof string from their DID Document for the context that signed
// the proof (Credit Rating Company: Credit Score Verifier)
const creditCompanyDidDocument = await didClient.get(creditCompanyDid)
const creditCompanyContextProof = customerDidDocument.locateContextProof(CONTEXT_NAME_CREDIT_SCORE_VERIFIER)

////////////////////////////////////////////////////////////////////////////////////////////////////
// Second, we build the information necessary for the customer to generate the smart contract request
const CONTEXT_NAME = 'OnChain Loan Company: No Deposit Loans'
const customerKeyring = customerVeridaAccount.keyring(CONTEXT_NAME)

// Fetch the DID of the customer
const customerDid = await customerVeridaAccount.did()

// Strip the address part of the DID for increased on-chain efficiency
const customerDidAddress = getAddressFromDid(customerDid)

// Fetch the next nonce for the customer DID from the smart contract, to include in our signed request
const customerDidNonce = (await contract.nonce(customerDidAddress)).toNumber()

// Fetch the proof that verifies the customer keyring signing context is controlled by the customer DID
const customerDidDocument = await didClient.get(customerDid)
const customerContextProof = customerDidDocument.locateContextProof(CONTEXT_NAME)

////////////////////////////////////////////////////////////////////////////////////////////////////
// Third, we generate the parameters that will be sent to the smart contract
const requestParams = ethers.utils.AbiCoder.prototype.encode(
    ['uint', 'bytes', 'bytes'],
    [creditScore, customerCreditRatingProof, creditCompanyContextProof]
)

// Sign the request parameters with the nonce
const requestParamsWithNonce = ethers.utils.solidityPack(
    ['bytes', 'uint'],
    [requestParams, userDidNonce]
)
const signedRequest = await customerKeyring.sign(requestParamsWithNonce)

////////////////////////////////////////////////////////////////////////////////////////////////////
// Finally, we submit the request to the smart contract
await contract.submitCreditScore(getAddressFromDid(customerDid), requestParams, signedRequest, customerContextProof)
```

### Verify and use the credit score data on-chain[​](https://developers.verida.network/docs/extensions/personal-data-bridge#verify-and-use-the-credit-score-data-on-chain) <a href="#verify-and-use-the-credit-score-data-on-chain" id="verify-and-use-the-credit-score-data-on-chain"></a>

Let's update the basic smart contract with a new `submitCreditScore()` method:

```typescript
// Import the Verida Verification Base Contract
import "./VDAVerificationContract.sol";

contract TestContract is VDAVerificationContract {
    function initialize() public initializer {
        __VDAVerificationContract_init();   
    }

    function submitCreditScore(
        address did, 
        bytes calldata params, 
        bytes calldata signature, 
        bytes calldata proof
    ) external {
        // Verify the `params` were signed by the requesting `did` and has the correct nonce to prevent replay attacks
        // Uses the VDAVerificationContract.verifyRequest() method that handles the verification
        // (This is exactly the same as the previous example)
        verifyRequest(did, params, signature, proof);

        // If we progressed this far the request can be trusted
        // We can now decode the request parameters
        uint memory creditScore;
        bytes memory customerCreditRatingProof;
        bytes memory creditCompanyContextProof;

        (creditScore, customerCreditRatingProof, creditCompanyContextProof) = abi.decode(params, (uint, bytes, bytes));

        // We now need to verify if the customer credit rating can be trusted
        // For now, let's hard code the DID of the credit company (should match `creditCompanyDidAddress` from the client side)
        address trustedCreditCompanies = ['0x...']

        // Verify the data is signed by a trusted credit company
        verifyDataWithSigners(bytes(creditScore), customerCreditRatingProof, creditCompanyContextProof, trustedCreditCompanies);

        // If we progressed this far the `creditScore` can be trusted and it can be used in the smart contract
        
        // Do something... like issue a loan based on the customer's trusted credit score!
    }
}
```

### Trusted signers[​](https://developers.verida.network/docs/extensions/personal-data-bridge#trusted-signers) <a href="#trusted-signers" id="trusted-signers"></a>

In the above example we hardcoded an array of addresses (`trustedCreditCompanies`) with a single trusted DID.

`VDAVerificationContract.sol` provides an easier way to manage this from your smart contracts.

There are methods that can be executed by the smart contract owner:

* `addTrustedSigner(address didAddress)`
* `removeTrustedSigner(address didAddress)`

This maintains an internal list of trusted DID addresses. In the above example, this might be a list of 8 credit rating company DID's that are trusted by the smart contract.

You can use this inbuilt list stored in the smart contract, by calling `verifyData()` instead of `verifyDataWithSigners()`:

```typescript
verifyData(bytes(creditScore), customerCreditRatingProof, creditCompanyContextProof);
```

## Multi-chain[​](https://developers.verida.network/docs/extensions/personal-data-bridge#multi-chain) <a href="#multi-chain" id="multi-chain"></a>

One of the huge benefits of the Verida DID's not being linked to any particular blockchain, means this implementation can be replicated across any chain that supports `keccak256` hashing and `ed25519` signatures (basically every blockchain).

All that's required to support a new chain is to port the `VDAVerificationContract.sol` to the new chain and use it within smart contracts on that chain.

This also means that user's can have their data verified once and then use it across multiple smart contracts across multiple blockchains without having to re-verify their data.
