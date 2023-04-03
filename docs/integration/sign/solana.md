# Solana

## Display Unsigned Transaction

For passing an unsigned Solana transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the unsigned transaction data, in hex string
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
dataType: Enum // supported data type. Currently supports transaction and message
origin: String(Optional) // source of the request, wallet name etc
address: String(Optional) // the address for request this signing
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

> **Note**  
> The Keystone SDK might generate an infinite number of QR codes when the unsigned transaction or message contains a big chunk of data,
> the software wallet will need to show the animated QR codes so that the Keystone hardware wallet can get all the
> transaction data. See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let solSignRequest = SolSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "48656c6c6f2c204b657973746f6e652e",
    path: "m/44'/501'/0'/0'",
    xfp: "F23F9FD2",
    signType: .message,
    origin: "Solflare"
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.sol.generateSignRequest(solSignRequest: solSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction.swift)

#### **Android(Kotlin)**


```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

// Prepare unsigned data
val requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d"
val signData = "48656c6c6f2c204b657973746f6e652e"
val path = "m/44'/501'/0'/0'"
val xfp = "F23F9FD2"
val address = ""
val origin = "Solflare"
val signType = KeystoneSolanaSDK.SignType.Message

val res = keystoneSDK.sol.generateSignRequest(requestId, signData, path, xfp, address, origin, signType)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```
An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

<!-- tabs:end -->

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> Note:
> The bigger the fragment length, the harder the Keystone scans it.

The QR Code the unsigned message generated above.

![](/_media/sign-sol-message.png ':size=300')

> [!ATTENTION]
> The unsigned data might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which the unsigned data is too big and need to be displayed with an animated QR code.

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedQR = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let signature = try keystoneSDK.sol.parseSignature(cborHex: decodedQR.cbor)
}
```
An example of continues scanning and parsing a transaction, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction.swift)

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.sol.parseSignature(decodedQR.cbor)
}
```
An example of continues scanning and parsing accounts data, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature in hex string
)
```
> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> you need to handle the scenario in which Keystone shows it in multiple QR codes.
