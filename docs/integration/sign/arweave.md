# Arweave

## Display Unsigned Transaction

For passing an unsigned Arweave transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data in hex string
signType: Enum // the data type to be signed. transaction, data item and message.
saltLen: Enum // the `rsa-pss` salt length, 0 or 32, zero for deterministic signature, 0 or 32
masterFingerprint: String // master fingerprint provided by Keystone when getting accounts
account: Optional(String) // the Arweave account of the signing type for verification purpose 
origin: Optional(String) // source of the request, wallet name etc
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let arweaveSignRequest = ArweaveSignRequest(
    masterFingerprint: "F23F9FD2",
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "7b22666f726d6174223a322c226964223a22222c226c6173745f7478223a22675448344631615059587639314a704b6b6e39495336684877515a3141597949654352793251694f654145547749454d4d5878786e466a30656b42466c713939222c226f776e6572223a22222c2274616773223a5b7b226e616d65223a2256486c775a51222c2276616c7565223a2256484a68626e4e6d5a5849227d2c7b226e616d65223a22513278705a573530222c2276616c7565223a2251584a44623235755a574e30227d2c7b226e616d65223a22513278705a5735304c565a6c636e4e70623234222c2276616c7565223a224d5334774c6a49227d5d2c22746172676574223a226b796977315934796c7279475652777454617473472d494e3965773838474d6c592d795f4c473346784741222c227175616e74697479223a2231303030303030303030222c2264617461223a22222c22646174615f73697a65223a2230222c22646174615f726f6f74223a22222c22726577617264223a2239313037353734333836222c227369676e6174757265223a22227d",
    saltLen: .zero,
    signType: .transaction,
    origin: "arweave wallet"
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.arweave.generateSignRequest(arweaveSignRequest: arweaveSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert an Arweave transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Arweave.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val arweaveSignRequest = ArweaveSignRequest(
    requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData = "7b22666f726d6174223a322c226964223a22222c226c6173745f7478223a22675448344631615059587639314a704b6b6e39495336684877515a3141597949654352793251694f654145547749454d4d5878786e466a30656b42466c713939222c226f776e6572223a22222c2274616773223a5b7b226e616d65223a2256486c775a51222c2276616c7565223a2256484a68626e4e6d5a5849227d2c7b226e616d65223a22513278705a573530222c2276616c7565223a2251584a44623235755a574e30227d2c7b226e616d65223a22513278705a5735304c565a6c636e4e70623234222c2276616c7565223a224d5334774c6a49227d5d2c22746172676574223a226b796977315934796c7279475652777454617473472d494e3965773838474d6c592d795f4c473346784741222c227175616e74697479223a2231303030303030303030222c2264617461223a22222c22646174615f73697a65223a2230222c22646174615f726f6f74223a22222c22726577617264223a2239313037353734333836222c227369676e6174757265223a22227d",
    signType = KeystoneArweaveSDK.SignType.Transaction,
    saltLen = KeystoneArweaveSDK.SaltLen.Zero,
    masterFingerprint = 'F23F9FD2',
    origin = "arweave wallet",
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.arweave.generateSignRequest(arweaveSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert an Arweave transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {KeystoneArweaveSDK}  from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let arweaveTransaction = {
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "7b22666f726d6174223a322c226964223a22222c226c6173745f7478223a22675448344631615059587639314a704b6b6e39495336684877515a3141597949654352793251694f654145547749454d4d5878786e466a30656b42466c713939222c226f776e6572223a22222c2274616773223a5b7b226e616d65223a2256486c775a51222c2276616c7565223a2256484a68626e4e6d5a5849227d2c7b226e616d65223a22513278705a573530222c2276616c7565223a2251584a44623235755a574e30227d2c7b226e616d65223a22513278705a5735304c565a6c636e4e70623234222c2276616c7565223a224d5334774c6a49227d5d2c22746172676574223a226b796977315934796c7279475652777454617473472d494e3965773838474d6c592d795f4c473346784741222c227175616e74697479223a2231303030303030303030222c2264617461223a22222c22646174615f73697a65223a2230222c22646174615f726f6f74223a22222c22726577617264223a2239313037353734333836222c227369676e6174757265223a22227d",
    signType: KeystoneArweaveSDK.SignType.Transaction,
    saltLen: KeystoneArweaveSDK.SaltLen.Zero,
    masterFingerprint: 'F23F9FD2',
    origin: 'arweave wallet'
}

const Arweave = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.arweave.generateSignRequest(arweaveTransaction);

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

![](/_media/sign-arweave-tx.gif ':size=200')

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
    let signature = try keystoneSDK.arweave.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing an Arweave signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Arweave.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.arweave.parseSignature(decodedResult.ur!!)
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

const ArweaveScanner = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({cbor, type}) => {
        const signature = keystoneSDK.arweave.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.ArweaveSignature]} />
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
