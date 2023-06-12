# Hardware Call

> [!TIP|labelVisibility:hidden|iconVisibility:hidden]
> Most wallet do not need this step when getting account information from Keystone.

Hardware Call is the process that a watch-only wallet send requests to Keystone hardware wallet,
Keystone then execute the given command and present the result in QR code.
You can think it as the API of Keystone hardware wallet.

The Hardware Call supports at the moment
- Key Derivation Call


## Key Derivation Call(WIP)

> [!WARNING|labelVisibility:hidden|iconVisibility:hidden|style:flat]
> Not available in Keystone hardware wallet at the moment.

Keystone hardware wallet needs to provide the account information to the software wallet to generate addresses, sync balances and etc.
There are situations that software needs multiple public keys to support different address types or different chains.
Key Derivation Call receives paths of public keys and generates corresponding QR codes.

For passing an key derivation call to the Keystone hardware wallet,
`KeystoneSDK` needs the data underneath, then covert it into a QR codes.

```js
schemas: Array(
    path: String // The path of public key that the software wallet want to get from hardware wallet
    curve: Optional(Enum) // The crypto curve for generating the public key, `secp256k1` by default, currently supports `secp256k1` and `ed25519`
    algo: Optional(Enum) // The derivation algorithm, `slip10` by default, currently supports `slip10` and `bip32ed25519`
)
origin: Optional(String) // source of the request, wallet name etc
```

Acceptable Combination

| curve     | derivation algorithm |            |
| --------- |----------------------|------------|
| secp256k1 | slip10               | ✔️           |
| secp256k1 | bip32ed25519         | Not Supported |
| ed25519   | slip10               | ✔️         |
| ed25519   | bip32ed25519         | ✔️         |


After generating the Key Derivation Call QR code with `KeystoneSDK`, scans the QR code with Keystone hardware wallet to execute it.

### Example

An example of trying to get the Bitcoin BIP 44 standard extended public key `m/44'/0'/0'` with curve `secp256k1`
and one of the Solana public key `m/44'/501'/0'/0'/0'` with curve `ed25519`.


<!-- tabs:start -->

#### **<span class="typescript">Web(TypeScript)</span>**

```jsx
import KeystoneSDK, {Curve} from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

export const KeyDerivationCall = () => {
    const schemas = [
        { path: "m/44'/0'/0'"},
        { path: "m/44'/501'/0'/0'/0'", curve: Curve.ed25519 }
    ]
    const ur = KeystoneSDK.generateKeyDerivationCall({ schemas });
    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")}/>
}
```

<!-- tabs:end -->

The QR code generated for the Key Derivation Call with path `m/44'/0'/0'` with curve `secp256k1` and path `m/44'/501'/0'/0'/0'` with curve `ed25519` above.

![](/_media/key-derivation-call.png ':size=200')
