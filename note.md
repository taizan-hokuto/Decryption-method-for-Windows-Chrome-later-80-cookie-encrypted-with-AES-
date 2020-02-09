chromiumのソース

https://github.com/chromium/chromium/blob/master/components/os_crypt/os_crypt_win.cc

AEADオブジェクト

https://github.com/chromium/chromium/blob/master/crypto/aead.cc

### AESで暗号化されているパートの復号
https://github.com/chromium/chromium/blob/57ae3550aeedf85faf04fe92c5a6d183e77c5c86/components/os_crypt/os_crypt_win.cc#L161

暗号文の先頭が「0x01」→ DPAPIで暗号化

暗号文の先頭が「V10」 → AESで暗号化


### AEAD
Authenticated Encryption with Associated Data)(認証付き暗号化)
AE モードの典型的な API は次のような関数を提供する:

- 暗号化
- - 入力: 平文、鍵、場合によりヘッダ — これは平文で、暗号化はされないが認証保護の対象である;
- - 出力: 暗号文、認証タグ (MAC)

-復号
 - - 入力: 暗号文、鍵、認証タグ、場合によりヘッダ;
 - - 出力: 平文、あるいは、認証タグが暗号文とヘッダに合致しない場合はエラー

## AES_256_GCM
Galois/Counter Mode (GCM)は、ブロック暗号の暗号利用モードの一つであり、認証付き暗号の一つである。

https://github.com/chromium/chromium/blob/master/crypto/aead.cc


## nonceの長さ
12
```c
const size_t kNonceLength = 96 / 8
```
## nonceの位置
cypher_txt の先頭からバージョン表記（V10）を除いて12バイト
