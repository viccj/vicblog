---
title: "Decrypt AES-GCM encoded content with JAVA Cipher using PHP OpenSSL"
date: 2023-07-05T15:00:41+08:00
tags:
- php
- java
- cipher
---


{{< toc >}}
最近工作遇到一個專案，是要把透過Java使用AES-GCM加密過的資料解密，因為對方也沒有講清楚（或是不熟）導致解密有點不順利，不過後來還是成功解出來了，因此紀錄一下。



In a recent project, I had to decrypt data encrypted using AES-GCM in Java. Despite some initial challenges due to unclear instructions or lack of familiarity, I eventually succeeded in decrypting the data. Here's a brief summary of the process.



## Conclusion



Java透過AES-GCM加密後的結果會包含兩部分：加密後字串+Tag，要解密時把字串拆開拿到Tag，並且只對加密後字串解密。

when encrypting data using AES-GCM in Java, the resulting ciphertext will consist of two parts: the **encrypted string** and the **tag**. When decrypting, it is important to separate the ciphertext and retrieve the tag. The decryption process should only be applied to the encrypted string part.



## The Process

Given data:

- Cipher: AES-GCM
- Key: i_am_key (to be hashed using SHA-256)
- IV: 6732ebbdb62d93067ad6aad8 (hex)
- encryptedText: e8675abcd27f9b2b734504cb8adfbd88d8592c25960e8e7d4df060
- plainText: 'hello world'



一開始只有這些資訊，因此我直接先試試看，因為有特地提到 key 要 `sha256 hash`，因此我判斷是用AES-256-GCM算法

With the initial information provided, I proceeded with the decryption attempt. Since it was specifically mentioned that the key needed to be hashed using SHA-256, I determined that the AES-256-GCM algorithm was being used.



```php
    public function testDecrypt()
    {
        $plainText = 'hello world';
        $cipher = 'AES-256-GCM';
        $key = 'i_am_key';
        $key = hash('sha256', $key, true);

        $iv = '6732ebbdb62d93067ad6aad8';
        $iv = hex2bin($iv);
        $encryptedText = 'e8675abcd27f9b2b734504cb8adfbd88d8592c25960e8e7d4df060';
   
        $encryptedBindToken = hex2bin(substr($encryptedText, 0, -$tagLength));

        $tag = hex2bin(substr($encryptedText, -$tagLength));


        $decrypted = openssl_decrypt(bin2hex($encryptedText), $cipher, $key, OPENSSL_RAW_DATA , $iv);
        $this->assertEquals($decrypted, $plainText);

    }
```

Get `false` directly, unexpectedly.

```bash
PHPUnit 9.6.9 by Sebastian Bergmann and contributors.

Time: 00:00.060, Memory: 6.00 MB

There was 1 failure:

1) xxxTest::testDecrypt
   Failed asserting that 'hello world' matches expected false.

FAILURES!
```



因此便去查一下資料，發現透過AES-GCM加密後會有一個 Tag 值，這個值也是解密會用到的

Therefore, I proceeded to look up the information and found that when using AES-GCM encryption, there is a tag value generated, which is also used during the decryption process.



[The GCM document from NIST (National Institute of Standards and Technology)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf)

> The following two bit strings comprise the output data of the authenticated encryption function:
>
> • A ciphertext, denoted C, whose bit length is the same as that of the plaintext.
>
> • An authentication tag, or tag, for short, denoted T.

