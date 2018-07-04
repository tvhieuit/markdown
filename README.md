# NANJCOIN GUILDELINE FOR ANDROID

## Features

- [x] Tạo ví NANJ
- [x] Nhập ví NANJ thông qua Private Key / Key Store
- [x] Xuất Private Key/ Key Store từ ví NANJ
- [x] Gửi NANJ Coin
- [x] Lịch sử giao dịch
- [x] Quét địa chỉ ví từ QRCode
- [x] Quét địa chỉ ví từ NFC

## Requirements
- Tool: [Android studio 3](https://developer.android.com/studio/)
- Minimum sdk support: 18


## Usage
### Khởi tạo
- Tạo lớp `Application` và thêm vào manifest
-  Điền thông tin sau vào bên dưới hàm onCreate()
```
new NANJWalletManager.Builder()
      .setContext(getApplicationContext())
      .setNANJAppId("AppId")
      .setNANJSecret("SecretKey")
      .build();
```

### Tạo địa ví mới
Để tạo một địa chỉ mới ta gọi tới hàm:
```swift
NANJWalletManager.shared.createWallet(password:"Password")
```

Để nhận địa chỉ ví trả về khi thực hiện thành công ta cần lắng nghe `delegate` từ `NANJWalletManager`
```swift
NANJWalletManager.shared.delegate = self
```
Khi địa chỉ ví ETH được tạo thành công ví ETH sẽ được trả về thông qua hàm sau, server sẽ tiến hành tạo ví NANJ
```swift
func didCreatingWallet(wallet: NANJWallet?)
```
Ví NANJ được tạo thành công sẽ được trả về thông qua delegate với hàm
```swift
func didCreateWallet(wallet: NANJWallet?, error: Error?) {
```

### Nhập địa chỉ ví vào ứng dụng
- Sử dụng Private Key
```swift
NANJWalletManager.shared.importWallet(privateKey: "Private Key")
```
- Sử dụng Key store (Chúng ta cần mật khẩu của key store)
```swift
NANJWalletManager.shared.importWallet(keyStore "Key Store", password "Password")
```

Nếu ví được nhập thành công và đã có ví `NANJ` sẽ đươc trả về thông qua `delegate` với hàm
```swift
func didImportWallet(wallet: NANJWallet?, error: Error?)
```
Trả về `wallet` nếu ví được nhập thành công,
`error` nếu ví nhập có lỗi xảy ra.

Ngược lại một ví NANJ sẽ được tạo như trong trường hợp tạo ví ETH thành công rồi tạo ví NANJ thông qua server.


### Xuất địa chỉ ví ra Private Key hoặc Key store
- Sử dụng hàm sau để xuất ra Private Key

```swift
NANJWalletManager.shared.exportPrivateKey(wallet: "NANJWallet")
```
`Private key` được trả về thông qua delegate với function
```swift
func didExportPrivatekey(wallet: NANJWallet, privateKey: String?, error: Error?, error: nil)
```
Trả về `String` `privateKey` nếu xuất thành công.

Ngược lại trả về một `Error`.
- Sử dụng hàm sau để xuất ra Keystore
```swift
NANJWalletManager.shared.exportKeystore(wallet: "NANJWallet", password: "Password")
```
Tương tự như export private key, export keystore sẽ trả về kết quả thông qua `delegate` với function
```swift
func didExportKeystore(wallet: NANJWallet, keyStore: String?, error: Error?)
```

### Gửi NANJ Coin
Để thực hiện việc gửi NANJ Coin tới một địa chỉ chúng ta gọi function sau ở đối tượng `NANJWallet` muốn gửi tiền đi
```swift
self.currentWallet = NANJWalletManager.shared.getCurrentWallet()
self.currentWallet?.delegate = self
self.currentWallet?.sendNANJ(toAddress: "NANJ Address", amount: "Amount send")
```
Việc gửi thông tin gửi NANJCoin được xác nhận thành công qua `delegate`
```swift
func didSendNANJCompleted(transaction: NANJTransaction?)
```

Hoặc lỗi xảy ra khi nảy với function
```swift
func didSendNANJError(error: String?)
```

## Author

NANJCOIN, support@nanjcoin.com

## License

NANJ SDK is available under the MIT license. See the LICENSE file for more info.
