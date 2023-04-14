# Tron(WIP)

> [!NOTE]
> Not available in Keystone hardware wallet at the moment.

## Display Unsigned Transaction

For passing an unsigned Tron transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: String(Optional) // source of the request, wallet name etc
tokenInfo: Optional ( // The token information if you are transfer TRC10 or TRC20 tokens
  name: String // the token name
  symbol: String // the token symbol
  decimals: Number // the decimal of token
)
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

let tronSignRequest = TronSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "0a0207902208e1b9de559665c6714080c49789bb2c5aae01081f12a9010a31747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e54726967676572536d617274436f6e747261637412740a1541c79f045e4d48ad8dae00e6a6714dae1e000adfcd1215410d292c98a5eca06c2085fff993996423cf66c93b2244a9059cbb0000000000000000000000009bbce520d984c3b95ad10cb4e32a9294e6338da300000000000000000000000000000000000000000000000000000000000f424070c0b6e087bb2c90018094ebdc03",
    path: "m/44'/195'/0'/0'",
    xfp: "f23f9fd2",
    tokenInfo: TokenInfo(name: "TONE", symbol: "TronOne", decimals: 8)
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.tron.generateSignRequest(tronSignRequest: tronSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Tron.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val tronSignRequest = TronSignRequest(
    requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData = "0a0207902208e1b9de559665c6714080c49789bb2c5aae01081f12a9010a31747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e54726967676572536d617274436f6e747261637412740a1541c79f045e4d48ad8dae00e6a6714dae1e000adfcd1215410d292c98a5eca06c2085fff993996423cf66c93b2244a9059cbb0000000000000000000000009bbce520d984c3b95ad10cb4e32a9294e6338da300000000000000000000000000000000000000000000000000000000000f424070c0b6e087bb2c90018094ebdc03",
    path = "m/44'/195'/0'/0'",
    xfp = "f23f9fd2",
    tokenInfo = TokenInfo("TONE", "TronOne", 8)
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.tron.generateSignRequest(tronSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```
An example of covert a Tron transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

<!-- tabs:end -->

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.
> Note:
> The bigger the fragment length, the harder the Keystone scans it.

The QR code generated for the unsigned message above.

![](/_media/sign-tron-trc20.gif ':size=300')

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
    let signature = try keystoneSDK.tron.parseSignature(ur: decodedQR)
}
```
An example of continues scanning and parsing a Tron signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Tron.swift)

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.tron.parseSignature(decodedQR)
}
```
An example of continues scanning and parsing QR code, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

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
