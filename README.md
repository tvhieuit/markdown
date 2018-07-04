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
```
NANJWalletManager.instance.createWallet(NANJCreateWalletListener callback)
```

Khi tạo lỗi: 
```
void onCreateWalletFailure()
```
Khi địa chỉ ví ETH được tạo thành công `private key` sẽ được trả về thông qua hàm, server sẽ tiến hành tạo ví NANJ
```
void onCreateProcess(String privateKey)
```
Lấy địa chỉ NANJ từ địa chỉ ETH
```
NANJWalletManager.instance.getNANJWallet(ETH address)
```

### Nhập địa chỉ ví vào ứng dụng
- Sử dụng Private Key
```
NANJWalletManager.instance.importWallet(String privateKey, NANJImportWalletListener callback)
```
- Sử dụng Key store (Chúng ta cần mật khẩu của key store)
```
NANJWalletManager.instance.importWallet(String password, String keystore, NANJImportWalletListener callback)
```

- Sử dụng Key store file (Chúng ta cần mật khẩu của key store)
```
NANJWalletManager.instance.importWallet(String password, File keystore, NANJImportWalletListener callback)
```

+ Nếu nhập ví lỗi 
```onImportWalletFailure()```

+ Nếu nhập ví thành công 
```onImportWalletSuccess()```



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
