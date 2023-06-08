# Get Accounts

In order to allow a wallet to collect information from the blockchain, 
Keystone would need to provide the account information, e.g. public key or extended public key,
to the watch-only wallet in which the wallet will use to 
generate addresses and query the necessary information from the blockchain.

We use the different standard to encode the account information.

Currently Supports
- **HD Key**, for single public key or extended public key
- **Multiple Accounts**, for multiple public keys and extended public keys of different paths 

![](/_media/connect.png)

> [!TIP|labelVisibility:hidden|iconVisibility:hidden]
> We build an [Android demo app](https://github.com/KeystoneHQ/keystone-sdk-android-demo/) and [iOS demo app](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/)
> using `KeystoneSDK` to parse account QR code from Keystone hardware wallet.


## HD Key

Keystone uses [HDKey](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-007-hdkey.md) for encoding and transmitting HDKeys.


<!-- tabs:start -->

#### **Web(Typescript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

export const Account = () => {

  const onSucceed = ({type, cbor}) => {
    const account = KeystoneSDK.parseHDKey(new UR(Buffer.from(cbor, "hex"), type))
    console.log("account: ", account);
  }
  const onError = (errorMessage) => {
    console.log("error: ",errorMessage);
  }

  return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CryptoHDKey]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->


The `Account` data structure

```js
chain: String // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
path: String // The derivation path of current key
publicKey: String // The public key in hex string
name: String // The name of hardware wallet
xfp: String // The master fingerprint
chainCode: String // The chain code
extendedPublicKey: Optional(String) //  The account extended public key
note: Optional(String) // The note, e.g. "account.standard", "account.ledger_live"
```

#### Example

Here is a QR code which contains the account information of Ethereum for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-hdkey-eth.png ':size=250')

The account information in the QR code

<!-- tabs:start -->

#### **Web(Typescript)**

```json
{
    "chain": "ETH",
    "path": "m/44'/60'/0'",
    "publicKey": "02cc6d7834204653ff10e0047a2395343cc6df081e76c88d5eee83f346f0b21cb7",
    "name": "Keystone",
    "xfp": "f23f9fd2",
    "chainCode": "712a9187e5c60c573a5acce855445376e1b74c240e417fe8cb2a8fdfd78d2d9d",
    "extendedPublicKey": "xpub6CBZfsQuZgVnvTcScAAXSxtX5jdMHtX5LdRuygnTScMBbKyjsxznd8XMEqDntdY1jigmjunwRwHsQs3xusYQBVFbvLdN4YLzH8caLSSiAoV"
}
```
<!-- tabs:end -->

## Multiple Accounts

Keystone hardware wallet uses [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) to encode accounts when 
a single QR code is not able to contain all the account information. Multiple QR code content is needed for Keystone SDK
to recover the account information provided by the Keystone hardware wallet.


