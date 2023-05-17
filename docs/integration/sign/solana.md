# Solana

## Display Unsigned Transaction

For passing an unsigned Solana transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the unsigned transaction data, in hex string
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
dataType: Enum // supported data type. Currently supports transaction and message
origin: String(Optional) // source of the request, wallet name etc
address: String(Optional) // the address for request this signing
```

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let solSignRequest = SolSignRequest(
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: "48656c6c6f2c204b657973746f6e652e",
    path: "m/44'/501'/0'/0'",
    xfp: "F23F9FD2",
    signType: .message,
    origin: "Solflare"
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.sol.generateSignRequest(solSignRequest: solSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert Solana transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Solana.swift)

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val solSignRequest = SolSignRequest(
    requestId = "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData = "48656c6c6f2c204b657973746f6e652e",
    path = "m/44'/501'/0'/0'",
    xfp = "F23F9FD2",
    signType = KeystoneSolanaSDK.SignType.Message,
    origin = "Solflare",
)

val keystoneSDK = KeystoneSDK()
val res = keystoneSDK.sol.generateSignRequest(solSignRequest)

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
import KeystoneSDK, {KeystoneSolanaSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const solSignRequest = {
    requestId: "6c3633c0-02c0-4313-9cd7-e25f4f296729", // uuid.v4()
    signData: "48656c6c6f2c204b657973746f6e652e",
    dataType: KeystoneSolanaSDK.DataType.Message,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2",
    chainId: 1,
    origin: "MetaMask"
}

export const Solana = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.sol.generateSignRequest(solSignRequest);

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

The QR Code the unsigned message generated above.

![](/_media/sign-sol-message.png ':size=300')


## Sign Request Example

### Transaction

<!-- tabs:start -->

#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {KeystoneSolanaSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";
import { Transaction, PublicKey, LAMPORTS_PER_SOL, SystemProgram } from "@solana/web3.js";

let transaction = new Transaction({
    recentBlockhash: "CBP1Vd5bL3LC7erH2EUykCo3sPKGMvG9ZCbRBwkXmbbr", // recent block hash
    feePayer: new PublicKey("DHwzop9H7oWEhyFV89TU7sC2U8LJmVLNstRjGa2tvkwg"),
});
transaction.add(
    SystemProgram.transfer({
        fromPubkey: new PublicKey("DHwzop9H7oWEhyFV89TU7sC2U8LJmVLNstRjGa2tvkwg"), // m/44'/501'/0'/0'
        toPubkey: new PublicKey("2q8vpggiroLnp65iDfm74RhLd1q9rpQrjdcJP27i5fhC"), // m/44'/501'/0'/1'
        lamports: 1 * LAMPORTS_PER_SOL,
    }),
);

const solSignRequest = {
    requestId: uuid.v4(),
    signData: transaction.serializeMessage(),
    dataType: KeystoneSolanaSDK.DataType.Transaction,
    path: "m/44'/501'/0'/0'",
    xfp: "F23F9FD2",
    origin: "Solflare"
}

export const Solana = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.sol.generateSignRequest(solSignRequest);

    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")}/>
}
```
<!-- tabs:end -->


### Message

<!-- tabs:start -->
#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {KeystoneSolanaSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const unsignedMessage = "68656c6c6f" // hex string of the message "hello"

const solSignRequest = {
    requestId: uuid.v4(),
    signData: unsignedMessage,
    dataType: KeystoneSolanaSDK.DataType.Message,
    path: "m/44'/501'/0'/0'",
    xfp: "F23F9FD2",
    chainId: 1,
    origin: "Solflare"
}

export const Solana = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.sol.generateSignRequest(solSignRequest);

    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")}/>
}
```
<!-- tabs:end -->


## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedQR = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    let signature = try keystoneSDK.sol.parseSignature(ur: decodedQR)
}
```
An example of continues scanning and parsing Solana signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Solana.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    val signature = keystoneSDK.sol.parseSignature(decodedQR)
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

const Solana = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signature = keystoneSDK.sol.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ",errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.SolSignature]} />
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
