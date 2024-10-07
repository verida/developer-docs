# Authentication Server

The [Verida Vault Authentication Server](https://github.com/verida/vault-auth-server/) is a web socket server that bridges connections between a mobile device and a web page to enable a secure authentication using private blockchain keys stored on the mobile device.

This is similar to how WalletConnect bridges between a mobile wallet and a web application for blockchain transactions, however Verida enables database storage connections. In the future, it may be possible to leverage WalletConnect with additional extensions to replace this infrastructure.

Verida is currently running a server for both Verida Banksia and Myrtle that is used by default by all applications.

### Usage[​](https://developers.verida.network/docs/single-sign-on-sdk/sso-authentication-server#usage) <a href="#usage" id="usage"></a>

To start your application, use `npm run start`

### Configuration[​](https://developers.verida.network/docs/single-sign-on-sdk/sso-authentication-server#configuration) <a href="#configuration" id="configuration"></a>

There is an example configuration file located in `src/config/index.example.js`. Copy this file to `src/config/index.js`.

You must update `AUTH_URI` to match the domain name / IP address of the server. You will also need to configure the Auth Client library to use this value as the `serverUri`. See [Verida Connect SDK / Getting Started /Configuration.](getting-started.md#configuration)

You must configure each application this server will support in the `CONTEXTS` section. You can specify a different private key per context. All data is encrypted and signed using the private key. This enables the data to be decrypted by the end user and in the future, will enable Application Contexts to be registered to specific keys, providing enhanced man-in-the-middle attack protection.

You must specify valid `DID_CLIENT_CONFIG` if you are registering a new Verida Account (DID) with the private key of a context. See [Client SDK / Authentication / Private Key.](../client-sdk/authentication.md#id-2-private-key)

## Security[​](https://developers.verida.network/docs/single-sign-on-sdk/sso-authentication-server#security) <a href="#security" id="security"></a>

The configuration file allows you to provide the private key of a valid blockchain account that can sign messages relating to the login process.

There is a `loginOrigin` property that, if specified, will check the `origin` HTTP header from each socket request to ensure it’s coming from the expected domain. This ensures third-party websites can’t easily request valid authentication tokens. This also ensures third party websites can be prevented from using the resources of any auth server that is running.

Malicious third parties could obtain a token by spoofing the `origin` HTTP header and then presenting that to the user. However, this `loginOrigin` property is passed inside the encrypted payload to the Verida Wallet and is displayed to the user. This allows the user to visually verify the domain name they are currently on matches the domain name displayed on the Verida Wallet login screen. In the future, the Verida Trust Framework will add an additional layer of security by matching on chain metadata against the public key and domain name used to sign the payload.

## Production[​](https://developers.verida.network/docs/single-sign-on-sdk/sso-authentication-server#production) <a href="#production" id="production"></a>

We recommend using the [PM2](https://www.npmjs.com/package/pm2) package to manage running the server.

Starting the server:

```bash
cd <vault-auth-server-location>
pm2 start ~/.nvm/versions/node/v12.14.1/bin/yarn --name vault-auth-server -- serve
```

Restarting the server:

```bash
pm2 restart vault-auth-server -- serve
```

Stopping the server:

```bash
pm2 stop vault-auth-server -- serve
```

Monitoring the server:

```bash
pm2 monit
```

{% hint style="info" %}
It’s also recommended to install `pm2-logrotate` which is useful to manage logs on the server.
{% endhint %}
