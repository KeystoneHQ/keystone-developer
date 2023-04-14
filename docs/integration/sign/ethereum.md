# Ethereum

## Display Unsigned Transaction

For passing an unsigned Ethereum transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
dataType: Enum // supported data type. Currently supports transaction, typed transaction, personal message and typed data
chainId: Int // the EVM chain ID
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
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

let ethSignRequest = EthSignRequest(
    requestId: "6c3633c0-02c0-4313-9cd7-e25f4f296729",
    signData: "48656c6c6f2c204b657973746f6e652e",
    dataType: .personalMessage,
    chainId: 1,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2",
    origin: "MetaMask"
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.eth.generateSignRequest(ethSignRequest: ethSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Ethereum.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val requestId = "6c3633c0-02c0-4313-9cd7-e25f4f296729"
val signData = "48656c6c6f2c204b657973746f6e652e"
val dataType = KeystoneEthereumSDK.DataType.PersonalMessage
val path = "m/44'/60'/0'/0/0"
val xfp = "F23F9FD2"
val address = ""
val origin = "MetaMask"
val chainId = 60

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.eth.generateSignRequest(requestId, signData, dataType, chainId, path, xfp, address, origin)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```
An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

<!-- tabs:end -->

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.
> Note:
> The bigger the fragment length, the harder the Keystone scans it.

The QR code generated for the unsigned message above.

![](/_media/sign-eth-message.png ':size=300')

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
    let signature = try keystoneSDK.eth.parseSignature(ur: decodedQR)
}
```
An example of continues scanning and parsing an Ethereum signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Ethereum.swift)

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.eth.parseSignature(decodedQR)
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
