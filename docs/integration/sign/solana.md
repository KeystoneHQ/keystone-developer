# Solana

## Display Unsigned Transaction

Put the return result of `nextPart` into QR code.

<!-- tabs:start -->

#### **Swift**

```swift
import KeystoneSDK

let solSignRequest = SolSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "48656c6c6f2c204b657973746f6e652e",
    path: "m/44'/501'/0'/0'",
    xfp: "12121212",
    signType: .message,
    origin: "solflare"
)
let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.sol.generateSignRequest(solSignRequest: solSignRequest)

//
let qrContent = qrCode.nextPart()
```

#### **Kotlin**


```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

// Prepare unsigned data
val requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d"
val signData = "48656c6c6f2c204b657973746f6e652e"
val path = "m/44'/501'/0'/0'"
val xfp = "12121212"
val address = ""
val origin = "solflare"
val signType = KeystoneSolanaSDK.SignType.Message

val res = keystoneSDK.sol.generateSignRequest(requestId, signData, path, xfp, address, origin, signType)

//
val qrContent = qrCode.nextPart()
```

<!-- tabs:end -->

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

<!-- tabs:start -->

#### **Swift**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedQR = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let signature = try keystoneSDK.sol.parseSignature(cborHex: decodedQR.cbor)
}
```

#### **Kotlin**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.sol.parseSignature(decodedQR.cbor)
}
```

<!-- tabs:end -->
