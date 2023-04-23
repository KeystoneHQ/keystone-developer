# Aptos

## Display Unsigned Transaction

For passing an unsigned Aptos transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
signType: Enum // supported data type. Currently supports single, multi and message
chainId: Int // the EVM chain ID
accounts: Array (
    path: String // the HD path to tell which private key should be used to sign the data
    xfp: String // master fingerprint provided by Keystone when getting accounts
    key: String(Optional) // the public key for request this signing
)
origin: String(Optional) // source of the request, wallet name etc
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

let aptosSignRequest = AptosSignRequest(
    requestId: "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
    signData: "4150544F530A6D6573736167653A207665726966795F77616C6C65740A6E6F6E63653A20373134363136353534363430333235393636333033313734",
    signType: .single,
    accounts: [
        AptosSignRequest.Account(path: "m/44'/637'/0'/0'/0'", xfp: "f23f9fd2")
    ],
    origin: "Petra"
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.aptos.generateSignRequest(aptosSignRequest: aptosSignRequest);

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Aptos.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val aptosSignRequest = AptosSignRequest(

)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.aptos.generateSignRequest(aptosSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```
An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

<!-- tabs:end -->

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.
> Note:
> The bigger the fragment length, the harder the Keystone scans it.

The QR code generated for the unsigned message above.

![](/_media/sign-aptos-message.png ':size=200')

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
    let signature = try keystoneSDK.aptos.parseSignature(ur: decodedQR)
}
```
An example of continues scanning and parsing an Ethereum signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Aptos.swift)

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.aptos.parseSignature(decodedQR)
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
