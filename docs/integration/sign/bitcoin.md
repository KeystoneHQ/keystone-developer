# Bitcoin

## Display Unsigned Transaction

For passing an unsigned Bitcoin transaction to the Keystone hardware wallet,
`KeystoneSDK` needs a PSBT in a hex format, then covert it into a QR code generator.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()
// Covert PSBT in Data to QR code generator
let qrCode = keystoneSDK.btc.generatePSBT(psbt: psbtData)

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

An example of covert PSBT data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Bitcoin.swift).

```swift
qrCode.nextPart() // ur:crypto-psbt/1-2/lpadaocfadcycyiychntfxhdlghkadchjojkidjyzmadaejsaoaeaeaeadolvwdpbnylrnsejzfegtskmhmtinamwzylbytdzmrlcxrsbbcwfpzcbntebbimcpaeaeaeaeaezmzmzmzmaolamtmkaeaeaeaeaecmaebbjkatbwhgkslklnbgfpvawlmesfctkkeopkltfyfyfzzmbeahaeaeaeaecmaebbtamygsdkmnamvwgtayrdzcsabwmedrsglartotgeaeaeaeaeaeadadctaevyykahaeaeaeaecmaebbkeweihwsuovy
qrCode.nextPart() // ur:crypto-psbt/2-2/lpaoaocfadcycyiychntfxhdlgkkknoyvsgtyackgrntspoxjelgrlwkpyplmecpamaxfptafwflzsrssaihaxhebkgyrfzssgfrihjonykskoinltinotenqzbbdlpkgrpmcswzfhnetdghaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaeaeaecpaoaxpyjsjkaofllnrdbbchnseouyfrkguriaaeessafzldfzmtemeyfrhfbkgrcaaohfcswzfhnetdghaeaelaaeaeaelaaeaeaelaadaeaeaeaeaeaeaeaefhbgecjt
qrCode.nextPart() // ur:crypto-psbt/3-2/lpaxaocfadcycyiychntfxhdlghkadchjojkidjyzmadaejsaoaeaeaeadolvwdpbnylrnsejzfegtskmhmtinamwzylbytdzmrlcxrsbbcwfpzcbntebbimcpaeaeaeaeaezmzmzmzmaolamtmkaeaeaeaeaecmaebbjkatbwhgkslklnbgfpvawlmesfctkkeopkltfyfyfzzmbeahaeaeaeaecmaebbtamygsdkmnamvwgtayrdzcsabwmedrsglartotgeaeaeaeaeaeadadctaevyykahaeaeaeaecmaebbkewestjkmefm
qrCode.nextPart() // ur:crypto-psbt/4-2/lpaaaocfadcycyiychntfxhdlgkkknoyvsgtyackgrntspoxjelgrlwkpyplmecpamaxfptafwflzsrssaihaxhebkgyrfzssgfrihjonykskoinltinotenqzbbdlpkgrpmcswzfhnetdghaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaeaeaecpaoaxpyjsjkaofllnrdbbchnseouyfrkguriaaeessafzldfzmtemeyfrhfbkgrcaaohfcswzfhnetdghaeaelaaeaeaelaaeaeaelaadaeaeaeaeaeaeaeaeaxstvwgl
```

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()
// Covert PSBT in ByteArray to QR code generator
val qrCode = keystoneSDK.btc.generatePSBT(psbt)

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

An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

```kotlin
qrCode.nextPart() // ur:crypto-psbt/1-2/lpadaocfadcycyiychntfxhdlghkadchjojkidjyzmadaejsaoaeaeaeadolvwdpbnylrnsejzfegtskmhmtinamwzylbytdzmrlcxrsbbcwfpzcbntebbimcpaeaeaeaeaezmzmzmzmaolamtmkaeaeaeaeaecmaebbjkatbwhgkslklnbgfpvawlmesfctkkeopkltfyfyfzzmbeahaeaeaeaecmaebbtamygsdkmnamvwgtayrdzcsabwmedrsglartotgeaeaeaeaeaeadadctaevyykahaeaeaeaecmaebbkeweihwsuovy
qrCode.nextPart() // ur:crypto-psbt/2-2/lpaoaocfadcycyiychntfxhdlgkkknoyvsgtyackgrntspoxjelgrlwkpyplmecpamaxfptafwflzsrssaihaxhebkgyrfzssgfrihjonykskoinltinotenqzbbdlpkgrpmcswzfhnetdghaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaeaeaecpaoaxpyjsjkaofllnrdbbchnseouyfrkguriaaeessafzldfzmtemeyfrhfbkgrcaaohfcswzfhnetdghaeaelaaeaeaelaaeaeaelaadaeaeaeaeaeaeaeaefhbgecjt
qrCode.nextPart() // ur:crypto-psbt/3-2/lpaxaocfadcycyiychntfxhdlghkadchjojkidjyzmadaejsaoaeaeaeadolvwdpbnylrnsejzfegtskmhmtinamwzylbytdzmrlcxrsbbcwfpzcbntebbimcpaeaeaeaeaezmzmzmzmaolamtmkaeaeaeaeaecmaebbjkatbwhgkslklnbgfpvawlmesfctkkeopkltfyfyfzzmbeahaeaeaeaecmaebbtamygsdkmnamvwgtayrdzcsabwmedrsglartotgeaeaeaeaeaeadadctaevyykahaeaeaeaecmaebbkewestjkmefm
qrCode.nextPart() // ur:crypto-psbt/4-2/lpaaaocfadcycyiychntfxhdlgkkknoyvsgtyackgrntspoxjelgrlwkpyplmecpamaxfptafwflzsrssaihaxhebkgyrfzssgfrihjonykskoinltinotenqzbbdlpkgrpmcswzfhnetdghaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaeaeaecpaoaxpyjsjkaofllnrdbbchnseouyfrkguriaaeessafzldfzmtemeyfrhfbkgrcaaohfcswzfhnetdghaeaelaaeaeaelaaeaeaelaadaeaeaeaeaeaeaeaeaxstvwgl
```

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.


