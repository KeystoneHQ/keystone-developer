# XRP Account

XRP account exports necessary account information for XRP wallets.

## Data Structure

<!-- tabs:start -->

#### **Web(Typescript)**

```typescript
interface XrpAccount {
  address: string
  key: string
}
```
<!-- tabs:end -->


## Example

<!-- tabs:start -->

#### **Web(Typescript)**

```jsx
import KeystoneSDK, {UR, URType} from "@keystonehq/keystone-sdk"
import {AnimatedQRScanner} from "@keystonehq/animated-qr"

const XrpAccount = () => {
    const onSucceed = ({type, cbor}) => {
        const account = KeystoneSDK.xrp.parseAccount(new UR(Buffer.from(cbor, "hex"), type))
        console.log("account: ", account);
    }
    const onError = (errorMessage) => {
        console.log("error: ", errorMessage);
    }

    return <AnimatedQRScanner handleScan={onSucceed} handleError={onError} urTypes={[URType.XrpAccount]} />
}
```

`AnimatedQRScanner` helps scan the QR code on Keystone hardware wallet and returns the data which can be parsed by `KeystoneSDK`.

<!-- tabs:end -->

The QR code which contains the arweave account information for the mnemonic underneath.

> twenty crawl clip injury guess bonus scare eager phone comfort slight invest

![](/_media/connect-xrp-account.png ':size=250')

> [!NOTE|labelVisibility:hidden|iconVisibility:hidden]
> Keystone hardware wallet uses [Fountain code](https://en.wikipedia.org/wiki/Fountain_code) to encode accounts when
> a single QR code is not able to contain all the account information. Multiple QR code content is needed for Keystone SDK
> to recover the account information provided by the Keystone hardware wallet.


The account information contains in the QR code

<!-- tabs:start -->

#### **Web(Typescript)**

```json
{
    "address": "rEHsDJtuyLguLQdww4UDUfmBHWSd8EUvKg",
    "pubkey": "0263e0f578081132fd9e12829c67b9e68185d7f7a8bb37b78f98e976c3d9d163e6"
}
```
<!-- tabs:end -->
