# Command Line Tools

## Installation

```bash
yarn global add @verida/cli-tools
```

## Available Commands

### Create Account

Create a new account on the Verida Network.

You will need to specify a Polygon POS private key that has sufficient tokens to pay for the on-chain transaction to register the identity on the network.

```bash
yarn verida-cli CreateAccount --help
```

### Get Account Information

Get the DID, private key and public key for a known Verida Account. Accepts a private key or seed phrase.

```bash
yarn verida-cli GetAccountInfo -help
```

### Get DID Document

Get the DID Document for a Verida DID.

```bash
yarn verida-cli GetDIDDocument --help
```

### Get Profile

Get the public profile of a Verida Account (if set).

```bash
yarn verida-cli GetProfile --help
```

### Set Profile

Set the public profile data of a Verida Account.

```bash
yarn verida-cli SetProfile --help
```

### Send Message

Send a test inbox message to a DID.

```bash
yarn verida-cli SendInboxMessage --help
```



