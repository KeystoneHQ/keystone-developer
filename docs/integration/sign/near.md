# Near

## Display Unsigned Transaction

For passing an unsigned Near transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: Array(String) // the serialized unsigned transaction data array, each in hex string
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: Optional(String) // source of the request, wallet name etc
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let nearSignRequest = NearSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: ["4000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037009FCC0720A016D3C1E849D86B16D7139E043EFC48ADD1C78F39C3D2F00EE98C07823E0CA1957100004000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037F0787E1CB1C22A1C63C24A37E4C6C656DD3CB049E6B7C17F75D01F0859EFB7D80100000003000000A1EDCCCE1BC2D3000000000000"],
    path: "m/44'/397'/0'",
    xfp: "F23F9FD2",
    origin: "nearwallet"
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.near.generateSignRequest(nearSignRequest: nearSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert a Near transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Near.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val nearSignRequest = NearSignRequest(
    requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData = arrayListOf("4000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037009FCC0720A016D3C1E849D86B16D7139E043EFC48ADD1C78F39C3D2F00EE98C07823E0CA1957100004000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037F0787E1CB1C22A1C63C24A37E4C6C656DD3CB049E6B7C17F75D01F0859EFB7D80100000003000000A1EDCCCE1BC2D3000000000000"),
    path = "m/44'/397'/0'",
    xfp = 'F23F9FD2',
    origin = "nearwallet"
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.near.generateSignRequest(nearSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert a Near transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK  from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let nearTransaction = {
    requestId: '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d',
    signData: ['4000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037009FCC0720A016D3C1E849D86B16D7139E043EFC48ADD1C78F39C3D2F00EE98C07823E0CA1957100004000000039666363303732306130313664336331653834396438366231366437313339653034336566633438616464316337386633396333643266303065653938633037F0787E1CB1C22A1C63C24A37E4C6C656DD3CB049E6B7C17F75D01F0859EFB7D80100000003000000A1EDCCCE1BC2D3000000000000'],
    path: "m/44'/397'/0'",
    xfp: 'F23F9FD2',
    origin: 'nearwallet'
}

const Near = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.near.generateSignRequest(nearTransaction);

    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")}/>
}
```

`AnimatedQRCode` will return the unsigned data as a QR code, or animated QR code if needed.

You can add `option` to `AnimatedQRCode` component to control the size, capacity and the update interval of QR code.
```jsx
options={{
    size: number, // optional, QR code width and length in UI, default 180px
    capacity: number, // optional, the capacity of a single QR code, default 400 bytes per image
    interval: number // optional, the QR code change time interval in mill seconds for animated QR code, default 100ms
}}
```
> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The bigger the capacity, the more difficult it is for Keystone to scan.

<!-- tabs:end -->

The QR code generated for the unsigned message above.

![](/_media/sign-near.png ':size=200')

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

The progress range in the `decodeQR` result `0 - 100`.

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedResult = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedResult.progress == 100 {
    let signature = try keystoneSDK.near.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a Near signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Near.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.near.parseSignature(decodedResult.ur!!)
}
```

An example of continues scanning and parsing accounts data, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const NearScanner = () => {
  const keystoneSDK = new KeystoneSDK();

  const onSucceed = ({type, cbor}) => {
    const signature = keystoneSDK.near.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
    console.log("signature: ", signature);
  }
  const onError = (errorMessage) => {
    console.log("error: ", errorMessage);
  }

  return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.NearSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // The requestId from sign request
    signature: Array(String) // An array of the serialized signatures, each in hex string
)
```