#### **Web(TypeScript)**

```jsx
import KeystoneSDK from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const Bitcoin = () => {
    const psbtHex = "70736274ff0100710200000001a6e52d0cf7bec16c454dc590966906f2f711d2ffb720bf141b41fd0cd3146a220000000000ffffffff02809698000000000016001473071357788c861241e6e991cc1f7933aa87444440ff100500000000160014d98f4c248e06e54d08bafdc213912aca80c0a34a000000000001011f00e1f505000000001600147ced797aa1e84df81e4b9dc8a46b8db7f4abae9122060341d94247fabfc265035f0a51bcfaca3b65709a7876698769a336b4142faa4bad18f23f9fd254000080000000800000008000000000000000000000220203ab7173024786ba14179c33db3b7bdf630039c24089409637323b560a4b1d025618f23f9fd2540000800000008000000080010000000000000000";
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.btc.generatePSBT(Buffer.from(psbtHex, "hex"));

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

### PSBT Example

An example of PSBT QR code

||address|value|path|
|--|--|--|--|
|input0|bc1q0nkhj74papxls8jtnhy2g6udkl62ht5345aeur|1|m/84'/0'/0'/0/0|
|output0|bc1qwvr3x4mc3jrpys0xaxguc8mexw4gw3zyt3h05c|0.1|m/84'/0'/0'/0/1|
|output1|bc1qmx85cfywqmj56z96lhpp8yf2e2qvpg620f2pa6|0.85|m/84'/0'/0'/1/0(change)|

> **Use Mnemonic:** twenty crawl clip injury guess bonus scare eager phone comfort slight invest

PSBT hex
```
70736274ff0100710200000001a6e52d0cf7bec16c454dc590966906f2f711d2ffb720bf141b41fd0cd3146a220000000000ffffffff02809698000000000016001473071357788c861241e6e991cc1f7933aa87444440ff100500000000160014d98f4c248e06e54d08bafdc213912aca80c0a34a000000000001011f00e1f505000000001600147ced797aa1e84df81e4b9dc8a46b8db7f4abae9122060341d94247fabfc265035f0a51bcfaca3b65709a7876698769a336b4142faa4bad18f23f9fd254000080000000800000008000000000000000000000220203ab7173024786ba14179c33db3b7bdf630039c24089409637323b560a4b1d025618f23f9fd2540000800000008000000080010000000000000000
```

PSBT base64
```
cHNidP8BAHECAAAAAablLQz3vsFsRU3FkJZpBvL3EdL/tyC/FBtB/QzTFGoiAAAAAAD/////AoCWmAAAAAAAFgAUcwcTV3iMhhJB5umRzB95M6qHRERA/xAFAAAAABYAFNmPTCSOBuVNCLr9whORKsqAwKNKAAAAAAABAR8A4fUFAAAAABYAFHzteXqh6E34HkudyKRrjbf0q66RIgYDQdlCR/q/wmUDXwpRvPrKO2Vwmnh2aYdpoza0FC+qS60Y8j+f0lQAAIAAAACAAAAAgAAAAAAAAAAAAAAiAgOrcXMCR4a6FBecM9s7e99jADnCQIlAljcyO1YKSx0CVhjyP5/SVAAAgAAAAIAAAACAAQAAAAAAAAAA
```

PSBT QR Code

![](/_media/sign-psbt-single.png ':size=350')

Example of multiple QR codes with the same PSBT above, given `maxFragmentLen`/`capacity` is `200`

![](/_media/sign-psbt-animated.gif ':size=300')

## Get Signature from Keystone

Keystone will sign the given transaction and give back the signed PSBT via QR code. 

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
    // signed PSBT as type Data
    let psbt = try keystoneSDK.btc.parsePSBT(ur: decodedResult.ur!)
}
```

An example of continues scanning and parsing a signed PSBT, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Bitcoin.swift)

> [!ATTENTION]
> The signed PSBT might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    // signed PSBT as type ByteArray
    val psbt = keystoneSDK.btc.parsePSBT(decodedResult.ur!!)
}
```

An example of continues scanning and parsing accounts data, check [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt)

> [!ATTENTION]
> The signed PSBT might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.


#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const Bitcoin = () => {
    const onSucceed = ({type, cbor}) => {
        const psbt = keystoneSDK.btc.parsePSBT(new UR(Buffer.from(cbor, "hex"), type))
        console.log("psbt: ", psbt);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CryptoPSBT]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.


<!-- tabs:end -->

The signed PSBT shown on Keystone hardware wallet.

![](/_media/sign-psbt-result.png ':size=300')
