# Tron(WIP)

> [!WARNING|labelVisibility:hidden|iconVisibility:hidden|style:flat]
> Not available in Keystone hardware wallet at the moment.

## Display Unsigned Transaction

For passing an unsigned Tron transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: Optional(String) // source of the request, wallet name etc
tokenInfo: Optional ( // The token information is required for TRC10 and TRC20 token transfer
  name: String // the token name, will be shown on Keystone if token symbol is not provided
  symbol: String // the token symbol, will be shown on Keystone
  decimals: Number // the decimal of token
)
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let tronSignRequest = TronSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "0a0207902208e1b9de559665c6714080c49789bb2c5aae01081f12a9010a31747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e54726967676572536d617274436f6e747261637412740a15418dfec1cde1fe6a9ec38a16c7d67073e3020851c0121541a614f803b6fd780986a42c78ec9c7f77e6ded13c2244a9059cbb0000000000000000000000009c0279f1bda9fc40a85f1b53c306602864533e7300000000000000000000000000000000000000000000000000000000000f424070c0b6e087bb2c90018094ebdc03",
    path: "m/44'/195'/0'/0'/0",
    xfp: "F23F9FD2",
    tokenInfo: TokenInfo(name: "TRON_USDT", symbol: "USDT", decimals: 6)
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.tron.generateSignRequest(tronSignRequest: tronSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert a Tron transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Tron.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val tronSignRequest = TronSignRequest(
    requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData = "0a0207902208e1b9de559665c6714080c49789bb2c5aae01081f12a9010a31747970652e676f6f676c65617069732e636f6d2f70726f746f636f6c2e54726967676572536d617274436f6e747261637412740a15418dfec1cde1fe6a9ec38a16c7d67073e3020851c0121541a614f803b6fd780986a42c78ec9c7f77e6ded13c2244a9059cbb0000000000000000000000009c0279f1bda9fc40a85f1b53c306602864533e7300000000000000000000000000000000000000000000000000000000000f424070c0b6e087bb2c90018094ebdc03",
    path = "m/44'/195'/0'/0'/0",
    xfp = "F23F9FD2",
    tokenInfo = TokenInfo("USDT", "TRON_USDT", 8)
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.tron.generateSignRequest(tronSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert a Tron transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, { KeystoneAptosSDK } from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let aptosSignRequest = {
    requestId: "17467482-2654-4058-972D-F436EFAEB38E",
    signData: "B5E97DB07FA0BD0E5598AA3643A9BC6F6693BDDC1A9FEC9E674A461EAA00B1931248CD3D5E09500ACB7082497DEC1B2690384C535F3882ED5D84392370AD0455000000000000000002000000000000000000000000000000000000000000000000000000000000000104636F696E087472616E73666572010700000000000000000000000000000000000000000000000000000000000000010A6170746F735F636F696E094170746F73436F696E0002201248CD3D5E09500ACB7082497DEC1B2690384C535F3882ED5D84392370AD04550880969800000000000A000000000000009600000000000000ACF63C640000000002",
    signType: KeystoneAptosSDK.SignType.SingleSign,
    accounts: [{
        path: "m/44'/637'/0'/0'/0'",
        xfp: "F23F9FD2"
    }],
    origin: "Petra"
}

const Aptos = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.aptos.generateSignRequest(aptosSignRequest);

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

![](/_media/sign-tron-trc20.png ':size=200')

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
    let signature = try keystoneSDK.tron.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a Tron signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Tron.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.tron.parseSignature(decodedResult.ur!!)
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

export const TronScanner = () => {
  const keystoneSDK = new KeystoneSDK();

  const onSucceed = ({type, cbor}) => {
    const signature = keystoneSDK.tron.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
    console.log("signature: ", signature);
  }
  const onError = (errorMessage) => {
    console.log("error: ", errorMessage);
  }

  return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.TronSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature in hex string
)
```