下面這個[RFC](https://www.rfc-editor.org/rfc/rfc5084)也有提到會有一個 "Tag"

The [RFC](https://www.rfc-editor.org/rfc/rfc5084), also mentions the presence of a "Tag"



> AES-GCM has four inputs: an AES key, an initialization vector (IV), a plaintext content, and optional additional authenticated data (AAD).
>
> AES-GCM generates two outputs: a ciphertext and message authentication code (also called an authentication tag).



但是我並沒有這個 "Tag"，客戶也沒提供，因此後來又去查一下資料，看看有沒有辦法透過已經有的資料推出Tag，後來被我查到了，Tag就在加密後字串的最後面

However, I did not have this "Tag" and the client did not provide it either. So I further researched and tried to find a way to derive the Tag using the available data. Eventually, I found it! The Tag is located at the end of the encrypted string.

> In Java the tag is *unfortunately* added at the end of the ciphertext. - [StackOverFlow](https://stackoverflow.com/questions/23864440/aes-gcm-implementation-with-authentication-tag-in-java)



所以客戶提供的字串會有一段是tag，但目前還是不知道長度是多少，本來想直接問一下他們tag長度設定多少，但還是算了，先試試看預設長度 128bits。

So the string provided contains a section that represents the tag. However, the length of the tag is still unknown. Initially, I thought about directly asking for the tag length, but I decided to try it using the default length of 128 bits.



因為提供的hex的加密字串，所以直接取的話要算32個字元，如果轉成binary，只要取16個字元就好，這邊就直接抓最後32字元

Since the provided encrypted string is in hexadecimal format, if we consider each hex digit as one character, it would require 32 characters. However, if we convert it to binary format, we only need 16 characters. Therefore, I will directly extract the last 32 characters as the tag.



```php
    public function testDecrypt()
    {
        $plainText = 'hello world';
        $cipher = 'AES-256-GCM';
        $key = 'i_am_key';
        $key = hash('sha256', $key, true);

        $iv = '6732ebbdb62d93067ad6aad8';
        $iv = hex2bin($iv);
        $encryptedText = 'e8675abcd27f9b2b734504cb8adfbd88d8592c25960e8e7d4df060';
        $tagLength = 32;
        $tag = hex2bin(substr($encryptedText, -$tagLength));
        $decrypted = openssl_decrypt(bin2hex($encryptedText), $cipher, $key, OPENSSL_RAW_DATA , $iv, $tag);
        $this->assertEquals($decrypted, $plainText);
    }
```

```bash
PHPUnit 9.6.9 by Sebastian Bergmann and contributors.

Time: 00:00.060, Memory: 6.00 MB

There was 1 failure:

1) xxxTest::testDecrypt
   Failed asserting that 'hello world' matches expected false.

FAILURES!
```



結果還是 `false`，不知道是哪邊弄錯了，難道是tag長度弄錯了?!

The result still turned out to be "false," and I'm not sure where the mistake lies. Could it be that the tag length was set incorrectly?!



但後來想到既然沒提供 Tag 的話，代表他們八成也是用預設長度（後來證明沒錯，測了幾個長度的 tag 也是失敗）

However, later on, I realized that if they didn't provide the tag, it probably means they also used the default length (which was later confirmed to be true as testing different tag lengths also failed).



最後真的沒辦法了，自己用 key 跟 iv 加密一次看一下結果如何

In the end, when all else failed, I decided to encrypt it myself using the key and IV and see what the result would be.



```php
    public function testEncrypt()
    {
        $plainText = 'hello world';
        $cipher = 'AES-256-GCM';
        $key = 'i_am_key';
        $key = hash('sha256', $key, true);

        $iv = '6732ebbdb62d93067ad6aad8';
        $iv = hex2bin($iv);
        $encryptedText = 'e8675abcd27f9b2b734504cb8adfbd88d8592c25960e8e7d4df060';
        $tagLength = 32;
        $encryptedBindToken = hex2bin(substr($encryptedText, 0, -$tagLength));

        $encrypted = openssl_encrypt($plainText, $cipher, $key, OPENSSL_RAW_DATA , $iv, $tag);
        echo '<pre>';var_dump(bin2hex($encrypted));echo '</pre>';die();

    }
```

```bash
string(22) "e8675abcd27f9b2b734504"
```



發現咦這不是只有加密字串的前面嗎？

Oh, I just realized that this is not the entire encrypted string, but only the beginning part, right?



**e8675abcd27f9b2b734504**cb8adfbd88d8592c25960e8e7d4df060



所以真正的加密字串只有一段，後面是tag，解密的時候只要解密tag以外的那一段就好了...

So the actual encrypted string consists of a portion followed by the tag. During decryption, we only need to decrypt the part excluding the tag...



立馬重新再解密一次

Let me proceed with decrypting it again immediately.

```php
public function testDecrypt()
{
    $plainText = 'hello world';
    $cipher = 'AES-256-GCM';
    $key = 'i_am_key';
    $key = hash('sha256', $key, true);

    $iv = '6732ebbdb62d93067ad6aad8';
    $iv = hex2bin($iv);
    $encryptedTextAndTag = 'e8675abcd27f9b2b734504cb8adfbd88d8592c25960e8e7d4df060';
    $tagLength = 32;
    $encryptedTextOnly = hex2bin(substr($encryptedTextAndTag, 0, -$tagLength));

    $tag = hex2bin(substr($encryptedTextAndTag, -$tagLength));

    $decrypted = openssl_decrypt($encryptedTextOnly, $cipher, $key, OPENSSL_RAW_DATA , $iv, $tag);
    $this->assertEquals($decrypted, $plainText);



}
```

```bash
PHPUnit 9.6.9 by Sebastian Bergmann and contributors.
........                                                            8 / 8 (100%)

Time: 00:00.048, Memory: 6.00 MB

OK
```



Bingo!



## 參考資料

---

- [叉叉哥的BLOG](https://xxgblog.com/2021/08/03/java-aes/)

