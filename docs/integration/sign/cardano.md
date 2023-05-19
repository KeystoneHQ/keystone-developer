# Cardano

> [!WARNING|labelVisibility:hidden|iconVisibility:hidden|style:flat]
> Not available in Keystone hardware wallet at the moment.

## Display Unsigned Transaction

For passing an unsigned Cardano transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR code generator.

```js
requestId: String // UUID for current request
signData: String // the serialized unsigned transaction data, in hex string
utxos: Array ( // the transactions inputs
    transactionHash: String
    index: Number
    amount: Number
    xfp: String // master fingerprint provided by Keystone when getting accounts 
    hdPath: String // the HD path to tell which private key should be used to sign the data
    address: String
)
certKeys: Array( // the stake keys need to use in this transaction
    keyHash: String // the stake key hash
    xfp: String // master fingerprint
    keyPath: String // the related private key path of this input
)
origin: String(Optional) // source of the request, wallet name etc
```

<!-- tabs:start -->

#### **Web(TypeScript)**

```jsx
import KeystoneSDK from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let cardanoSignRequest = {
    requestId: "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
    signData: Buffer.from("84a400828258204e3a6e7fdcb0d0efa17bf79c13aed2b4cb9baf37fb1aa2e39553d5bd720c5c99038258204e3a6e7fdcb0d0efa17bf79c13aed2b4cb9baf37fb1aa2e39553d5bd720c5c99040182a200581d6179df4c75f7616d7d1fd39cbc1a6ea6b40a0d7b89fea62fc0909b6c370119c350a200581d61c9b0c9761fd1dc0404abd55efc895026628b5035ac623c614fbad0310119c35002198ecb0300a0f5f6", "hex"),
    utxos: [
        {
            transactionHash: "4e3a6e7fdcb0d0efa17bf79c13aed2b4cb9baf37fb1aa2e39553d5bd720c5c99",
            index: 3,
            amount: 10000000,
            xfp: "73c5da0a",
            hdPath: "m/1852'/1815'/0'/0/0",
            address: "addr1qy8ac7qqy0vtulyl7wntmsxc6wex80gvcyjy33qffrhm7sh927ysx5sftuw0dlft05dz3c7revpf7jx0xnlcjz3g69mq4afdhv",
        },
        {
            transactionHash: "4e3a6e7fdcb0d0efa17bf79c13aed2b4cb9baf37fb1aa2e39553d5bd720c5c99",
            index: 4,
            amount: 18020000,
            xfp: "73c5da0a",
            hdPath: "m/1852'/1815'/0'/0/1",
            address: "addr1qyz85693g4fr8c55mfyxhae8j2u04pydxrgqr73vmwpx3azv4dgkyrgylj5yl2m0jlpdpeswyyzjs0vhwvnl6xg9f7ssrxkz90",
        },
    ],
    certKeys: [
        {
            keyHash: "e557890352095f1cf6fd2b7d1a28e3c3cb029f48cf34ff890a28d176",
            xfp: "73c5da0a",
            keyPath: "m/1852'/1815'/0'/2/0",
        },
    ],
    origin: "cardano-wallet"
}

export const Cardano = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.cardano.generateSignRequest(cardanoSignRequest);

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

![](/_media/sign-cardano-tx.png ':size=200')

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.

<!-- tabs:start -->

#### **Web(TypeScript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const Cardano = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({type, cbor}) => {
        const signature = keystoneSDK.cardano.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ",errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CardanoSignature]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature data structure in the QR code
```
Signature (
    requestId: String // the requestId from sign request
    witnessSet: String // the transaction witness set
)
```
