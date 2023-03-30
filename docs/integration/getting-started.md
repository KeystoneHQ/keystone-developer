# Getting Started

The goal of the Keystone hardware wallet is to provide users with a reliable and quality signer by protecting users' 
private keys so Keystone seeks to integrate with existing wallets and applications. Keystone has now provided the 
Web3 Mode which has worked with a lot of wallets like Metamask, Solflare, Petra and etc.


## Prerequisites

### Keystone Firmware
You need Keystone Multi-coin Firmware M-5.0 or above for the integration. Find it from [here](https://keyst.one/firmware?locale=en)


## Wallet Integration

In order to work with Keystone, a wallet can follow the following process:

1. Keystone provides the account information to the wallet to generate addresses, sync balances, etc via QR Codes. 
2. The wallet generates the unsigned data that can include transactions, typed data, and etc, and then sends it to Keystone to sign via QR Code. 
3. Keystone would then sign the data and provide a signature back to the wallet via QR Code. 
4. The wallet receives the signature, constructs the signed data (transaction), and performs the following activities like broadcasting the transaction, etc.


## Library

<!-- tabs:start -->

#### **iOS**

We have created a **Swift package** to help developers to extract the account information and sign transaction with Keystone hardware wallet.

[keystone-sdk-ios](https://github.com/KeystoneHQ/keystone-sdk-ios/)

In Xcode, click `File` -> `Add Packages...`,  
paste `https://github.com/KeystoneHQ/keystone-sdk-ios/` in search box,  
click `Add Package` to add `keystone-sdk-ios` into your project.


![img.png](/_media/sdk-ios-install.png ':size=70%')


You can start to init the `KeystoneSDK` in your App after the package is added.

```swift
import KeystoneSDK

let sdk = KeystoneSDK()
```

#### **Android**

We have created an **Android library** with JitPack to help developers to extract the account information and sign transaction with Keystone hardware wallet.

[keystone-sdk-android](https://github.com/KeystoneHQ/keystone-sdk-android/)

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

You can start to init the `KeystoneSDK` in your App after the library is added.

```kotlin
import com.keystone.sdk.KeystoneSDK

val sdk = KeystoneSDK()
```

<!-- tabs:end -->