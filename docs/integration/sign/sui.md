# Sui

> [!WARNING|labelVisibility:hidden|iconVisibility:hidden|style:flat]
> Not available in Keystone hardware wallet at the moment.

## Display Unsigned Transaction

For passing an unsigned Sui transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
signType: Enum // supported data type. Currently supports single, multi and message
accounts: Array (
    path: String // the HD path to tell which private key should be used to sign the data
    xfp: String // master fingerprint provided by Keystone when getting accounts
    address: Optional(String) // the address for request this signing
)
origin: Optional(String) // source of the request, wallet name etc
```

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let suiSignRequest = SuiSignRequest(
    requestId: "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
    signData: "00000200201ff915a5e9e32fdbe0135535b6c69a00a9809aaf7f7c0275d3239ca79db20d6400081027000000000000020200010101000101020000010000ebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec3944093886901a2e3e42930675d9571a467eb5d4b22553c93ccb84e9097972e02c490b4e7a22ab73200000000000020176c4727433105da34209f04ac3f22e192a2573d7948cb2fabde7d13a7f4f149ebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec39440938869e803000000000000640000000000000000",
    signType: .single,
    accounts: [
        SuiSignRequest.Account(
            path: "m/44'/784'/0'/0'/0'",
            xfp: "f23f9fd2",
            address: "0xebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec39440938869"
        )
    ],
    origin: "Sui Wallet"
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.sui.generateSignRequest(suiSignRequest: suiSignRequest);

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Sui.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val accounts = ArrayList<SuiAccount>();
accounts.add(
    SuiAccount(
        "m/44'/784'/0'/0'/0'",
        "F23F9FD2",
        "0xebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec39440938869"
    )
)

val signRequest = SuiSignRequest(
    requestId = "17467482-2654-4058-972D-F436EFAEB38E",
    signData = "00000200201ff915a5e9e32fdbe0135535b6c69a00a9809aaf7f7c0275d3239ca79db20d6400081027000000000000020200010101000101020000010000ebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec3944093886901a2e3e42930675d9571a467eb5d4b22553c93ccb84e9097972e02c490b4e7a22ab73200000000000020176c4727433105da34209f04ac3f22e192a2573d7948cb2fabde7d13a7f4f149ebe623e33b7307f1350f8934beb3fb16baef0fc1b3f1b92868eec39440938869e803000000000000640000000000000000",
    signType = KeystoneSuiSDK.SignType.Single,
    accounts = accounts,
    origin = "Sui Wallet"
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.sui.generateSignRequest(signRequest)

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

#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {KeystoneSuiSDK} from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let suiSignRequest = {
  requestId: "7AFD5E09-9267-43FB-A02E-08C4A09417EC",
  signData: "000002002086ac6179ca6ad9a7b1ccb47202d06ae09a131e66309944922af9c73d3c203b66000810270000000000000202000101010001010200000100000e4d9313fb5b3f166bb6f2aea587edbe21fb1c094472ccd002f34b9d0633c71901d833a8eabc697a0b2e23740aca7be9b0b9e1560a39d2f390cf2534e94429f91ced0c00000000000020190ca0d64215ac63f50dbffa47563404182304e0c10ea30b5e4d671b7173a34c0e4d9313fb5b3f166bb6f2aea587edbe21fb1c094472ccd002f34b9d0633c719e803000000000000640000000000000000",
  signType: KeystoneSuiSDK.SignType.Single,
  accounts:[{
      path: "m/44'/784'/0'/0'/0'",
      xfp: '78230804',
      address: '0e4d9313fb5b3f166bb6f2aea587edbe21fb1c094472ccd002f34b9d0633c719'
  }],
  origin: "Sui Wallet"
}

export const Sui = () => {
  const keystoneSDK = new KeystoneSDK();
  const ur = keystoneSDK.sui.generateSignRequest(suiSignRequest);

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

![](/_media/sign-sui-single.png ':size=200')

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
    let signature = try keystoneSDK.sui.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a Sui signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Sui.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.sui.parseSignature(decodedResult.ur!!)
}
```
An example of continues scanning and parsing accounts data, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

export const SuiScanner = () => {
  const keystoneSDK = new KeystoneSDK();

  const onSucceed = ({type, cbor}) => {
    const signature = keystoneSDK.sui.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
    console.log("signature: ", signature);
  }
  const onError = (errorMessage) => {
    console.log("error: ", errorMessage);
  }

  return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.SuiSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature in hex string
    publicKey: String // indicate which signer signed the transaction
)
```
