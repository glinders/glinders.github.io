+++
title = "openssl Snippets"
date = 2026-02-19T11:07:15+13:00
author = "Geert"
description = "Some openssl commands on Linux"
categories = ["Encryption"]
tags = ["openssl", "encryption", "sha-256", "aes-ecb", "aes-cbc", ]
draft = false
+++

# SHA-256
Calculate SHA-256 over a number of bytes:
```bash
# pass raw binary data to openssl
echo -n "<hex data>" | xxd -r -p | openssl dgst -sha256
```
Examples:
```bash
# calculate SHA-256 over 16 bytes
$ echo -n "0123456789abcdef0123456789abcdef" | xxd -r -p | openssl dgst -sha256
SHA2-256(stdin)= 223e0a160af9da0a03e6dd2c4719c56f5d66a633cbe84e78aaa9f3735865522a

# calculate SHA-256 over 32 bytes
$ echo -n "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef" | xxd -r -p | \
    openssl dgst -sha256
SHA2-256(stdin)= 4884fdaafea47c29fea7159d0daddd9c085d6200e1359e85bb81736af6b7c837
```

# AES-ECB
AES-ECB doesn't need an IV (Initialisation Vector).
```bash
# encrypt
echo -n "<hex data>" | xxd -r -p | openssl enc -aes-<key-size>-ecb -K <key> -nopad | xxd -p -c 256

# decrypt
echo -n "<hex data>" | xxd -r -p | openssl enc -aes-<key-size>-ecb -d -K <key> -nopad | xxd -p -c 256
```
Set `<key-size>` to 128, 192 or 256 for AES-128, AES-192 or AES-256.

Examples:
```bash
# encrypt using a 128 bit key
$ echo -n "feedc0dec0ffeeaa6051722104c23a55" | xxd -r -p | \
    openssl enc -aes-128-ecb -K 223e0a160af9da0a03e6dd2c4719c56f -nopad | xxd -p -c 256
231d6608b0cd97de9f9fc6e14cdf8e4c

# decrypt using a 128 bit key
$ echo -n "231d6608b0cd97de9f9fc6e14cdf8e4c" | xxd -r -p | \
    openssl enc -aes-128-ecb -d -K 223e0a160af9da0a03e6dd2c4719c56f -nopad | xxd -p -c 256
feedc0dec0ffeeaa6051722104c23a55

# encrypt using a 256 bit key
$ echo -n "feedc0dec0ffeeaa6051722104c23a55" | xxd -r -p | \
    openssl enc -aes-256-ecb -K 4884fdaafea47c29fea7159d0daddd9c085d6200e1359e85bb81736af6b7c837 \
    -nopad | xxd -p -c 256
0231a80d377013ce49113f5618cb75f6

# decrypt using a 256 bit key
$ echo -n "0231a80d377013ce49113f5618cb75f6" | xxd -r -p | \
    openssl enc -aes-256-ecb -d -K 4884fdaafea47c29fea7159d0daddd9c085d6200e1359e85bb81736af6b7c837 \
    -nopad | xxd -p -c 256
feedc0dec0ffeeaa6051722104c23a55
```

# AES-CBC
AES-CBC needs an IV (Initialisation Vector).
```bash
# encrypt
echo -n "<hex data>" | xxd -r -p | openssl enc -aes-<key-size>-cbc -iv <iv> -K <key> -nopad | xxd -p -c 256

# decrypt
echo -n "<hex data>" | xxd -r -p | openssl enc -aes-<key-size>-cbc -iv <iv> -d -K <key> -nopad | xxd -p -c 256
```
Set `<key-size>` to 128, 192 or 256 for AES-128, AES-192 or AES-256.

Examples:
```bash
# encrypt using a 128-bit key
$ echo -n "\
feedc0debabebeefc0ffeedeadbeeffaceb00ccafebabe0badf00d1234567890\
abcdef1337c0dea1b2c3d45a5a5a5adec0ded0acce55e5defaced00011223344" | xxd -r -p | \
    openssl enc -aes-128-cbc -iv 7a913cef125804bd6680a92fd34518ce \
    -K 223e0a160af9da0a03e6dd2c4719c56f -nopad | xxd -p -c 256
8254b87fd118f15dd7fb8483d6c7fcf9ba902f692ec6cb7d3bebf6c6c0bb8c19576bd0a2fbd9e01efaabe98a18394293df4f0291ee63a45fa5d2a4bb3ada68d4

# decrypt using a 128-bit key
$ echo -n "\
8254b87fd118f15dd7fb8483d6c7fcf9ba902f692ec6cb7d3bebf6c6c0bb8c19\
576bd0a2fbd9e01efaabe98a18394293df4f0291ee63a45fa5d2a4bb3ada68d4" | xxd -r -p | \
    openssl enc -aes-128-cbc -iv 7a913cef125804bd6680a92fd34518ce -d \
    -K 223e0a160af9da0a03e6dd2c4719c56f -nopad | xxd -p -c 256
feedc0debabebeefc0ffeedeadbeeffaceb00ccafebabe0badf00d1234567890abcdef1337c0dea1b2c3d45a5a5a5adec0ded0acce55e5defaced00011223344
```


