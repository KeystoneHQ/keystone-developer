# Sign Transaction & Message

![](../_media/sign.png)

1. The software wallet generate the unsigned transaction/message. For sending the unsigned data to Keystone, the data have to be encoded into the QR code.
2. Use Keystone the Scan the QRCode.
3. Sign Transaction on Keystone hardware wallet. After Keystone signs the request, the Keystone device will provide a signature via QR code.
4. Use software wallet extract the signature via scanning the signature showing on the Keystone.
5. The software wallet construct the signed transaction/message and broadcast to blockchain.
