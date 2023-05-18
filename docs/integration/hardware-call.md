# Hardware Call

Hardware Call is the process that a watch-only wallet send requests to Keystone hardware wallet,
Keystone then execute the given command and present the result in QR code.
You can think it as the API of Keystone hardware wallet. 
Not every wallet needs this step when getting account information from Keystone.

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
paths: Array(String) // the paths of public keys that the software wallet want
curve: Enum(Optional) // the crypto curve for generating the public key, `secp256k1` as default, currently supports `secp256k1` and `ed25519`, 
algo: Enum(Optional) // the derivation algorithm, `slip10` as default, currently supports `slip10` and `bip32ed25519`
origin: String(Optional) // source of the request, wallet name etc
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

An example of trying to get the Ethereum BIP 44 standard extended public key `m/44'/60'/0'`
and Bitcoin BIP 84 standard extended public key `m/84'/0'/0'`.


<!-- tabs:start -->

#### **Web(TypeScript)**

```jsx
import KeystoneSDK from "@keystonehq/keystone-sdk";
import {AnimatedQRCode} from "@keystonehq/animated-qr";

export const KeyDerivationCall = () => {
    const paths = ["m/44'/60'/0'", "m/84'/0'/0'"];
    const ur = KeystoneSDK.generateKeyDerivationCall({ paths });
    return <AnimatedQRCode type={ur.type} cbor={ur.cbor.toString("hex")}/>
}
```

<!-- tabs:end -->

The QR code generated for the Key Derivation Call with paths `m/44'/60'/0'` and `m/44'/501'/0'` above.

![](/_media/key-derivation-call.png ':size=200')
