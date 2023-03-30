# Bitcoin

## Display Unsigned Transaction




<!-- tabs:start -->

#### **Swift**

```swift
  import KeystoneSDK

  let sdk = KeystoneSDK()

```

#### **Kotlin**

```kotlin
  import com.keystone.sdk.KeystoneSDK

  val sdk = KeystoneSDK()
```

<!-- tabs:end -->

## Get Signature from Keystone

<!-- tabs:start -->

#### **Javascript**

```javascript
  var qr = sdk.generatePSBT(buf)
```

#### **Swift**

```swift

```

#### **Kotlin**

```kotlin
  val res = sdk.btc.generatePSBT(psbtBytes)
  val qrcode = res.nextPart()
```

<!-- tabs:end -->