<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns scan progress and data, progress range 0 - 100.
let result = try sdk.decodeQR(qrCode: qrCodeString)
if result.progress == 100 {
    let accounts = try sdk.parseMultiAccounts(ur: result.ur!)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/MultiAccountsView.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns scan progress and data, progress range 0 - 100.
val decodedResult = sdk.decodeQR(qrCodeString)
if (decodedQR.progress == 100) {
    val accounts = sdk.parseMultiAccounts(decodedResult.ur!!)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt).

#### **Web(Typescript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const MultiAccounts = () => {
    const onSucceed = ({type, cbor}) => {
        const multiAccounts = KeystoneSDK.parseMultiAccounts(new UR(Buffer.from(cbor, "hex"), type))
        console.log("multiAccounts: ", multiAccounts);
    }
    const onError = (errorMessage) => {
        console.log("error: ",errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CryptoMultiAccounts]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->


The `MultiAccounts` information data structure

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
struct MultiAccounts {
    public var masterFingerprint: String  // A 4 bytes hex string indicates the current mnemonic, e.g. 'f23f9fd2'
    public var keys: Array<Account> // An array of public keys
    public var device: String?  // The device name, e.g. 'Keystone'
    public var deviceId: String?  // The device id, e.g. 28475c8d80f6c06bafbe46a7d1750f3fcf2565f7
}

struct Account {
    public var chain: String  // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
    public var path: String  // The full derivation path of current key
    public var publicKey: String // Public key in hex string
    public var name: String  // The address name in hardware wallet
    public var note: String?  // The note for current account
    public var extendedPublicKey: String // The bip32 extended public key, e.g. xpub6BwgsDRPiuArdfKHXD2FdF1cnXK4WSJCNUbZaAj9AbkdGrBoJM3B834xVAqfLR8eFh6VwLzVk8NKALiZgyytwMmayc6z7n9zSFjBjdkkKrk
}
```

The note field in account only used for `ETH` at the moment. Keystone SDK has an enum `AccountNote` for it.

```swift
public enum AccountNote: String {
    case standard = "account.standard"  // The BIP 44 standard xpub
    case ledgerLegacy = "account.ledger_legacy" // The Ledger Legacy xpub, same as BIP 44 xpub
    case ledgerLive = "account.ledger_live" // The Ledger Live public key
}
```

#### **Android(Kotlin)**

```kotlin
data class MultiAccounts (
    val masterFingerprint: String,  // A 4 bytes hex string indicates the current mnemonic, e.g. 'f23f9fd2'
    val keys: Array<Account>,  // An array of public keys
    val device: String?,  // The device name, e.g. 'Keystone'
    val deviceId: String?,  // The device id, e.g. 28475c8d80f6c06bafbe46a7d1750f3fcf2565f7
)

data class Account(
    var chain: String,  // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
    var path: String,  // The full derivation path of current key
    var publicKey: String, // Public key in hex string
    var name: String,  // The address name in hardware wallet
    var note: String?,  // The note for current account
){ fun getExtendedPublicKey(): String } // The bip32 extended public key, e.g. xpub6BwgsDRPiuArdfKHXD2FdF1cnXK4WSJCNUbZaAj9AbkdGrBoJM3B834xVAqfLR8eFh6VwLzVk8NKALiZgyytwMmayc6z7n9zSFjBjdkkKrk
```

The note field in account only used for `ETH` at the moment. Keystone SDK has an enum `Note` for it.

```kotlin
enum class Note(val value: String) {
    STANDARD("account.standard"), // The BIP 44 standard xpub
    LEDGER_LEGACY("account.ledger_legacy"), // The Ledger Legacy xpub, same as BIP 44 xpub
    LEDGER_LIVE("account.ledger_live"), // The Ledger Live public key
}
```

#### **Web(Typescript)**

```typescript
interface MultiAccounts {
    masterFingerprint: string  // A 4 bytes hex string indicates the current mnemonic, e.g. 'f23f9fd2'
    keys: Account[]  // An array of public keys
    device?: string  // The device name, e.g. 'Keystone'
    deviceId?: string  // The device id, e.g. '28475c8d80f6c06bafbe46a7d1750f3fcf2565f7'
}

interface Account {
    chain: string  // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
    path: string  // The full derivation path of current key
    publicKey: string  // Public key in hex string
    name?: string  // The address name in hardware wallet
    chainCode: string  // The chain code if exist
    extendedPublicKey?: string  // The bip32 extended public key, e.g. xpub6BwgsDRPiuArdfKHXD2FdF1cnXK4WSJCNUbZaAj9AbkdGrBoJM3B834xVAqfLR8eFh6VwLzVk8NKALiZgyytwMmayc6z7n9zSFjBjdkkKrk
    note?: string  // The note for current account
}
```

The note field in account only used for `ETH` at the moment. Keystone SDK has an enum `AccountNote` for it.

```typescript
enum AccountNote {
    Standard = 'account.standard', // The BIP 44 standard xpub
    LedgerLegacy = 'account.ledger_legacy', // The Ledger Legacy xpub, same as BIP 44 xpub
    LedgerLive = 'account.ledger_live' // The Ledger Live public key
}
```

<!-- tabs:end -->

#### Example

Here is a QR code which contains the account information of Solana for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-multi-accounts-sol.png ':size=250')

The account information contains in the QR code

<!-- tabs:start -->

#### **iOS(Swift)**

```
MultiAccounts (
    masterFingerprint: "f23f9fd2",
    keys: [
        Account(
            chain: "SOL",
            path: "m/44\'/501\'/O\'",
            publicKey: "b6a368936bbd2ed057f0de520a32bd9486591f7677410d0f137bc68b6a72d47",
            name: "SOL-O",
            chainCode: "",
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
    ],
    device: "Keystone",
)
```

#### **Android(Kotlin)**

```
MultiAccounts (
    masterFingerprint: "f23f9fd2",
    keys: [
        Account(
            chain: "SOL",
            path: "m/44\'/501\'/O\'",
            publicKey: "b6a368936bbd2ed057f0de520a32bd9486591f7677410d0f137bc68b6a72d47",
            name: "SOL-O",
            chainCode: "",
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
    ],
    device: "Keystone",
)
```

#### **Web(Typescript)**

```json
{
    "masterFingerprint": "f23f9fd2",
    "keys": [
        {
            "chain": "SOL",
            "path": "m/44'/501'/0'",
            "publicKey": "b6a3b8936bbd2ed057f0de520a3f2bd9486591f7b77410d0f137bc68b6a72d47",
            "name": "SOL-0",
            "chainCode": "",
            "extendedPublicKey": ""
        },
        {
            "chain": "SOL",
            "path": "m/44'/501'/1'",
            "publicKey": "1b3087aa6762c569fab85e653634802c680c0063b8b2ec4141fd8d93b6ea5aa3",
            "name": "SOL-1",
            "chainCode": "",
            "extendedPublicKey": ""
        }
    ],
    "device": "Keystone"
}
```
<!-- tabs:end -->
