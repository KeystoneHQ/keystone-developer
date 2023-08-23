# Utils

## CryptoPath

`CryptoPath` is here to help parse the HD path string to an Object which easier to use.
The field naming follows the [BIP44 convention](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki#path-levels), in CamelCase.

> m / purpose' / coin_type' / account' / change / address_index

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

```swift
import KeystoneSDK

let hdPath = "m/44'/60'/0'/10/1"
let path = CryptoPath.parseHDPath(hdPath: hdPath)

path.purpose?.index // 44
path.purpose?.hardened // true

path.coinType?.index // 60
path.account?.index // 0
path.change?.index // 10
path.addressIndex?.index // 1
```

Data Struct

```swift
struct PathItem {
    let index: UInt32
    let hardened: Bool
}

struct HDPath {
    let purpose: PathItem?
    let coinType: PathItem?
    let account: PathItem?
    let change: PathItem?
    let addressIndex: PathItem?
}
```

#### **<span class="kotlin">Android(Kotlin)</span>**

```kotlin
import com.keystone.utils.CryptoPath

val hdPath = "m/44'/60'/0'/10/1"
val path = CryptoPath().parseHDPath(hdPath)

path.purpose?.index // 44
path.purpose?.hardened // true

path.coinType?.index // 60
path.account?.index // 0
path.change?.index // 10
path.addressIndex?.index // 1
```

Data Struct

```kotlin
data class PathItem(
    val index: Int,
    val hardened: Boolean,
)

data class HDPath(
    val purpose: PathItem?,
    val coinType: PathItem?,
    val account: PathItem?,
    val change: PathItem?,
    val addressIndex: PathItem?,
)
```

<!-- tabs:end -->
