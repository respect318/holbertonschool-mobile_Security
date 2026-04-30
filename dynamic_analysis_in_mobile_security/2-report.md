# Task 2: Android Cryptography Challenge – Report

## Objective

Intercept and decrypt encrypted HTTP communication from an Android app to extract a hidden flag using cryptographic analysis.

## Tools Used

* Burp Suite / mitmproxy
* Wireshark
* APKTool / jadx

## Target

APK: `Apk_task2`
Protocols: HTTP/HTTPS with encrypted payloads
Crypto Methods: AES, RSA (suspected)

## Step-by-Step Process

1. **Environment Setup**

   * Enabled debugging on emulator/device.
   * Configured Burp Suite to proxy mobile traffic.
   * Installed Burp’s CA certificate on the Android emulator to capture HTTPS.

2. **Traffic Interception**

   * Captured encrypted responses via Burp.
   * Saved request/response payloads for further analysis.

3. **APK Decompilation**

   * Decompiled APK using JADX and APKTool.
   * Located encryption methods inside a `CryptoUtils` class.
   * Found usage of AES with hardcoded key and IV.

4. **Analyzing Cryptography**

   * Observed AES/CBC/PKCS5Padding encryption scheme.
   * Base64 decoding applied before decryption.
   * Extracted key and IV from the source code.

5. **Decryption**

   * Wrote a Python script to decode and decrypt the encrypted payload:

     ```python
     from Crypto.Cipher import AES
     import base64

     key = b"hardcodedkey1234"
     iv = b"hardcodediv12345"
     encrypted = base64.b64decode("<captured_encrypted_data>")

     cipher = AES.new(key, AES.MODE_CBC, iv)
     plaintext = cipher.decrypt(encrypted)
     print(plaintext.decode())
     ```

6. **Flag Retrieval**

   * Successfully decrypted payload and extracted the hidden flag.

## Output

> ⚑ Decrypted Flag: `Holberton{keystore_is_not_as_safe_as_u_think!}`

## Challenges Faced

* HTTPS interception required proper CA setup.
* Some string values were obfuscated but bypassed through dynamic logging.
