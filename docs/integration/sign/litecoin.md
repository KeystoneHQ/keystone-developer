# Litecoin

## Display Unsigned Transaction

For passing an unsigned Litecoin transaction to the Keystone hardware wallet,
`KeystoneSDK` needs a LTC sign request, then covert it into a QR code generator.

Sign Request
```js
requestId: String // UUID for current request
signData: Object(LitecoinTransaction) // the Litecoin transaction
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: Optional(String) // source of the request, wallet name etc
```

LitecoinTransaction
```js
fee: Number // transaction fee
inputs: Array (
    hash: String // UTXO hash in hex string
    index: Number // UTXO index
    utxo: (
        publicKey: String // the public key which the UTXO locking on, in hex string
        value: Number // the amount of the UTXO
        script: Optional(String) // the locking script of UTXO
    )
    ownerKeyPath: String // the HD path of the public key
)
outputs: Array (
    address: String // the receive address
    value: Number // send amount
    isChange: Optional(Boolean) // is this receive address a change address
    changeAddressPath: Optional(String) // the change address HD path if given isChange as true
)
dustThreshold: Optional(Number) // the dust threshold for transaction
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let ltcTransaction = LitecoinTransaction (
    fee: 2250,
    inputs: [
        LitecoinTransaction.Input(
            hash: "a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663",
            index: 1,
            utxo: LitecoinTransaction.Input.Utxo(
                publicKey: "035684d200e10bc1a3e2bd7d59e58a07f2f19ef968725e18f1ed65e13396ab9466",
                value: 18519750
            ),
            ownerKeyPath: "m/49'/2'/0'/0/0"
        )
    ],
    outputs: [
        Output(address: "MUfnaSqZjggTrHA2raCJ9kxpP2hM6zezKw", value: 10000),
        Output(address: "MK9aTexgpbRuMPqGpMERcjJ8hLJbAS31Nx", value: 18507500, isChange: true, changeAddressPath: "M/49'/2'/0'/0/0")
    ]
)
let ltcSignRequest = KeystoneSignRequest<LitecoinTransaction>(
    requestId: "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData: ltcTransaction,
    xfp: "F23F9FD2"
)

let keystoneSDK = KeystoneSDK()
let qrCode = keystoneSDK.ltc.generateSignRequest(ltcSignRequest: ltcSignRequest)

// Return the content that should be shown in QR code
qrCode.nextPart()

// Check if a single QR code can contain all the transaction information
let isSingleQRCode = qrCode.isSinglePart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert LTC sign request into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Litecoin.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val inputs = ArrayList<Input>()
inputs.add(
    Input(
        "a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663",
        2,
        UTXO(
            "035684d200e10bc1a3e2bd7d59e58a07f2f19ef968725e18f1ed65e13396ab9466",
            18519750
        ),
        "m/49'/2'/0'/0/0"
    )
)
val outputs = ArrayList<Output>()
outputs.add(Output("MUfnaSqZjggTrHA2raCJ9kxpP2hM6zezKw", 10000))
outputs.add(Output("MK9aTexgpbRuMPqGpMERcjJ8hLJbAS31Nx", 18507500, true, "M/49'/2'/0'/0/0"))

val signData = LtcTx(inputs, outputs, 2250)
val signRequest = KeystoneSignRequest(
    "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData,
    "F23F9FD2"
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.ltc.generateSignRequest(signRequest)

// Return the content that should be shown in QR code
qrCode.nextPart()

// Check if a single QR code can contain all the transaction information
val isSingleQRCode = qrCode.isSinglePart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

An example of covert LTC sign request into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

const ltcTransaction = {
    inputs: [{
        hash: 'a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663',
        index: 1,
        utxo: {
            publicKey: '035684d200e10bc1a3e2bd7d59e58a07f2f19ef968725e18f1ed65e13396ab9466',
            value: 18519750n,
        },
        ownerKeyPath: "m/49'/2'/0'/0/0"
    }],
    outputs: [
        {address: 'MUfnaSqZjggTrHA2raCJ9kxpP2hM6zezKw', value: 10000n, isChange: false, changeAddressPath: ''},
        {address: 'MK9aTexgpbRuMPqGpMERcjJ8hLJbAS31Nx', value: 18507500n, isChange: true, changeAddressPath: "M/49'/2'/0'/0/0"}
    ],
    fee: "2250"
}

const ltcSignRequest = {
    requestId: "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData: ltcTransaction,
    xfp: "F23F9FD2"
}

const Litecoin = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.ltc.generateSignRequest(ltcSignRequest);

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

The QR code generated for the LTC sign request above.

![](/_media/sign-ltc-tx.png ':size=300')

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

The progress range in the `decodeQR` result `0 - 100`.

The sign result data structure in the QR code
```
SignResult (
    requestId: String // the requestId from sign request
    rawData: String // the signed transaction raw format in hex string
)
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedResult = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedResult.progress == 100 {
    let signResult = try keystoneSDK.ltc.parseSignResult(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a signed PSBT, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Litecoin.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signResult = keystoneSDK.ltc.parseSignResult(decodedResult.ur!!)
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

const LitecoinScanner = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signResult = keystoneSDK.ltc.parseSignResult(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signResult: ", signResult);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.KeystoneSignResult]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->
