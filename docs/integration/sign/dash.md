# DigitalCash

> [!WARNING|labelVisibility:hidden|iconVisibility:hidden|style:flat]
> Not available in Keystone hardware wallet at the moment.

## Display Unsigned Transaction

For passing an unsigned DASH transaction to the Keystone hardware wallet,
`KeystoneSDK` needs DASH transaction data, then covert it into a QR code generator.

Sign Request
```js
requestId: String // UUID for current request
signData: Object(DashTransaction) // the DASH transaction
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: Optional(String) // source of the request, wallet name etc
```

DashTransaction
```js
fee: Number // transaction fee
inputs: Array (
    hash: String // UTXO hash in hex string
    index: Number // UTXO index
    value: Number // the amount of the UTXO
    pubKey: String // the public key which the UTXO locking on, in hex string
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

let dashTransaction = UtxoBaseTransaction (
    fee: 2250,
    inputs: [
        Input(
            hash: "a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663",
            index: 1,
            value: 18519750,
            pubkey: "03cf51a0e4f926e50177d3a662eb5cc38728828cec249ef42582e77e5503675314",
            ownerKeyPath: "m/44'/5'/0'/0/0"
        ),
    ],
    outputs: [
        Output(address: "XphpGezU3DUKHk87v2DoL4r7GhZUvCvvbm", value: 10000),
        Output(address: "XfmecwGwcPBR7pXTqrn26jTjNe8a4fvcSL", value: 18507500, isChange: true, changeAddressPath: "M/44'/5'/0'/0/0")
    ]
)
let dashSignRequest = KeystoneSignRequest<UtxoBaseTransaction>(
    requestId: "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData: dashTransaction,
    xfp: "F23F9FD2"
)

let keystoneSDK = KeystoneSDK()
let qrCode = keystoneSDK.dash.generateSignRequest(dashSignRequest: dashSignRequest)

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

An example of covert DASH sign request into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Dash.swift).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val inputs = ArrayList<CashInput>()
inputs.add(
    CashInput(
        "a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663",
        1,
        18519750,
        "03cf51a0e4f926e50177d3a662eb5cc38728828cec249ef42582e77e5503675314",
        "m/44'/5'/0'/0/0"
    )
)
val outputs = ArrayList<Output>()
outputs.add(Output("XphpGezU3DUKHk87v2DoL4r7GhZUvCvvbm", 10000))
outputs.add(Output("XfmecwGwcPBR7pXTqrn26jTjNe8a4fvcSL", 18507500, true, "M/44'/5'/0'/0/0"))

val signData = CashTx(inputs, outputs, 2250)
val signRequest = KeystoneSignRequest(
    "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData,
    "F23F9FD2"
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.dash.generateSignRequest(signRequest)

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

An example of covert DASH sign request into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
const dashTransaction = {
    inputs: [{
        hash: 'a59bcbaaae11ba5938434e2d4348243e5e392551156c4a3e88e7bdc0b2a8f663',
        index: 1,
        pubkey: '03cf51a0e4f926e50177d3a662eb5cc38728828cec249ef42582e77e5503675314',
        value: 18519750n,
        ownerKeyPath: "m/44'/5'/0'/0/0"
    }],
    outputs: [
        {address: 'XphpGezU3DUKHk87v2DoL4r7GhZUvCvvbm', value: 10000n, isChange: false, changeAddressPath: ''},
        {address: 'XfmecwGwcPBR7pXTqrn26jTjNe8a4fvcSL', value: 18507500n, isChange: true, changeAddressPath: "M/44'/5'/0'/0/0"}
    ],
    fee: "2250"
}

const dashSignRequest = {
    requestId: "cc946be2-8e4c-42be-a321-56a53a8cf516",
    signData: dashTransaction,
    xfp: "F23F9FD2"
}

const DigitalCash = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.dash.generateSignRequest(dashSignRequest);

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

The QR code generated for the DASH sign request above.

![](/_media/sign-dash-tx.png ':size=300')

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
    let signResult = try keystoneSDK.dash.parseSignResult(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing a signed transaction, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Dash.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signResult = keystoneSDK.dash.parseSignResult(decodedResult.ur!!)
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

export const DigitalCashScanner = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signResult = keystoneSDK.dash.parseSignResult(new UR(Buffer.from(cbor, "hex"), type))
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
