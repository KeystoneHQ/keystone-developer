# XRP

## Display Unsigned Transaction

For passing an unsigned XRP transaction or message to the Keystone hardware wallet,
`KeystoneSDK` needs a XRP transaction, then covert it into a QR code generator.

<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK from "@keystonehq/keystone-sdk"
import {AnimatedQRCode} from "@keystonehq/animated-qr"

let xrpTransaction = {
    TransactionType: "Payment",
    Amount: "10000000",
    Destination: "rHSW257ioNLCsyGNjWqk1RetxZmWYjkAFy",
    Flags: 2147483648,
    Account: "rEHsDJtuyLguLQdww4UDUfmBHWSd8EUvKg",
    Fee: "12",
    Sequence: 79991857,
    LastLedgerSequence: 80032220,
    SigningPubKey: "0263e0f578081132fd9e12829c67b9e68185d7f7a8bb37b78f98e976c3d9d163e6"
}

const Xrp = () => {
    const keystoneSDK = new KeystoneSDK();
    const ur = keystoneSDK.xrp.generateSignRequest(xrpTransaction);

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

![](/_media/sign-xrp-tx.png ':size=200')

## Get Signature from Keystone

Scan the animated QR code with your application on Keystone hardware wallet after signing the data.

Keystone hardware wallet uses Fountain code to encode data when a single QR code is not able to contain all the information.
Multiple QR code content is needed for Keystone SDK to recover the information provided by the Keystone hardware wallet.


<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const XrpScanner = () => {
    const keystoneSDK = new KeystoneSDK();

    const onSucceed = ({cbor, type}) => {
        const signature = keystoneSDK.xrp.parseSignature(new UR(Buffer.from(cbor, "hex"), type))
        console.log("signature: ", signature);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.XrpAccount]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns signed data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The signature returns from the QR code is a hex string, you can use it as the `TxnSignature`.

