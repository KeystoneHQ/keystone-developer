# Ethereum

## Display Unsigned Transaction

For passing an unsigned Ethereum transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

Currently, Keystone supports signing
- **Legacy Transaction**
- **EIP1559 Transaction**
- **Personal Message**
- **Typed Data**

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data or unsigned message, in hex string
dataType: Enum // supported data type. transaction, typed transaction, personal message and typed data.
chainId: Int // the EVM chain ID
path: String // the HD path to tell which private key should be used to sign the data
xfp: String // master fingerprint provided by Keystone when getting accounts
origin: Optional(String) // source of the request, wallet name etc
address: Optional(String) // the address for request this signing
```

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let ethSignRequest = EthSignRequest(
    requestId: "6c3633c0-02c0-4313-9cd7-e25f4f296729",
    signData: "48656c6c6f2c204b657973746f6e652e",
    dataType: .personalMessage,
    chainId: 1,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2",
    origin: "MetaMask"
)

let keystoneSDK = KeystoneSDK()
let qrCode = try keystoneSDK.eth.generateSignRequest(ethSignRequest: ethSignRequest)

// The QR code content which you can put in a QR code presenter.
let qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

An example of covert an unsigned message into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Ethereum.swift).

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400` bytes per image.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val ethSignRequest = EthSignRequest(
    requestId = "6c3633c0-02c0-4313-9cd7-e25f4f296729",
    signData = "48656c6c6f2c204b657973746f6e652e",
    dataType = KeystoneEthereumSDK.DataType.PersonalMessage,
    path = "m/44'/60'/0'/0/0",
    xfp = "F23F9FD2",
    origin = "MetaMask",
    chainId = 1,
)

val keystoneSDK = KeystoneSDK()
val qrCode = keystoneSDK.eth.generateSignRequest(ethSignRequest)

// The QR code content which you can put in a QR code presenter.
val qrContent = qrCode.nextPart()
```

All you need is to give the result of `nextPart` to a QR code presenter component,
Keystone can then scan and parse the transaction data.

An example of covert transaction data into QR code [here](https://github.com/KeystoneHQ/keystone-sdk-android-demo/blob/master/app/src/main/kotlin/com/keystone/sdk/demo/PlayerFragment.kt).

You can change the value of `KeystoneSDK.maxFragmentLen` to modify the capacity of a single QR code, the default length is `400` bytes per image.

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> The longer the fragment length, the more difficult it is for Keystone to scan.

> [!ATTENTION]
> The Keystone SDK will generate an infinite number of QR codes when the unsigned data is too big to put into a single QR code,
> the software wallet needs to show the animated QR codes so that the Keystone hardware wallet can get all the transaction data via continuous scanning.
> See [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) for more information about multiple QR codes.

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {KeystoneEthereumSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const ethSignRequest = {
    requestId: "6c3633c0-02c0-4313-9cd7-e25f4f296729", // uuid.v4()
    signData: "48656c6c6f2c204b657973746f6e652e",
    dataType: KeystoneEthereumSDK.DataType.personalMessage,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2",
    chainId: 1,
    origin: "MetaMask"
}

const Ethereum = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.eth.generateSignRequest(ethSignRequest);

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

![](/_media/sign-eth-message.png ':size=300')

## Sign Request Example

### Legacy Transaction

<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import { bufArrToArr } from '@ethereumjs/util'
import { RLP } from '@ethereumjs/rlp'
import { Transaction } from '@ethereumjs/tx';
import { Hardfork, Chain, Common } from '@ethereumjs/common';
import KeystoneSDK, {KeystoneEthereumSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const common =  new Common({ chain: Chain.Mainnet, hardfork: Hardfork.London });
const txParams = {
    to: "0x31bA53Ca350975007B27CF43AcB4D9Bc3db2641c",
    gasLimit: 200000,
    gasPrice: 120000000000,
    data: "0x",
    nonce: 1,
    value: 1200000000000000000n,  // 1.2 ETH
};
const tx = Transaction.fromTxData(txParams, { common: common, freeze: false });
const message = tx.getMessageToSign(false); // generate the unsigned transaction
const serializedMessage = Buffer.from(RLP.encode(bufArrToArr(message))).toString("hex") // use this for the HW wallet input

const ethSignRequest = {
    requestId: uuid.v4(),
    signData: serializedMessage,
    dataType: KeystoneEthereumSDK.DataType.transaction,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2", // master fingerprint
    chainId: 1,
    origin: "MetaMask"
}

const Ethereum = () => {
  const keystoneSDK = new KeystoneSDK();
  const ur = keystoneSDK.eth.generateSignRequest(ethSignRequest);
    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")} />
}
```
<!-- tabs:end -->


