---
hidden: true
---

# Migrate to Myrtle

The Verida Mainnet is now live and the Verida protocol libraries have been updated to manage most of the complexity of the myrtle upgrade.

As a developer, there are just three things you need to focus on.

### 1. Upgrade to the latest libraries[​](https://developers.verida.network/docs/extras/migrate-mainnet#1-upgrade-to-the-latest-libraries) <a href="#id-1-upgrade-to-the-latest-libraries" id="id-1-upgrade-to-the-latest-libraries"></a>

There is a new `3.0.0` release of the protocol that supports myrtle.

You will need to run `yarn update` on all `@verida/xxx` packages used by your project.

ie: `yarn update @verida/client-ts @verida/account-web-vault`

### 2. Specify Verida EnvironmentType.MYRTLE[​](https://developers.verida.network/docs/extras/migrate-mainnet#2-specify-verida-environmenttypemainnet) <a href="#id-2-specify-verida-environmenttypemainnet" id="id-2-specify-verida-environmenttypemainnet"></a>

In your code that connects to the Verida network, replace references of `EnvironmentType.BANKSIA` to `EnvironmentType.MYRTLE`

### 3. Server-side app changes[​](https://developers.verida.network/docs/extras/migrate-mainnet#3-server-side-app-changes) <a href="#id-3-server-side-app-changes" id="id-3-server-side-app-changes"></a>

(Not required for web applications)

Server-side applications may be manually creating accounts on the Verida network and paying MATIC for those blockchain transactions. The Verida mainnet runs on a Polygon PoS network, so you will need to replace your [authentication code](http://localhost:3000/docs/client-sdk/authentication#example) `webConfig.privateKey` with a Polygon PoS mainnet private key with sufficient MATIC to create blockchain accounts.
