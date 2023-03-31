# Ethereum

## Display Unsigned Transaction

PSBT ser
Put the return result of `nextPart` into QR code.

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
    xfp: "12345678",
    origin: "metamask"
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.eth.generateSignRequest(ethSignRequest: ethSignRequest)

//
let qrContent = qrCode.nextPart()
```

#### **Android(Kotlin)**

PSBT: ByteArray


```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

// Prepare unsigned data
val requestId = "6c3633c0-02c0-4313-9cd7-e25f4f296729"
val signData = "48656c6c6f2c204b657973746f6e652e"
val dataType = KeystoneEthereumSDK.DataType.PersonalMessage
val path = "m/44'/60'/0'/0/0"
val xfp = "12345678"
val address = ""
val origin = "metamask"
val chainId = 60

val qrCode = keystoneSDK.eth.generateSignRequest(requestId, signData, dataType, chainId, path, xfp, address, origin)

//
val qrContent = qrCode.nextPart()
```

<!-- tabs:end -->

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedQR = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let signature = try keystoneSDK.eth.parseSignature(cborHex: decodedQR.cbor)
}
```

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.eth.parseSignature(decodedQR.cbor)
}
```

<!-- tabs:end -->
