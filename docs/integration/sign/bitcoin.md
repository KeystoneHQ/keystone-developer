# Bitcoin

## Display Unsigned Transaction

For passing an unsigned Bitcoin transaction to the Keystone hardware wallet,
`KeystoneSDK` needs a PSBT in a hex format, then covert it into a QR code generator.

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

> **Note**  
> The Keystone SDK might generate an infinite number of QR codes when the unsigned transaction contains a big chunk of data,
> the software wallet will need to show the animated QR codes so that the Keystone hardware wallet can get all the
> transaction data. See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

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
An example of covert PSBT data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignBitcoinTxView.swift).

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

<!-- tabs:end -->

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400`.

### Example
An example of PSBT QR code

||address|value|path|
|--|--|--|--|
|input0|bc1q0nkhj74papxls8jtnhy2g6udkl62ht5345aeur|1|m/84'/0'/0'/0/0|
|output0|bc1qwvr3x4mc3jrpys0xaxguc8mexw4gw3zyt3h05c|0.1|m/84'/0'/0'/0/1|
|output1|bc1qmx85cfywqmj56z96lhpp8yf2e2qvpg620f2pa6|0.85|m/84'/0'/0'/1/0(change)|

PSBT hex
```shell
70736274ff0100710200000001a6e52d0cf7bec16c454dc590966906f2f711d2ffb720bf141b41fd0cd3146a220000000000ffffffff02809698000000000016001473071357788c861241e6e991cc1f7933aa87444440ff100500000000160014d98f4c248e06e54d08bafdc213912aca80c0a34a000000000001011f00e1f50500000000160014f6de6edbdef5f0e62777c14e6e322ecb27c7824b22060341d94247fabfc265035f0a51bcfaca3b65709a7876698769a336b4142faa4bad18f23f9fd254000080000000800000008000000000000000000000220203ab7173024786ba14179c33db3b7bdf630039c24089409637323b560a4b1d025618f23f9fd2540000800000008000000080010000000000000000
```

PSBT base64
```shell
cHNidP8BAHECAAAAAablLQz3vsFsRU3FkJZpBvL3EdL/tyC/FBtB/QzTFGoiAAAAAAD/////AoCWmAAAAAAAFgAUcwcTV3iMhhJB5umRzB95M6qHRERA/xAFAAAAABYAFNmPTCSOBuVNCLr9whORKsqAwKNKAAAAAAABAR8A4fUFAAAAABYAFPbebtve9fDmJ3fBTm4yLssnx4JLIgYDQdlCR/q/wmUDXwpRvPrKO2Vwmnh2aYdpoza0FC+qS60Y8j+f0lQAAIAAAACAAAAAgAAAAAAAAAAAAAAiAgOrcXMCR4a6FBecM9s7e99jADnCQIlAljcyO1YKSx0CVhjyP5/SVAAAgAAAAIAAAACAAQAAAAAAAAAA
```
PSBT QR Code  
![](/_media/sign-psbt-single.png ':size=350')

> Note: 
> The bigger the fragment length, the harder the Keystone scans it.

Example of multiple QR codes with the same PSBT above, given `maxFragmentLen` is `200`

![](/_media/sign-psbt-animated.gif ':size=300')

```swift
qrCode.nextPart() // ur:crypto-psbt/1-2/lpadaocfadhfcyjobbwdwyhdpyhkadgujojkidjyzmadaejsaoaeaeaeadctmhkbaarsasttnsnyptolotbbjlmyzoctsovofrhdceqzbkenwzlkaddegogtmuadaeaeaeaezmzmzmzmaovsaxaeaeaeaeaeaecmaebbmefwlrcmpfykcmlrbdiovwjehdselbdtetrofxlnfrrtaaaeaeaeaeaecmaebbfwvdemjeclenfxotidfpatbbiabwdwkttagrcpfmaeaeaeaeaeadadctfesoaaaeaeaeaeaecmaebbcfdeiesnchsprkspkgdkrslydpkbrehtcwahkklbcpamaomouolpimlykgvegdlbbdnnoxde
qrCode.nextPart() // ur:crypto-psbt/2-2/lpaoaocfadhfcyjobbwdwyhdpytdehuouyfmwpztltweenfrbbgwdepyeodndwbkuovtfrrlcswzfhnetddwaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaecpamaxmwhphgpmgrplloensfptongtuegskpnehpgswmbagofllaehdtgttozeenfmpyplcsjokbwejzghaeaelaaeaeaelaaeaeaelaadaeaeaeckaeaeaeaeaecpaoaopeghcwsbldvwetgetychsbdldasttnpfjlwkknttmykbdywdfspldnkondwpktvecsjokbwejzghaeaelaaeaeaelaaeaeaelaadaeaeaectaeaeaeaejkluhpkt
qrCode.nextPart() // ur:crypto-psbt/3-2/lpaxaocfadhfcyjobbwdwyhdpytdehuouyfmwpztltweenfrbbgwdepyeodndwbkuovtfrrlcswzfhnetddwaeaelaaeaeaelaaeaeaelaaeaeaeaeaeaeaeaecpamaxmwhphgpmgrplloensfptongtuegskpnehpgswmbagofllaehdtgttozeenfmpyplcsjokbwejzghaeaelaaeaeaelaaeaeaelaadaeaeaeckaeaeaeaeaecpaoaopeghcwsbldvwetgetychsbdldasttnpfjlwkknttmykbdywdfspldnkondwpktvecsjokbwejzghaeaelaaeaeaelaaeaeaelaadaeaeaectaeaeaeaemksrldcx
qrCode.nextPart() // ur:crypto-psbt/4-2/lpaaaocfadhfcyjobbwdwyhdpyhkadgujojkidjyzmadaejsaoaeaeaeadctmhkbaarsasttnsnyptolotbbjlmyzoctsovofrhdceqzbkenwzlkaddegogtmuadaeaeaeaezmzmzmzmaovsaxaeaeaeaeaeaecmaebbmefwlrcmpfykcmlrbdiovwjehdselbdtetrofxlnfrrtaaaeaeaeaeaecmaebbfwvdemjeclenfxotidfpatbbiabwdwkttagrcpfmaeaeaeaeaeadadctfesoaaaeaeaeaeaecmaebbcfdeiesnchsprkspkgdkrslydpkbrehtcwahkklbcpamaomouolpimlykgvegdlbzocheooy
```

## Get Signature from Keystone

Keystone will sign the given transaction and give back the signed PSBT via QR code.

> **Note**  
> Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
> Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()

let decodedQR = try keystoneSDK.decodeQR(qrCode: qrCodeString)
if decodedQR != nil {
    // signed PSBT as type Data
    let psbt = try keystoneSDK.btc.parsePSBT(cborHex: decodedQR.cbor)
}
```

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedQR = keystoneSDK.decodeQR(qrCodeString)
if (decodedQR != null) {
    // signed PSBT as type ByteArray
    val psbt = keystoneSDK.btc.parsePSBT(decodedQR.cbor)
}
```

<!-- tabs:end -->

The signed PSBT shown on Keystone hardware wallet.

![](/_media/sign-psbt-signature.png ':size=300')
