# Multiple Accounts

Multiple accounts is used when multiple public keys or extended public keys needs to be extract from Keystone.

## Data Structure

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

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

#### **<span class="kotlin">Android(Kotlin)</span>**

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

#### **<span class="typescript">Web(TypeScript)</span>**

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

## Example

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

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

#### **<span class="kotlin">Android(Kotlin)</span>**

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

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const MultiAccounts = () => {
    const onSucceed = ({type, cbor}) => {
        const multiAccounts = KeystoneSDK.parseMultiAccounts(new UR(Buffer.from(cbor, "hex"), type))
        console.log("multiAccounts: ", multiAccounts);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CryptoMultiAccounts]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The QR code which contains the account information of Solana for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-multi-accounts-sol.png ':size=250')

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> Keystone hardware wallet uses [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) to encode accounts when
> a single QR code is not able to contain all the account information. Multiple QR code content is needed for Keystone SDK
> to recover the account information provided by the Keystone hardware wallet.

The account information contains in the QR code

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
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

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
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

#### **<span class="typescript">Web(TypeScript)</span>**

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
