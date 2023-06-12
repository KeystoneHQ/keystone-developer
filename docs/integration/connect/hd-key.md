
# HD Key

Keystone uses [HDKey](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-007-hdkey.md) for encoding and transmitting HDKeys.

## Data Structure

<!-- tabs:start -->

#### **Web(Typescript)**

```js
chain: String // The symbol of the coin this key belongs to, e.g. 'BTC', 'ETH'
path: String // The derivation path of current key
publicKey: String // The public key in hex string
name: String // The name of hardware wallet
xfp: String // The master fingerprint
chainCode: String // The chain code
extendedPublicKey: Optional(String) //  The account extended public key
note: Optional(String) // The note, e.g. "account.standard", "account.ledger_live"
```

<!-- tabs:end -->

## Example

<!-- tabs:start -->

#### **Web(Typescript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

export const Account = () => {

  const onSucceed = ({type, cbor}) => {
    const account = KeystoneSDK.parseHDKey(new UR(Buffer.from(cbor, "hex"), type))
    console.log("account: ", account);
  }
  const onError = (errorMessage) => {
    console.log("error: ", errorMessage);
  }

  return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.CryptoHDKey]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The QR code which contains the account information of Ethereum for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-hdkey-eth.png ':size=250')

The account information in the QR code

<!-- tabs:start -->

#### **Web(Typescript)**

```json
{
    "chain": "ETH",
    "path": "m/44'/60'/0'",
    "publicKey": "02cc6d7834204653ff10e0047a2395343cc6df081e76c88d5eee83f346f0b21cb7",
    "name": "Keystone",
    "xfp": "f23f9fd2",
    "chainCode": "712a9187e5c60c573a5acce855445376e1b74c240e417fe8cb2a8fdfd78d2d9d",
    "extendedPublicKey": "xpub6CBZfsQuZgVnvTcScAAXSxtX5jdMHtX5LdRuygnTScMBbKyjsxznd8XMEqDntdY1jigmjunwRwHsQs3xusYQBVFbvLdN4YLzH8caLSSiAoV"
}
```
<!-- tabs:end -->
