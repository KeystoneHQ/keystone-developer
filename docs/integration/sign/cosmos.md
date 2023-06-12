# Cosmos

## Display Unsigned Transaction

For passing an unsigned Cosmos transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

Currently, Keystone supports signing
- **Amino**
- **Direct**
- **Textual**
- **Message**

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
dataType: Enum // supported data type. Amino, Direct, Textual and Message
origin: Optional(String) // source of the request, wallet name etc
accounts: Array (
  path: String // the HD path to tell which private key should be used to sign the data
  xfp: String // master fingerprint provided by Keystone when getting accounts
  address: Optional(String) // the address for request this signing
)
```

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let cosmosSignRequest = CosmosSignRequest(
    requestId: "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
    signData: "7B226163636F756E745F6E756D626572223A22323930353536222C22636861696E5F6964223A226F736D6F2D746573742D34222C22666565223A7B22616D6F756E74223A5B7B22616D6F756E74223A2231303032222C2264656E6F6D223A22756F736D6F227D5D2C22676173223A22313030313936227D2C226D656D6F223A22222C226D736773223A5B7B2274797065223A22636F736D6F732D73646B2F4D736753656E64222C2276616C7565223A7B22616D6F756E74223A5B7B22616D6F756E74223A223132303030303030222C2264656E6F6D223A22756F736D6F227D5D2C2266726F6D5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D222C22746F5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D227D7D5D2C2273657175656E6365223A2230227D",
    dataType: .amino,
    accounts: [
        CosmosSignRequest.Account(
            path: "m/44'/118'/0'/0/0",
            xfp: "f23f9fd2",
            address: "4c2a59190413dff36aba8e6ac130c7a691cfb79f"
        )
    ]
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.cosmos.generateSignRequest(cosmosSignRequest: cosmosSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```
All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Cosmos.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val accounts = ArrayList<CosmosAccount>();
accounts.add(
    CosmosAccount(
        path = "m/44'/118'/0'/0/0",
        xfp = "f23f9fd2",
        address = "4c2a59190413dff36aba8e6ac130c7a691cfb79f"
    )
)
val cosmosSignRequest = CosmosSignRequest(
    requestId = "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
    signData = "7B226163636F756E745F6E756D626572223A22323930353536222C22636861696E5F6964223A226F736D6F2D746573742D34222C22666565223A7B22616D6F756E74223A5B7B22616D6F756E74223A2231303032222C2264656E6F6D223A22756F736D6F227D5D2C22676173223A22313030313936227D2C226D656D6F223A22222C226D736773223A5B7B2274797065223A22636F736D6F732D73646B2F4D736753656E64222C2276616C7565223A7B22616D6F756E74223A5B7B22616D6F756E74223A223132303030303030222C2264656E6F6D223A22756F736D6F227D5D2C2266726F6D5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D222C22746F5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D227D7D5D2C2273657175656E6365223A2230227D",
    accounts = accounts,
    dataType = KeystoneCosmosSDK.DataType.Amino
)
val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.cosmos.generateSignRequest(cosmosSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```
All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert Cosmos transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {KeystoneCosmosSDK} from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let cosmosSignRequest = {
    requestId: "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
    signData: "7B226163636F756E745F6E756D626572223A22323930353536222C22636861696E5F6964223A226F736D6F2D746573742D34222C22666565223A7B22616D6F756E74223A5B7B22616D6F756E74223A2231303032222C2264656E6F6D223A22756F736D6F227D5D2C22676173223A22313030313936227D2C226D656D6F223A22222C226D736773223A5B7B2274797065223A22636F736D6F732D73646B2F4D736753656E64222C2276616C7565223A7B22616D6F756E74223A5B7B22616D6F756E74223A223132303030303030222C2264656E6F6D223A22756F736D6F227D5D2C2266726F6D5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D222C22746F5F61646472657373223A226F736D6F31667334396A7867797A30306C78363436336534767A767838353667756C64756C6A7A6174366D227D7D5D2C2273657175656E6365223A2230227D",
    dataType: KeystoneCosmosSDK.DataType.amino,
    accounts: [{
        path: "m/44'/118'/0'/0/0",
        xfp: "f23f9fd2",
        address: "4c2a59190413dff36aba8e6ac130c7a691cfb79f"
    }]
}

const Cosmos = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.cosmos.generateSignRequest(cosmosSignRequest);

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

![](/_media/sign-cosmos-amino.gif ':size=300')

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

The progress range in the `decodeQR` result `0 - 100`.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedResult = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedResult.progress == 100 {
    let signature = try keystoneSDK.cosmos.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a Cosmos signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Cosmos.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.cosmos.parseSignature(decodedResult.ur!!)
}
```
An example of continues scanning and parsing QR code, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const Cosmos = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signature = keystoneSDK.cosmos.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CosmosSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature in hex string
    publicKey: String // the public key of private key which signed the data
)
```
