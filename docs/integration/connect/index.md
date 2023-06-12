# Get Accounts

In order to allow a wallet to collect information from the blockchain,
Keystone would need to provide the account information, e.g. public key or extended public key,
to the watch-only wallet in which the wallet will use to
generate addresses and query the necessary information from the blockchain.

We use the different standard to encode the account information.

Currently Supports
- **HD Key**, for single public key or extended public key
- **Multiple Accounts**, for multiple public keys and extended public keys of different paths

Special Cases
- **Arweave Account**, only if you need an Arweave account from Keystone
- **XRP Account**, only if you need an XRP account from Keystone

![](/_media/connect.png)

> [!TIP|labelVisibility:hidden|iconVisibility:hidden]
> We build an [Android demo app](https://github.com/KeystoneHQ/keystone-sdk-android-demo/) and [iOS demo app](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/)
> using `KeystoneSDK` to parse account QR code from Keystone hardware wallet.
