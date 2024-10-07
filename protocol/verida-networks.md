---
cover: ../.gitbook/assets/Screenshot 2024-08-05 131304.png
coverY: 0
---

# Verida Networks

| Name                  | Description                                                                                                                                | Myrtle (mainnet)                                                                                      | Banksia (testnet)                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| Storage Node Registry | List of all storage nodes on the network. This will eventually be replaced with a smart contract.                                          | [myrtle.json](https://assets.verida.io/registry/storageNodes/myrtle.json)                             | [banksia.json](https://assets.verida.io/registry/storageNodes/banksia.json)                         |
| Auth Server           | A generic authentication server operated by the Verida Foundation to facilitate SSO between dApps and the Verida Wallet mobile application | [link](https://auth.testnet.verida.io/) (shared between Myrtle and Banksia)                           | [link](https://auth.testnet.verida.io/) (shared between Myrtle and Banksia)                         |
| DID Registry          | Smart contract that maintains a registry of all DID accounts on the Verida network                                                         | [Polygon PoS Mainnet](https://www.polygonscan.com/address/0x6FF180EF62FA57e611E91bdDaDadB6635D6b9Af7) | [Polygon PoS Amoy](https://amoy.polygonscan.com/address/0x5CC5cf757C0f2af7b3935093F88EaF45c5210002) |
| Name Registry         | Smart contract that maintains a mapping of Verida network usernames to DIDs                                                                | [Polygon PoS Mainnet](https://www.polygonscan.com/address/0xc9ce048b464034C53207Bf120bF85f317fdb38C8) | [Polygon PoS Amoy](https://amoy.polygonscan.com/address/0x91381c424485dc12650811601d9a8B0025e51afc) |
| Verida Token          | Smart contract for the Verida Token (VDA)                                                                                                  | [Polygon PoS Mainnet](https://www.polygonscan.com/address/0x683565196C3EAb450003C964D4bad1fd3068D4cC) | [Polygon PoS Amoy](https://amoy.polygonscan.com/address/0xC3D1eB4E0241a4A2B859f91dd2a6aDA176cCB6F2) |
| Verida Rewards        | Smart contract that manages rewards for users who claim their data from centralized platforms                                              | [Polygon PoS Mainnet](https://www.polygonscan.com/address/0xBAeEA910f6BBe29Ef33e0051e51dc60f9702B7b9) | [Polygon PoS Amoy](https://amoy.polygonscan.com/address/0x4BDf0193aF01dF6b6Ff14A97eECE42071575d706) |
| Network Explorer      | Simple interface to view the activity on the Verida Network                                                                                | [link](https://explorer.verida.network)                                                               | [link](https://explorer.banksia.verida.network)                                                     |

The protocol also maintains a [registry of smart contract addresses](https://github.com/verida/verida-js/blob/main/packages/vda-common/src/contract.ts) in code.

Verida currently operates all the storage nodes on the network while it measures performance and rapidly resolves issues during development.

We will soon be opening up storage nodes for third parties to operate. You can [learn more here](https://news.verida.io/decentralizing-the-verida-network-become-a-node-operator-e31a6a31daf1) or [register your interest in providing storage nodes for the network](https://www.verida.network/storage-node-providers).
