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
// `decodeQR` decodes given QR code content and returns accounts, or `nil` when more QR codes are needed 
let decodedQR = try sdk.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let accounts = try sdk.parseMultiAccounts(cborHex: ur.cbor)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/MultiAccountsView.swift).

The MultiAccounts information structure

```swift
struct MultiAccounts {
    public var masterFingerprint: String
    public var device: String
    public var keys: Array<Account>
}

struct Account {
    public var chain: String
    public var path: String
    public var publicKey: String
    public var name: String
    public var chainCode: String
    public var extendedPublicKey: String
}
```


#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns accounts, or `null` when more QR codes are needed
val decodedQR = sdk.decodeQR(qrCodeString)
if (decodedQR != null) {
    val accounts = sdk.parseMultiAccounts(decodedQR.cbor)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt).

The MultiAccounts information structure

```kotlin
data class MultiAccounts (
    val device: String,
    val keys: Array<Account>,
    val masterFingerprint: String,
)

data class Account(
    var chain: String,
    var path: String,
    var publicKey: String,
    var name: String,
    private var chainCode: String,
    private var extendedPublicKey: String,
)
```

<!-- tabs:end -->
