# Getting Started

The goal of the Keystone hardware wallet is to provide users with a reliable and quality signer by protecting users' 
private keys so Keystone seeks to integrate with existing wallets and applications. Keystone has now provided the 
Web3 Mode which has worked with a lot of wallets like Metamask, Solflare, Petra and etc.


## Wallet Integration

In order to work with Keystone, a wallet can follow the following process:

1. Keystone provides the account information to the wallet to generate addresses, sync balances, etc via QR Codes. 
2. The wallet generates the unsigned data that can include transactions, typed data, and etc, and then sends it to Keystone to sign via QR Code. 
3. Keystone would then sign the data and provide a signature back to the wallet via QR Code. 
4. The wallet receives the signature, constructs the signed data (transaction), and performs the following activities like broadcasting the transaction, etc.

An example of how to use MetaMask mobile with Keystone Hardware Wallet

<iframe style='aspect-ratio: 16/9; width: 100%; height:100%; max-width: 800px; min-width: 200px' src="https://www.youtube.com/embed/ixRIoGfbmTI" title="MetaMask and Keystone Mobile Tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


## Prerequisites

### Keystone Firmware

You need Keystone Multi-coin Firmware M-5.0 or above for the integration. Find it from [here](https://keyst.one/firmware?locale=en)

### The Keystone SDK

The Keystone SDK is built to simplify the way how software wallets communicate with the Keystone hardware wallet.

Besides, QR code presenting and scanning ability is needed within the software wallet, since QR code is the only language 
Keystone hardware wallet speaks.

<!-- tabs:start -->

#### **<span class="swift">iOS(Swift)</span>**

We have created a **Swift package** to help developers to extract the account information and sign transaction with Keystone hardware wallet.

[keystone-sdk-ios](https://github.com/KeystoneHQ/keystone-sdk-ios/), check all the available versions [here](https://github.com/KeystoneHQ/keystone-sdk-ios/tags)


Simply add it to the dependencies in your Package.swift.

```swift
dependencies: [
    .package(url: "https://github.com/KeystoneHQ/keystone-sdk-ios/", from: "0.0.1")
]
```


Or in Xcode, click `File` -> `Add Packages...`,  
paste `https://github.com/KeystoneHQ/keystone-sdk-ios/` in search box,  
click `Add Package` to add `keystone-sdk-ios` into your project.

![](/_media/sdk-ios-install.png ':size=800')

You can start to play with the `KeystoneSDK` in your App after the package has been added.

```swift
import KeystoneSDK

let keystoneSDK = KeystoneSDK()
```

#### **<span class="kotlin">Android(Kotlin)</span>**

We have created an **Android library** with JitPack to help developers to extract the account information and sign transaction with Keystone hardware wallet.

[keystone-sdk-android](https://github.com/KeystoneHQ/keystone-sdk-android/), check all the available versions [here](https://jitpack.io/#KeystoneHQ/keystone-sdk-android)

1. Add JitPack as dependency repository in your project.
    add to `build.gradle` with

    ```groovy
    allprojects {
        repositories {
            maven { url "https://jitpack.io" }
        }
    }
    ```
    
    or add to `settings.gradle` with
    
    ```groovy
    dependencyResolutionManagement {
        repositories {
            maven { url 'https://www.jitpack.io' }
        }
    }
    ```


2. Add KeystoneSDK Android library to your project

    ```groovy
    dependencies {
        compile 'com.github.KeystoneHQ/keystone-sdk-android:{latest version}'
    }
    ```

You can start to play with the `KeystoneSDK` in your App after the library has been added.

```kotlin
import com.keystone.sdk.KeystoneSDK

val keystoneSDK = KeystoneSDK()
```

#### **<span class="typescript">Web(TypeScript)</span>**

We have created **NPM libraries** to help developers to extract the account information and sign transaction with Keystone hardware wallet.

- [@keystonehq/keystone-sdk](https://www.npmjs.com/package/@keystonehq/keystone-sdk), covert transaction to QR code content, and parse QR code to signature.
- [@keystonehq/animated-qr](https://www.npmjs.com/package/@keystonehq/animated-qr), animated QR code presenting and scanning.

1. Add library as dependency in your project.

```
yarn add @keystonehq/keystone-sdk @keystonehq/animated-qr
```

```
npm install --save @keystonehq/keystone-sdk @keystonehq/animated-qr
```

You can start to play with the `KeystoneSDK` in your App after the library has been added.

```typescript
import KeystoneSDK from "@keystonehq/keystone-sdk"

let keystoneSDK = new KeystoneSDK()
```

<!-- tabs:end -->

> [!TIP|labelVisibility:hidden|iconVisibility:hidden]
> We build an [Android demo app](https://github.com/KeystoneHQ/keystone-sdk-android-demo/) and [iOS demo app](https://github.com/KeystoneHQ/keystone-sdk-ios-demo/)
> using `KeystoneSDK` to parse accounts QR code, generate sign request QR code and parse signature QR code.