### EIP-1559 Typed Transaction

<!-- tabs:start -->
#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import { FeeMarketEIP1559Transaction } from '@ethereumjs/tx';
import { Hardfork, Chain, Common } from '@ethereumjs/common';
import KeystoneSDK, {KeystoneEthereumSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const common =  new Common({ chain: Chain.Mainnet, hardfork: Hardfork.London });

const txParams = {
  to: "0x31bA53Ca350975007B27CF43AcB4D9Bc3db2641c",
  gasLimit: 35552,
  maxPriorityFeePerGas: 75853,
  maxFeePerGas: 121212,
  data: "0x",
  nonce: 1,
  value: 1200000000000000000n,  // 1.2 ETH
};
const eip1559Tx = FeeMarketEIP1559Transaction.fromTxData(txParams, { common });
const unsignedMessage = Buffer.from(eip1559Tx.getMessageToSign(false)).toString("hex");

const ethSignRequest = {
    requestId: uuid.v4(),
    signData: unsignedMessage,
    dataType: KeystoneEthereumSDK.DataType.typedTransaction,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2", // master fingerprint
    chainId: 1,
    origin: "MetaMask"
}

const Ethereum = () => {
  const keystoneSDK = new KeystoneSDK();
  const ur = keystoneSDK.eth.generateSignRequest(ethSignRequest);
    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")} />
}
```
<!-- tabs:end -->

### Message

<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {KeystoneEthereumSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const unsignedMessage = "68656c6c6f" // hex string of the message "hello"

const ethSignRequest = {
    requestId: uuid.v4(),
    signData: unsignedMessage,
    dataType: KeystoneEthereumSDK.DataType.personalMessage,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2", // master fingerprint
    chainId: 1,
    origin: "MetaMask"
}

const Ethereum = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.eth.generateSignRequest(ethSignRequest);
    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")} />
}
```

<!-- tabs:end -->

### EIP-712 Typed Data

<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {KeystoneEthereumSDK} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

const data = {
    types: {
        EIP712Domain: [{name: 'name', type: 'string'}],
        Message: [{ name: 'data', type: 'string' }],
    },
    primaryType: 'Message',
    domain: {name: 'Keystone'},
    message: {data: 'Hello!'},
}
const unsignedMessage = Buffer.from(JSON.stringify(data), 'utf-8');

const ethSignRequest = {
    requestId: uuid.v4(),
    signData: unsignedMessage,
    dataType: KeystoneEthereumSDK.DataType.typedData,
    path: "m/44'/60'/0'/0/0",
    xfp: "F23F9FD2", // master fingerprint
    chainId: 1,
    origin: "MetaMask"
}

const Ethereum = () => {
  const keystoneSDK = new KeystoneSDK();
  const ur = keystoneSDK.eth.generateSignRequest(ethSignRequest);
  return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")} />
}
```

<!-- tabs:end -->


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
    let signature = try keystoneSDK.eth.parseSignature(ur: decodedResult.ur!)
}
```
An example of continues scanning and parsing an Ethereum signature, check [here](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/blob/master/keystone-sdk-ios-demo/SignTransaction/Ethereum.swift)

> [!ATTENTION]
> The signature might not always be able to encode in a single QR code,
> don't forget to handle the scenario in which Keystone shows it in animated QR codes.

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()

val decodedResult = keystoneSDK.decodeQR(qrCodeString)
if (decodedResult.progress == 100) {
    val signature = keystoneSDK.eth.parseSignature(decodedResult.ur!!)
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

const Ethereum = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signature = keystoneSDK.eth.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.EthSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    signature: String // the serialized signature (r,s,v) in hex string
)
```
