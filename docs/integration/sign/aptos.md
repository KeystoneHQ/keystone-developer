# Aptos

## Display Unsigned Transaction

For passing an unsigned Aptos transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
signType: Enum // supported data type. Currently supports single, multi and message
accounts: Array (
    path: String // the HD path to tell which private key should be used to sign the data
    xfp: String // master fingerprint provided by Keystone when getting accounts
    key: Optional(String) // the public key for request this signing
)
origin: Optional(String) // source of the request, wallet name etc
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

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

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Aptos.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val accounts = ArrayList<AptosAccount>();
accounts.add(AptosAccount(path = "m/44'/637'/0'/0'/0'", xfp = "F23F9FD2"))
val signRequest = AptosSignRequest(
    requestId = "17467482-2654-4058-972D-F436EFAEB38E",
    signData = "B5E97DB07FA0BD0E5598AA3643A9BC6F6693BDDC1A9FEC9E674A461EAA00B1931248CD3D5E09500ACB7082497DEC1B2690384C535F3882ED5D84392370AD0455000000000000000002000000000000000000000000000000000000000000000000000000000000000104636F696E087472616E73666572010700000000000000000000000000000000000000000000000000000000000000010A6170746F735F636F696E094170746F73436F696E0002201248CD3D5E09500ACB7082497DEC1B2690384C535F3882ED5D84392370AD04550880969800000000000A000000000000009600000000000000ACF63C640000000002",
    signType = KeystoneAptosSDK.SignType.Single,
    accounts = accounts,
    origin = "Petra"
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.aptos.generateSignRequest(signRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

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

![](/_media/sign-aptos-message.png ':size=200')

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
    let signature = try keystoneSDK.aptos.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing an Aptos signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Aptos.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.aptos.parseSignature(decodedResult.ur!!)
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

const Aptos = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signature = keystoneSDK.aptos.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.AptosSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature in hex string
    authenticationPublicKey: String // indicate which signer signed the transaction
)
```
