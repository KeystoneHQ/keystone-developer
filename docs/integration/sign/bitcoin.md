# Bitcoin

## Display Unsigned Transaction

PSBT ser
Put the return result of `nextPart` into QR code.

<!-- tabs:start -->

#### **Swift**


PSBT: Data

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()
let qrCode = keystoneSDK.btc.generatePSBT(psbt)

qrCode.nextPart() // ur:crypto-psbt/1-2/lpadaocsptcybkgdcarhhdgohdosjojkidjyzmadaenyaoaeaeaeaohdvsknclrejnpebncnrnmnjojofejzeojlkerdonspkpkkdkykfelokgprpyutkpaeaeaeaeaezmzmzmzmlslgaaditiwpihbkispkfgrkbdaslewdfycprtjsprsgksecdratkkhktimndacnch
qrCode.nextPart() // ur:crypto-psbt/2-2/lpaoaocsptcybkgdcarhhdgokewdcaadaeaeaeaezmzmzmzmaojopkwtayaeaeaeaecmaebbtphhdnjstiambdassoloimwmlyhygdnlcatnbggtaevyykahaeaeaeaecmaebbaeplptoevwwtyakoonlourgofgvsjydpcaltaemyaeaeaeaeaeaeaeaeaeaeswhhtptt
```

#### **Kotlin**

PSBT: ByteArray


```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.btc.generatePSBT(psbt)

//
qrCode.nextPart() //
qrCode.nextPart() //
```

<!-- tabs:end -->

## Get Signature from Keystone

PSBT ur -> PSBT ser

<!-- tabs:start -->

#### **Swift**

```swift
import KeystoneSDK

let sdk = KeystoneSDK()

let decodedQR = try sdk.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let psbt = try sdk.btc.parsePSBT(cborHex: decodedQR.cbor)
}
```

#### **Kotlin**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val psbt = keystoneSDK.btc.parsePSBT(decodedQR.cbor)
}
```

<!-- tabs:end -->
