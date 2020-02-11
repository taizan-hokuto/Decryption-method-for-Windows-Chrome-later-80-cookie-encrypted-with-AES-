chromiumのソース

https://github.com/chromium/chromium/blob/master/components/os_crypt/os_crypt_win.cc

AEADオブジェクト

https://github.com/chromium/chromium/blob/master/crypto/aead.cc

Google Git　のリポジトリ（baseフォルダ）

https://chromium.googlesource.com/chromium/src/+/refs/heads/master/base


### AESで暗号化されているパートの復号
https://github.com/chromium/chromium/blob/57ae3550aeedf85faf04fe92c5a6d183e77c5c86/components/os_crypt/os_crypt_win.cc#L161

暗号文の先頭が「0x01 00 00 00」→ DPAPIで暗号化

暗号文の先頭が「v10」 → AESで暗号化


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
「Galois/Counter Mode (GCM)は、ブロック暗号の暗号利用モードの一つであり、認証付き暗号の一つである。」

復号するには、original keyだけでなくNonceも必要。

## Nonceの位置
各暗号化cookieデータ の先頭からバージョン表記（'v10'）を除いた4バイト目以降。

## Nonceの長さ
12
```c
const size_t kNonceLength = 96 / 8
```


## Key

'''C:\Users\{username}\AppData\Local\Google\Chrome\User Data\Local State```

（jsonファイルになっている）

の中の

``'os_encrypt -> encrypted key'``

の先頭から29バイトを飛ばして30バイト目以降が
BASE64でencodeされたkeyデータ。


[cookie_store_util.cc](https://github.com/chromium/chromium/blob/master/components/cookie_config/cookie_store_util.cc)

[DecryptString](https://github.com/chromium/chromium/blob/75c6482a2ad46970621ba6bd9b828f115fabd284/components/os_crypt/os_crypt_win.cc#L161)

[GetEncryptionKeyInternal](https://github.com/chromium/chromium/blob/75c6482a2ad46970621ba6bd9b828f115fabd284/components/os_crypt/os_crypt_win.cc#L101)

[GetEncryptionKeyFactory](https://github.com/chromium/chromium/blob/75c6482a2ad46970621ba6bd9b828f115fabd284/components/os_crypt/os_crypt_win.cc#L47)

[wincrypt_shim.h](https://chromium.googlesource.com/chromium/src/+/refs/heads/master/base/win/wincrypt_shim.h)

[network_service.cc](https://github.com/chromium/chromium/blob/master/services/network/network_service.cc)

[system_network_context_manager.cc](https://github.com/chromium/chromium/blob/master/chrome/browser/net/system_network_context_manager.cc)


[kuhl_m_dpapi_chrome_alg_key_from_b64](https://github.com/gentilkiwi/mimikatz/compare/2.2.0-20200104...2.2.0-20200208#diff-a9d628d2ebef21afa6489a3aa90c5f04)

[content::GetNetwrkService()](https://github.com/chromium/chromium/search?p=2&q=GetNetworkService&unscoped_q=GetNetworkService)

[network_service_instance.h](https://github.com/chromium/chromium/blob/master/content/public/browser/network_service_instance.h)

