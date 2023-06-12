# Arweave Account

Arweave account exports necessary account information for Arweave wallets.

## Data Structure

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
struct ArweaveAccount {
    public var masterFingerprint: String
    public var keyData: String
    public var device: String
}
```

#### **Android(Kotlin)**

```kotlin
data class ArweaveAccount(
    val masterFingerprint: String,
    val keyData: String,
    val device: String,
)
```

#### **Web(Typescript)**

```typescript
interface ArweaveAccount {
  masterFingerprint: string
  keyData: string
  device: string
}
```
<!-- tabs:end -->


## Example

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
import KeystoneSDK

let sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns scan progress and data, progress range 0 - 100.
let result = try sdk.decodeQR(qrCode: qrCodeString)
if result.progress == 100 {
    let account = try sdk.arweave.parseAccount(ur: result.ur!)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/MultiAccountsView.swift).

#### **Android(Kotlin)**

```kotlin
import com.keystone.sdk.KeystoneSDK

val sdk = KeystoneSDK()
// `decodeQR` decodes given QR code content and returns scan progress and data, progress range 0 - 100.
val decodedResult = sdk.decodeQR(qrCodeString)
if (decodedQR.progress == 100) {
    val accounts = sdk.arweave.parseAccount(decodedResult.ur!!)
}
```

The accounts QR code parsing process within a demo app is available [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/ScannerFragment.kt).

#### **Web(Typescript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const MultiAccounts = () => {
    const onSucceed = ({type, cbor}) => {
        const account = KeystoneSDK.arweave.parseAccount(new UR(Buffer.from(cbor, "hex"), type))
        console.log("account: ", account);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.ArweaveCryptoAccount]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The QR code which contains the arweave account information for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-arweave-account.gif ':size=250')

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> Keystone hardware wallet uses [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) to encode accounts when
> a single QR code is not able to contain all the account information. Multiple QR code content is needed for Keystone SDK
> to recover the account information provided by the Keystone hardware wallet.


The account information contains in the QR code

<!-- tabs:start -->

#### **iOS(Swift)**

```swift
ArweaveAccount (
    masterFingerprint: "f23f9fd2",
    keyData: "c65925b0066affd7be100756e9ca662f52aa23faaa5b399cba83d81f871a1b7b589b36bdbc8e4a7205b74b6720cb1b9c1fc0309a4ee5150f7ad98d83c0dd12bbf1017bc1a5173925f9be08297c50ba6745c7a35c1264302e7b08d9ab48a5473f62ff0b952d13af63dfdad5ab581efc6c6c0bebfacdbfea20e6e36f3bf28c452b35bf1b3cacf6dcd80d2655b07d6ed6704a72a0825ac1885831510befbc2fbd394133357f669a3d8c0d1b7f2eafe0b5d9104d56d962133fe44c9217bf33fe11235ea4568e958e844ab2e70fe2d1b431d162fc3e2eae5cce55d85b827b0d1448ec6010bf048db8f2ede88838d652fa06bd40d61fa4a18c86587af4b419680fbea0351ad99920322813b70ed8f098acb4ef5ea8a52c73f2208c8deff1c140f056b7b584ba85b9f8cb2b9c655e69f575b65a663a1fa3d04585d9315d10c85d4bd4f234bbf989f60af60a894e4c62e9d509ad8c8a8bd16623c1385486ee869febcb2d53b03b152e24eaa6590abb2e053432f0963b62ba5b2d179f890c126e5503c4da72b4d28b48401f6296308a032c7d8d2a2716150b4869c60bf808d2bee2e59a4838f02f750e597c5d8073d3a5e19b998272b130b7386e258136f5eec9efbc7c66dbc294c88f47e64d74588a50056a19d8c7c6edf9c16ec1a3c5b7d337d1aafa30a99e930fa7883fcafdec9021a3982a011cb34c8e061d33f800dd6259cb6e637f"
    device: "Keystone",
)
```

#### **Android(Kotlin)**

```kotlin
ArweaveAccount (
    masterFingerprint: "f23f9fd2",
    keyData: "c65925b0066affd7be100756e9ca662f52aa23faaa5b399cba83d81f871a1b7b589b36bdbc8e4a7205b74b6720cb1b9c1fc0309a4ee5150f7ad98d83c0dd12bbf1017bc1a5173925f9be08297c50ba6745c7a35c1264302e7b08d9ab48a5473f62ff0b952d13af63dfdad5ab581efc6c6c0bebfacdbfea20e6e36f3bf28c452b35bf1b3cacf6dcd80d2655b07d6ed6704a72a0825ac1885831510befbc2fbd394133357f669a3d8c0d1b7f2eafe0b5d9104d56d962133fe44c9217bf33fe11235ea4568e958e844ab2e70fe2d1b431d162fc3e2eae5cce55d85b827b0d1448ec6010bf048db8f2ede88838d652fa06bd40d61fa4a18c86587af4b419680fbea0351ad99920322813b70ed8f098acb4ef5ea8a52c73f2208c8deff1c140f056b7b584ba85b9f8cb2b9c655e69f575b65a663a1fa3d04585d9315d10c85d4bd4f234bbf989f60af60a894e4c62e9d509ad8c8a8bd16623c1385486ee869febcb2d53b03b152e24eaa6590abb2e053432f0963b62ba5b2d179f890c126e5503c4da72b4d28b48401f6296308a032c7d8d2a2716150b4869c60bf808d2bee2e59a4838f02f750e597c5d8073d3a5e19b998272b130b7386e258136f5eec9efbc7c66dbc294c88f47e64d74588a50056a19d8c7c6edf9c16ec1a3c5b7d337d1aafa30a99e930fa7883fcafdec9021a3982a011cb34c8e061d33f800dd6259cb6e637f",
    device: "Keystone",
)
```

#### **Web(Typescript)**

```json
{
    "masterFingerprint": "f23f9fd2",
    "keyData": "c65925b0066affd7be100756e9ca662f52aa23faaa5b399cba83d81f871a1b7b589b36bdbc8e4a7205b74b6720cb1b9c1fc0309a4ee5150f7ad98d83c0dd12bbf1017bc1a5173925f9be08297c50ba6745c7a35c1264302e7b08d9ab48a5473f62ff0b952d13af63dfdad5ab581efc6c6c0bebfacdbfea20e6e36f3bf28c452b35bf1b3cacf6dcd80d2655b07d6ed6704a72a0825ac1885831510befbc2fbd394133357f669a3d8c0d1b7f2eafe0b5d9104d56d962133fe44c9217bf33fe11235ea4568e958e844ab2e70fe2d1b431d162fc3e2eae5cce55d85b827b0d1448ec6010bf048db8f2ede88838d652fa06bd40d61fa4a18c86587af4b419680fbea0351ad99920322813b70ed8f098acb4ef5ea8a52c73f2208c8deff1c140f056b7b584ba85b9f8cb2b9c655e69f575b65a663a1fa3d04585d9315d10c85d4bd4f234bbf989f60af60a894e4c62e9d509ad8c8a8bd16623c1385486ee869febcb2d53b03b152e24eaa6590abb2e053432f0963b62ba5b2d179f890c126e5503c4da72b4d28b48401f6296308a032c7d8d2a2716150b4869c60bf808d2bee2e59a4838f02f750e597c5d8073d3a5e19b998272b130b7386e258136f5eec9efbc7c66dbc294c88f47e64d74588a50056a19d8c7c6edf9c16ec1a3c5b7d337d1aafa30a99e930fa7883fcafdec9021a3982a011cb34c8e061d33f800dd6259cb6e637f",
    "device": "Keystone"
}
```
<!-- tabs:end -->
