# Get Accounts

In order to allow a wallet to collect information from the blockchain, 
Keystone would need to provide the account information, e.g. public key or extended public key,
to the watch-only wallet in which the wallet will use to 
generate addresses and query the necessary information from the blockchain.

We use the different standard to encode the account information.

![](/_media/connect.png)

## Multiple Accounts

Keystone hardware wallet uses [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) to encode accounts when 
a single QR code is not able to contain all the account information. Multiple QR code content is needed for Keystone SDK
to recover the account information provided by the Keystone hardware wallet.


<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns MultiAccounts object, or `nil` when more QR codes are needed
let decodedQR = try sdk.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let accounts = try sdk.parseMultiAccounts(cborHex: ur.cbor)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/MultiAccountsView.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns MultiAccounts object, or `null` when more QR codes are needed
val decodedQR = sdk.decodeQR(qrCodeString)
if (decodedQR != null) {
    val accounts = sdk.parseMultiAccounts(decodedQR.cbor)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt).

<!-- tabs:end -->


The `MultiAccounts` information data structure

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
struct MultiAccounts {
    public var masterFingerprint: String  // A 4 bytes hex string indicates the current mnemonic, e.g. 'f23f9fd2'
    public var device: String  // The device name
    public var keys: Array<Account> // An array of public keys
}

struct Account {
    public var chain: String  // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
    public var path: String  // The full derivation path of current key
    public var publicKey: String // Public key in hex string
    public var name: String  // The address name in hardware wallet
    public var extendedPublicKey: String // The bip32 extended public key in hex string
}
```

#### **Android(Kotlin)**

```kotlin
data class MultiAccounts (
    val device: String,  // A 4 bytes hex string indicates the current mnemonic, e.g. 'f23f9fd2'
    val keys: Array<Account>,  // The device name
    val masterFingerprint: String,  // An array of public keys
)

data class Account(
    var chain: String,  // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
    var path: String,  // The full derivation path of current key
    var publicKey: String, // Public key in hex string
    var name: String,  // The address name in hardware wallet
)
```

<!-- tabs:end -->

#### Example

Here is a QR code which contains the account information of Solana for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-multi-accounts-sol.png ':size=250')

The account information contains in the QR code

```
MultiAccounts (
    masterFingerprint: "f23f9fd2",
    device: "Keystone",
    keys: [
        Account(
            chain: "SOL",
            path: "m/44\'/501\'/O\'",
            publicKey: "b6a368936bbd2ed057f0de520a32bd9486591f7677410d0f137bc68b6a72d47",
            name: "SOL-O",
            chainCode: ""',
            extendedPublicKey: ""
        ),
        Account(
            chain: "SOL",
            path: "m/44\'/501\'/1\'",
            publicKey: "1b3087a6762c569ab85e653634802c680c0063b8b2ec4141fd8d93b6ea5aa3",
            name: "SOL-1",
            chainCode: "",
            extendedPublicKey: ""
        )
    ]
)
```
