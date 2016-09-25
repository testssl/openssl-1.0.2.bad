OpenSSL
================

Build status for target linux-x86_64: [![Build Status](https://travis-ci.org/PeterMosmans/openssl.svg?branch=1.0.2-chacha)](https://travis-ci.org/PeterMosmans/openssl)

This is a fork of the official OpenSSL github repository at https://github.com/openssl/openssl.git

You're looking at the 1.0.2-chacha branch, which aligns with the OpenSSL_1_0_2-stable branch. The source from the official branch is merged on a regular base.

The main reason of the fork is to include ChaCha20, Poly1305, other (experimental/insecure) ciphers, and to add some extra features to s_client. It should compile 'as least as good' as the official OpenSSL_1_0_2-stable branch.

#### Security notices
Please note that some security restrictions have been removed on purpose: In contrast of the official fork, this version of openssl for instance [does not restrict the size of DH parameters](https://github.com/PeterMosmans/openssl/commit/1fb62ccc6360a4c29fc24fbc0ec82508356752fc). It also enables a lot of extra ciphers deemed *insecure*, so please be aware to explicity enable only those ciphers that you trust, and disable the rest.

#### Latest news
This branch is up to date with the OpenSSL 1.0.2j-dev version, and contains the fixes for CVE-2016-6304 from 09-09-2016 (and all earlier published CVE fixes).

#### Goals
The main goals of this fork are

1. add as much ciphers and (test) functionality as possible
2. to keep the source as aligned to the original as possible
3. keep the patches transparent (easily applicable to the original source)
4. keep the patches maintainable
5. write as little custom/new code as possible

#### More information
See [https://www.onwebsecurity.com/openssl/the-work-flow-of-the-full-featured-openssl-fork-chacha20poly1305](https://www.onwebsecurity.com/openssl/the-work-flow-of-the-full-featured-openssl-fork-chacha20poly1305) for detailed differences between the official openssl source and this fork, and on the workflow of keeping everything as up-to-date as possible.

Please see [https://www.onwebsecurity.com/openssl/replacing-chacha20poly1305-a-new-owner](https://www.onwebsecurity.com/openssl/replacing-chacha20poly1305-a-new-owner) for information about the future of the current ChaCha20 / Poly1305 code.

#### Additions
##### Ciphers
* Added ChaCha20 and Poly1305 ciphers (backported from the upstream 1.0.2-aead branch)
+ [Re-enabled elliptic curves < 256 bit](https://github.com/PeterMosmans/openssl/commit/f340dabc859192f9805919793834094b14e55a9b)
* [Added TLS-RSA-PSK ciphers](https://github.com/PeterMosmans/openssl/commit/ba47950a02a380413f3e5dbf8d94a89eb9e2fb42)
* [Added SHA256 CAMELLIA ciphers (cherry-picked from the upstream master branch)](https://github.com/PeterMosmans/openssl/commit/535e141f0e9df912232a6bd2ece72f30945962a1)
* [Added HMAC based CAMELLIA ciphers](https://github.com/PeterMosmans/openssl/commit/8efbb71e40b99e86741aafd6a3c95b941a26e5ce)
* [Enabled experimental features](https://github.com/PeterMosmans/openssl/commit/8c722ce5fb005a1886e2d76e788cc3441592490e)
* [Enabled even more ciphers](https://github.com/PeterMosmans/openssl/commit/c77a5fc708c9e88bce2c0c742f419ac908cd44d)
* [Removed the DH parameters restriction](https://github.com/PeterMosmans/openssl/commit/1fb62ccc6360a4c29fc24fbc0ec82508356752fc)
* [Re-enabled certain export ciphers](https://github.com/PeterMosmans/openssl/commit/8ae2e1d49308e0b1ff2e91beca1ad04e6e163a9a)

##### s_client
* [-no_tlsext addition] (https://github.com/PeterMosmans/openssl/commit/c1348037c3bdf6a2c024f3572f0d1141b5d57e4f)
* -proxy (RT #2651)
* -starttls telnet (RT #2451)
* [-starttls xmpp improvement (RT #2860)](https://github.com/PeterMosmans/openssl/commit/854eb9c88da8b742c1d77a11058fcd0d4036c0da)
* [-starttls ldap support (RT #2665)](https://github.com/PeterMosmans/openssl/commit/f7e338776d998cb2f2d9ff133473cc87b337821a)
* [-fix Windows blocking (RT #3464)](https://github.com/PeterMosmans/openssl/commit/68ab9b308e173072e5015063be7e194bec1f311f)

##### generic
* Minor changes to Makefiles to simplify building using the mingw / mingw64 platform on Windows
* [-universal build time instead of local build time](https://github.com/PeterMosmans/openssl/commit/51cf1c9043efdc06937c0d3550ff8f6fd8e43e1f)
* [Test all SSL ciphers by default (RT #2584)](https://github.com/PeterMosmans/openssl/commit/85f54b0907f8b7bd67336b742b162effb154ed20)


#### Thanks to
* [Dirk Wetter](https://github.com/drwetter)
* [Hubert Kario](https://github.com/tomato42)
* [Stefan Zehl](https://github.com/Sec42)


#### Windows binaries
The latest binary Windows 64-bit builds of these branches can be found at http://onwebsecurity.com/cryptography/openssl
The OpenSSL version string of these binaries (#define OPENSSL_VERSION_TEXT) include the branch name and commit hash.

Please see the official OpenSSL repository for all relevant license / copyright info. This repository is merely a fork of their great work with some minimal merges, additions and changes.

#### GOST support
Note that you'll have to make sure that your openssl.cnf contains the following lines to use GOST ciphers:

```
openssl_conf=openssl_def

[openssl_def]
engines=engine_section

[engine_section]
gost=gost_section

[gost_section]
default_algorithms=ALL
CRYPT_PARAMS=id-Gost28147-89-CryptoPro-A-ParamSet
```

#### Supported ciphers
Currently 183

```
openssl ciphers -l -V "ALL:COMPLEMENTOFALL"

          0xCC,0x14 - ECDHE-ECDSA-CHACHA20-POLY1305 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=ChaCha20(256) Mac=AEAD
          0xCC,0x13 - ECDHE-RSA-CHACHA20-POLY1305 TLSv1.2 Kx=ECDH     Au=RSA  Enc=ChaCha20(256) Mac=AEAD
          0xCC,0x15 - DHE-RSA-CHACHA20-POLY1305 TLSv1.2 Kx=DH       Au=RSA  Enc=ChaCha20(256) Mac=AEAD
          0xC0,0x30 - ECDHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AESGCM(256) Mac=AEAD
          0xC0,0x2C - ECDHE-ECDSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AESGCM(256) Mac=AEAD
          0xC0,0x28 - ECDHE-RSA-AES256-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AES(256)  Mac=SHA384
          0xC0,0x24 - ECDHE-ECDSA-AES256-SHA384 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AES(256)  Mac=SHA384
          0xC0,0x14 - ECDHE-RSA-AES256-SHA    SSLv3 Kx=ECDH     Au=RSA  Enc=AES(256)  Mac=SHA1
          0xC0,0x0A - ECDHE-ECDSA-AES256-SHA  SSLv3 Kx=ECDH     Au=ECDSA Enc=AES(256)  Mac=SHA1
          0xC0,0x22 - SRP-DSS-AES-256-CBC-SHA SSLv3 Kx=SRP      Au=DSS  Enc=AES(256)  Mac=SHA1
          0xC0,0x21 - SRP-RSA-AES-256-CBC-SHA SSLv3 Kx=SRP      Au=RSA  Enc=AES(256)  Mac=SHA1
          0xC0,0x20 - SRP-AES-256-CBC-SHA     SSLv3 Kx=SRP      Au=SRP  Enc=AES(256)  Mac=SHA1
          0x00,0xA5 - DH-DSS-AES256-GCM-SHA384 TLSv1.2 Kx=DH/DSS   Au=DH   Enc=AESGCM(256) Mac=AEAD
          0x00,0xA3 - DHE-DSS-AES256-GCM-SHA384 TLSv1.2 Kx=DH       Au=DSS  Enc=AESGCM(256) Mac=AEAD
          0x00,0xA1 - DH-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=DH/RSA   Au=DH   Enc=AESGCM(256) Mac=AEAD
          0x00,0x9F - DHE-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=DH       Au=RSA  Enc=AESGCM(256) Mac=AEAD
          0x00,0x6B - DHE-RSA-AES256-SHA256   TLSv1.2 Kx=DH       Au=RSA  Enc=AES(256)  Mac=SHA256
          0x00,0x6A - DHE-DSS-AES256-SHA256   TLSv1.2 Kx=DH       Au=DSS  Enc=AES(256)  Mac=SHA256
          0x00,0x69 - DH-RSA-AES256-SHA256    TLSv1.2 Kx=DH/RSA   Au=DH   Enc=AES(256)  Mac=SHA256
          0x00,0x68 - DH-DSS-AES256-SHA256    TLSv1.2 Kx=DH/DSS   Au=DH   Enc=AES(256)  Mac=SHA256
          0x00,0x39 - DHE-RSA-AES256-SHA      SSLv3 Kx=DH       Au=RSA  Enc=AES(256)  Mac=SHA1
          0x00,0x38 - DHE-DSS-AES256-SHA      SSLv3 Kx=DH       Au=DSS  Enc=AES(256)  Mac=SHA1
          0x00,0x37 - DH-RSA-AES256-SHA       SSLv3 Kx=DH/RSA   Au=DH   Enc=AES(256)  Mac=SHA1
          0x00,0x36 - DH-DSS-AES256-SHA       SSLv3 Kx=DH/DSS   Au=DH   Enc=AES(256)  Mac=SHA1
          0xC0,0x77 - ECDHE-RSA-CAMELLIA256-SHA384 TLSv1.2 Kx=ECDH     Au=RSA  Enc=Camellia(256) Mac=SHA384
          0xC0,0x73 - ECDHE-ECDSA-CAMELLIA256-SHA384 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=Camellia(256) Mac=SHA384
          0x00,0xC4 - DHE-RSA-CAMELLIA256-SHA256 TLSv1.2 Kx=DH       Au=RSA  Enc=Camellia(256) Mac=SHA256
          0x00,0xC3 - DHE-DSS-CAMELLIA256-SHA256 TLSv1.2 Kx=DH       Au=DSS  Enc=Camellia(256) Mac=SHA256
          0x00,0xC2 - DH-RSA-CAMELLIA256-SHA256 TLSv1.2 Kx=DH/RSA   Au=DH   Enc=Camellia(256) Mac=SHA256
          0x00,0xC1 - DH-DSS-CAMELLIA256-SHA256 TLSv1.2 Kx=DH/DSS   Au=DH   Enc=Camellia(256) Mac=SHA256
          0x00,0x88 - DHE-RSA-CAMELLIA256-SHA SSLv3 Kx=DH       Au=RSA  Enc=Camellia(256) Mac=SHA1
          0x00,0x87 - DHE-DSS-CAMELLIA256-SHA SSLv3 Kx=DH       Au=DSS  Enc=Camellia(256) Mac=SHA1
          0x00,0x86 - DH-RSA-CAMELLIA256-SHA  SSLv3 Kx=DH/RSA   Au=DH   Enc=Camellia(256) Mac=SHA1
          0x00,0x85 - DH-DSS-CAMELLIA256-SHA  SSLv3 Kx=DH/DSS   Au=DH   Enc=Camellia(256) Mac=SHA1
          0x00,0x81 - GOST2001-GOST89-GOST89  SSLv3 Kx=GOST     Au=GOST01 Enc=GOST89(256) Mac=GOST89
          0x00,0x80 - GOST94-GOST89-GOST89    SSLv3 Kx=GOST     Au=GOST94 Enc=GOST89(256) Mac=GOST89
          0xC0,0x19 - AECDH-AES256-SHA        SSLv3 Kx=ECDH     Au=None Enc=AES(256)  Mac=SHA1
          0x00,0xA7 - ADH-AES256-GCM-SHA384   TLSv1.2 Kx=DH       Au=None Enc=AESGCM(256) Mac=AEAD
          0x00,0x6D - ADH-AES256-SHA256       TLSv1.2 Kx=DH       Au=None Enc=AES(256)  Mac=SHA256
          0x00,0x3A - ADH-AES256-SHA          SSLv3 Kx=DH       Au=None Enc=AES(256)  Mac=SHA1
          0x00,0xC5 - ADH-CAMELLIA256-SHA256  TLSv1.2 Kx=DH       Au=None Enc=Camellia(256) Mac=SHA256
          0x00,0x89 - ADH-CAMELLIA256-SHA     SSLv3 Kx=DH       Au=None Enc=Camellia(256) Mac=SHA1
          0xC0,0x32 - ECDH-RSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=AESGCM(256) Mac=AEAD
          0xC0,0x2E - ECDH-ECDSA-AES256-GCM-SHA384 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=AESGCM(256) Mac=AEAD
          0xC0,0x2A - ECDH-RSA-AES256-SHA384  TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=AES(256)  Mac=SHA384
          0xC0,0x26 - ECDH-ECDSA-AES256-SHA384 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=AES(256)  Mac=SHA384
          0xC0,0x0F - ECDH-RSA-AES256-SHA     SSLv3 Kx=ECDH/RSA Au=ECDH Enc=AES(256)  Mac=SHA1
          0xC0,0x05 - ECDH-ECDSA-AES256-SHA   SSLv3 Kx=ECDH/ECDSA Au=ECDH Enc=AES(256)  Mac=SHA1
          0xC0,0x79 - ECDH-RSA-CAMELLIA256-SHA384 TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=Camellia(256) Mac=SHA384
          0xC0,0x75 - ECDH-ECDSA-CAMELLIA256-SHA384 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=Camellia(256) Mac=SHA384
          0x00,0x9D - AES256-GCM-SHA384       TLSv1.2 Kx=RSA      Au=RSA  Enc=AESGCM(256) Mac=AEAD
          0x00,0x3D - AES256-SHA256           TLSv1.2 Kx=RSA      Au=RSA  Enc=AES(256)  Mac=SHA256
          0x00,0x35 - AES256-SHA              SSLv3 Kx=RSA      Au=RSA  Enc=AES(256)  Mac=SHA1
          0x00,0xC0 - CAMELLIA256-SHA256      TLSv1.2 Kx=RSA      Au=RSA  Enc=Camellia(256) Mac=SHA256
          0x00,0x84 - CAMELLIA256-SHA         SSLv3 Kx=RSA      Au=RSA  Enc=Camellia(256) Mac=SHA1
          0x00,0x95 - RSA-PSK-AES256-CBC-SHA  SSLv3 Kx=RSAPSK   Au=RSA  Enc=AES(256)  Mac=SHA1
          0x00,0x8D - PSK-AES256-CBC-SHA      SSLv3 Kx=PSK      Au=PSK  Enc=AES(256)  Mac=SHA1
          0xC0,0x2F - ECDHE-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AESGCM(128) Mac=AEAD
          0xC0,0x2B - ECDHE-ECDSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AESGCM(128) Mac=AEAD
          0xC0,0x27 - ECDHE-RSA-AES128-SHA256 TLSv1.2 Kx=ECDH     Au=RSA  Enc=AES(128)  Mac=SHA256
          0xC0,0x23 - ECDHE-ECDSA-AES128-SHA256 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=AES(128)  Mac=SHA256
          0xC0,0x13 - ECDHE-RSA-AES128-SHA    SSLv3 Kx=ECDH     Au=RSA  Enc=AES(128)  Mac=SHA1
          0xC0,0x09 - ECDHE-ECDSA-AES128-SHA  SSLv3 Kx=ECDH     Au=ECDSA Enc=AES(128)  Mac=SHA1
          0xC0,0x1F - SRP-DSS-AES-128-CBC-SHA SSLv3 Kx=SRP      Au=DSS  Enc=AES(128)  Mac=SHA1
          0xC0,0x1E - SRP-RSA-AES-128-CBC-SHA SSLv3 Kx=SRP      Au=RSA  Enc=AES(128)  Mac=SHA1
          0xC0,0x1D - SRP-AES-128-CBC-SHA     SSLv3 Kx=SRP      Au=SRP  Enc=AES(128)  Mac=SHA1
          0x00,0xA4 - DH-DSS-AES128-GCM-SHA256 TLSv1.2 Kx=DH/DSS   Au=DH   Enc=AESGCM(128) Mac=AEAD
          0x00,0xA2 - DHE-DSS-AES128-GCM-SHA256 TLSv1.2 Kx=DH       Au=DSS  Enc=AESGCM(128) Mac=AEAD
          0x00,0xA0 - DH-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=DH/RSA   Au=DH   Enc=AESGCM(128) Mac=AEAD
          0x00,0x9E - DHE-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=DH       Au=RSA  Enc=AESGCM(128) Mac=AEAD
          0x00,0x67 - DHE-RSA-AES128-SHA256   TLSv1.2 Kx=DH       Au=RSA  Enc=AES(128)  Mac=SHA256
          0x00,0x40 - DHE-DSS-AES128-SHA256   TLSv1.2 Kx=DH       Au=DSS  Enc=AES(128)  Mac=SHA256
          0x00,0x3F - DH-RSA-AES128-SHA256    TLSv1.2 Kx=DH/RSA   Au=DH   Enc=AES(128)  Mac=SHA256
          0x00,0x3E - DH-DSS-AES128-SHA256    TLSv1.2 Kx=DH/DSS   Au=DH   Enc=AES(128)  Mac=SHA256
          0x00,0x33 - DHE-RSA-AES128-SHA      SSLv3 Kx=DH       Au=RSA  Enc=AES(128)  Mac=SHA1
          0x00,0x32 - DHE-DSS-AES128-SHA      SSLv3 Kx=DH       Au=DSS  Enc=AES(128)  Mac=SHA1
          0x00,0x31 - DH-RSA-AES128-SHA       SSLv3 Kx=DH/RSA   Au=DH   Enc=AES(128)  Mac=SHA1
          0x00,0x30 - DH-DSS-AES128-SHA       SSLv3 Kx=DH/DSS   Au=DH   Enc=AES(128)  Mac=SHA1
          0xC0,0x76 - ECDHE-RSA-CAMELLIA128-SHA256 TLSv1.2 Kx=ECDH     Au=RSA  Enc=Camellia(128) Mac=SHA256
          0xC0,0x72 - ECDHE-ECDSA-CAMELLIA128-SHA256 TLSv1.2 Kx=ECDH     Au=ECDSA Enc=Camellia(128) Mac=SHA256
          0x00,0xBE - DHE-RSA-CAMELLIA128-SHA256 TLSv1.2 Kx=DH       Au=RSA  Enc=Camellia(128) Mac=SHA256
          0x00,0xBD - DHE-DSS-CAMELLIA128-SHA256 TLSv1.2 Kx=DH       Au=DSS  Enc=Camellia(128) Mac=SHA256
          0x00,0xBC - DH-RSA-CAMELLIA128-SHA256 TLSv1.2 Kx=DH/RSA   Au=DH   Enc=Camellia(128) Mac=SHA256
          0x00,0xBB - DH-DSS-CAMELLIA128-SHA256 TLSv1.2 Kx=DH/DSS   Au=DH   Enc=Camellia(128) Mac=SHA256
          0x00,0x9A - DHE-RSA-SEED-SHA        SSLv3 Kx=DH       Au=RSA  Enc=SEED(128) Mac=SHA1
          0x00,0x99 - DHE-DSS-SEED-SHA        SSLv3 Kx=DH       Au=DSS  Enc=SEED(128) Mac=SHA1
          0x00,0x98 - DH-RSA-SEED-SHA         SSLv3 Kx=DH/RSA   Au=DH   Enc=SEED(128) Mac=SHA1
          0x00,0x97 - DH-DSS-SEED-SHA         SSLv3 Kx=DH/DSS   Au=DH   Enc=SEED(128) Mac=SHA1
          0x00,0x45 - DHE-RSA-CAMELLIA128-SHA SSLv3 Kx=DH       Au=RSA  Enc=Camellia(128) Mac=SHA1
          0x00,0x44 - DHE-DSS-CAMELLIA128-SHA SSLv3 Kx=DH       Au=DSS  Enc=Camellia(128) Mac=SHA1
          0x00,0x43 - DH-RSA-CAMELLIA128-SHA  SSLv3 Kx=DH/RSA   Au=DH   Enc=Camellia(128) Mac=SHA1
          0x00,0x42 - DH-DSS-CAMELLIA128-SHA  SSLv3 Kx=DH/DSS   Au=DH   Enc=Camellia(128) Mac=SHA1
          0xC0,0x18 - AECDH-AES128-SHA        SSLv3 Kx=ECDH     Au=None Enc=AES(128)  Mac=SHA1
          0x00,0xA6 - ADH-AES128-GCM-SHA256   TLSv1.2 Kx=DH       Au=None Enc=AESGCM(128) Mac=AEAD
          0x00,0x6C - ADH-AES128-SHA256       TLSv1.2 Kx=DH       Au=None Enc=AES(128)  Mac=SHA256
          0x00,0x34 - ADH-AES128-SHA          SSLv3 Kx=DH       Au=None Enc=AES(128)  Mac=SHA1
          0x00,0xBF - ADH-CAMELLIA128-SHA256  TLSv1.2 Kx=DH       Au=None Enc=Camellia(128) Mac=SHA256
          0x00,0x9B - ADH-SEED-SHA            SSLv3 Kx=DH       Au=None Enc=SEED(128) Mac=SHA1
          0x00,0x46 - ADH-CAMELLIA128-SHA     SSLv3 Kx=DH       Au=None Enc=Camellia(128) Mac=SHA1
          0xC0,0x31 - ECDH-RSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=AESGCM(128) Mac=AEAD
          0xC0,0x2D - ECDH-ECDSA-AES128-GCM-SHA256 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=AESGCM(128) Mac=AEAD
          0xC0,0x29 - ECDH-RSA-AES128-SHA256  TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=AES(128)  Mac=SHA256
          0xC0,0x25 - ECDH-ECDSA-AES128-SHA256 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=AES(128)  Mac=SHA256
          0xC0,0x0E - ECDH-RSA-AES128-SHA     SSLv3 Kx=ECDH/RSA Au=ECDH Enc=AES(128)  Mac=SHA1
          0xC0,0x04 - ECDH-ECDSA-AES128-SHA   SSLv3 Kx=ECDH/ECDSA Au=ECDH Enc=AES(128)  Mac=SHA1
          0xC0,0x78 - ECDH-RSA-CAMELLIA128-SHA256 TLSv1.2 Kx=ECDH/RSA Au=ECDH Enc=Camellia(128) Mac=SHA256
          0xC0,0x74 - ECDH-ECDSA-CAMELLIA128-SHA256 TLSv1.2 Kx=ECDH/ECDSA Au=ECDH Enc=Camellia(128) Mac=SHA256
          0x00,0x9C - AES128-GCM-SHA256       TLSv1.2 Kx=RSA      Au=RSA  Enc=AESGCM(128) Mac=AEAD
          0x00,0x3C - AES128-SHA256           TLSv1.2 Kx=RSA      Au=RSA  Enc=AES(128)  Mac=SHA256
          0x00,0x2F - AES128-SHA              SSLv3 Kx=RSA      Au=RSA  Enc=AES(128)  Mac=SHA1
          0x00,0xBA - CAMELLIA128-SHA256      TLSv1.2 Kx=RSA      Au=RSA  Enc=Camellia(128) Mac=SHA256
          0x00,0x96 - SEED-SHA                SSLv3 Kx=RSA      Au=RSA  Enc=SEED(128) Mac=SHA1
          0x00,0x41 - CAMELLIA128-SHA         SSLv3 Kx=RSA      Au=RSA  Enc=Camellia(128) Mac=SHA1
          0x00,0x07 - IDEA-CBC-SHA            SSLv3 Kx=RSA      Au=RSA  Enc=IDEA(128) Mac=SHA1
     0x05,0x00,0x80 - IDEA-CBC-MD5            SSLv2 Kx=RSA      Au=RSA  Enc=IDEA(128) Mac=MD5
     0x03,0x00,0x80 - RC2-CBC-MD5             SSLv2 Kx=RSA      Au=RSA  Enc=RC2(128)  Mac=MD5
          0x00,0x94 - RSA-PSK-AES128-CBC-SHA  SSLv3 Kx=RSAPSK   Au=RSA  Enc=AES(128)  Mac=SHA1
          0x00,0x8C - PSK-AES128-CBC-SHA      SSLv3 Kx=PSK      Au=PSK  Enc=AES(128)  Mac=SHA1
          0xC0,0x11 - ECDHE-RSA-RC4-SHA       SSLv3 Kx=ECDH     Au=RSA  Enc=RC4(128)  Mac=SHA1
          0xC0,0x07 - ECDHE-ECDSA-RC4-SHA     SSLv3 Kx=ECDH     Au=ECDSA Enc=RC4(128)  Mac=SHA1
          0x00,0x66 - DHE-DSS-RC4-SHA         SSLv3 Kx=DH       Au=DSS  Enc=RC4(128)  Mac=SHA1
          0xC0,0x16 - AECDH-RC4-SHA           SSLv3 Kx=ECDH     Au=None Enc=RC4(128)  Mac=SHA1
          0x00,0x18 - ADH-RC4-MD5             SSLv3 Kx=DH       Au=None Enc=RC4(128)  Mac=MD5
          0xC0,0x0C - ECDH-RSA-RC4-SHA        SSLv3 Kx=ECDH/RSA Au=ECDH Enc=RC4(128)  Mac=SHA1
          0xC0,0x02 - ECDH-ECDSA-RC4-SHA      SSLv3 Kx=ECDH/ECDSA Au=ECDH Enc=RC4(128)  Mac=SHA1
          0x00,0x05 - RC4-SHA                 SSLv3 Kx=RSA      Au=RSA  Enc=RC4(128)  Mac=SHA1
          0x00,0x04 - RC4-MD5                 SSLv3 Kx=RSA      Au=RSA  Enc=RC4(128)  Mac=MD5
     0x01,0x00,0x80 - RC4-MD5                 SSLv2 Kx=RSA      Au=RSA  Enc=RC4(128)  Mac=MD5
          0x00,0x92 - RSA-PSK-RC4-SHA         SSLv3 Kx=RSAPSK   Au=RSA  Enc=RC4(128)  Mac=SHA1
          0x00,0x8A - PSK-RC4-SHA             SSLv3 Kx=PSK      Au=PSK  Enc=RC4(128)  Mac=SHA1
          0xC0,0x12 - ECDHE-RSA-DES-CBC3-SHA  SSLv3 Kx=ECDH     Au=RSA  Enc=3DES(168) Mac=SHA1
          0xC0,0x08 - ECDHE-ECDSA-DES-CBC3-SHA SSLv3 Kx=ECDH     Au=ECDSA Enc=3DES(168) Mac=SHA1
          0xC0,0x1C - SRP-DSS-3DES-EDE-CBC-SHA SSLv3 Kx=SRP      Au=DSS  Enc=3DES(168) Mac=SHA1
          0xC0,0x1B - SRP-RSA-3DES-EDE-CBC-SHA SSLv3 Kx=SRP      Au=RSA  Enc=3DES(168) Mac=SHA1
          0xC0,0x1A - SRP-3DES-EDE-CBC-SHA    SSLv3 Kx=SRP      Au=SRP  Enc=3DES(168) Mac=SHA1
          0x00,0x16 - EDH-RSA-DES-CBC3-SHA    SSLv3 Kx=DH       Au=RSA  Enc=3DES(168) Mac=SHA1
          0x00,0x13 - EDH-DSS-DES-CBC3-SHA    SSLv3 Kx=DH       Au=DSS  Enc=3DES(168) Mac=SHA1
          0x00,0x10 - DH-RSA-DES-CBC3-SHA     SSLv3 Kx=DH/RSA   Au=DH   Enc=3DES(168) Mac=SHA1
          0x00,0x0D - DH-DSS-DES-CBC3-SHA     SSLv3 Kx=DH/DSS   Au=DH   Enc=3DES(168) Mac=SHA1
          0xC0,0x17 - AECDH-DES-CBC3-SHA      SSLv3 Kx=ECDH     Au=None Enc=3DES(168) Mac=SHA1
          0x00,0x1B - ADH-DES-CBC3-SHA        SSLv3 Kx=DH       Au=None Enc=3DES(168) Mac=SHA1
          0xC0,0x0D - ECDH-RSA-DES-CBC3-SHA   SSLv3 Kx=ECDH/RSA Au=ECDH Enc=3DES(168) Mac=SHA1
          0xC0,0x03 - ECDH-ECDSA-DES-CBC3-SHA SSLv3 Kx=ECDH/ECDSA Au=ECDH Enc=3DES(168) Mac=SHA1
          0x00,0x0A - DES-CBC3-SHA            SSLv3 Kx=RSA      Au=RSA  Enc=3DES(168) Mac=SHA1
     0x07,0x00,0xC0 - DES-CBC3-MD5            SSLv2 Kx=RSA      Au=RSA  Enc=3DES(168) Mac=MD5
          0x00,0x93 - RSA-PSK-3DES-EDE-CBC-SHA SSLv3 Kx=RSAPSK   Au=RSA  Enc=3DES(168) Mac=SHA1
          0x00,0x8B - PSK-3DES-EDE-CBC-SHA    SSLv3 Kx=PSK      Au=PSK  Enc=3DES(168) Mac=SHA1
     0x08,0x00,0x80 - RC4-64-MD5              SSLv2 Kx=RSA      Au=RSA  Enc=RC4(64)   Mac=MD5
          0x00,0x63 - EXP1024-DHE-DSS-DES-CBC-SHA SSLv3 Kx=DH(1024) Au=DSS  Enc=DES(56)   Mac=SHA1 export
          0x00,0x15 - EDH-RSA-DES-CBC-SHA     SSLv3 Kx=DH       Au=RSA  Enc=DES(56)   Mac=SHA1
          0x00,0x12 - EDH-DSS-DES-CBC-SHA     SSLv3 Kx=DH       Au=DSS  Enc=DES(56)   Mac=SHA1
          0x00,0x0F - DH-RSA-DES-CBC-SHA      SSLv3 Kx=DH/RSA   Au=DH   Enc=DES(56)   Mac=SHA1
          0x00,0x0C - DH-DSS-DES-CBC-SHA      SSLv3 Kx=DH/DSS   Au=DH   Enc=DES(56)   Mac=SHA1
          0x00,0x1A - ADH-DES-CBC-SHA         SSLv3 Kx=DH       Au=None Enc=DES(56)   Mac=SHA1
          0x00,0x62 - EXP1024-DES-CBC-SHA     SSLv3 Kx=RSA(1024) Au=RSA  Enc=DES(56)   Mac=SHA1 export
          0x00,0x09 - DES-CBC-SHA             SSLv3 Kx=RSA      Au=RSA  Enc=DES(56)   Mac=SHA1
          0x00,0x61 - EXP1024-RC2-CBC-MD5     SSLv3 Kx=RSA(1024) Au=RSA  Enc=RC2(56)   Mac=MD5  export
     0x06,0x00,0x40 - DES-CBC-MD5             SSLv2 Kx=RSA      Au=RSA  Enc=DES(56)   Mac=MD5
          0x00,0x65 - EXP1024-DHE-DSS-RC4-SHA SSLv3 Kx=DH(1024) Au=DSS  Enc=RC4(56)   Mac=SHA1 export
          0x00,0x64 - EXP1024-RC4-SHA         SSLv3 Kx=RSA(1024) Au=RSA  Enc=RC4(56)   Mac=SHA1 export
          0x00,0x60 - EXP1024-RC4-MD5         SSLv3 Kx=RSA(1024) Au=RSA  Enc=RC4(56)   Mac=MD5  export
          0x00,0x14 - EXP-EDH-RSA-DES-CBC-SHA SSLv3 Kx=DH(512)  Au=RSA  Enc=DES(40)   Mac=SHA1 export
          0x00,0x11 - EXP-EDH-DSS-DES-CBC-SHA SSLv3 Kx=DH(512)  Au=DSS  Enc=DES(40)   Mac=SHA1 export
          0x00,0x0E - EXP-DH-RSA-DES-CBC-SHA  SSLv3 Kx=DH/RSA   Au=DH   Enc=DES(40)   Mac=SHA1 export
          0x00,0x0B - EXP-DH-DSS-DES-CBC-SHA  SSLv3 Kx=DH/DSS   Au=DH   Enc=DES(40)   Mac=SHA1 export
          0x00,0x19 - EXP-ADH-DES-CBC-SHA     SSLv3 Kx=DH(512)  Au=None Enc=DES(40)   Mac=SHA1 export
          0x00,0x08 - EXP-DES-CBC-SHA         SSLv3 Kx=RSA(512) Au=RSA  Enc=DES(40)   Mac=SHA1 export
          0x00,0x06 - EXP-RC2-CBC-MD5         SSLv3 Kx=RSA(512) Au=RSA  Enc=RC2(40)   Mac=MD5  export
     0x04,0x00,0x80 - EXP-RC2-CBC-MD5         SSLv2 Kx=RSA(512) Au=RSA  Enc=RC2(40)   Mac=MD5  export
          0x00,0x17 - EXP-ADH-RC4-MD5         SSLv3 Kx=DH(512)  Au=None Enc=RC4(40)   Mac=MD5  export
          0x00,0x03 - EXP-RC4-MD5             SSLv3 Kx=RSA(512) Au=RSA  Enc=RC4(40)   Mac=MD5  export
     0x02,0x00,0x80 - EXP-RC4-MD5             SSLv2 Kx=RSA(512) Au=RSA  Enc=RC4(40)   Mac=MD5  export
          0xC0,0x10 - ECDHE-RSA-NULL-SHA      SSLv3 Kx=ECDH     Au=RSA  Enc=None      Mac=SHA1
          0xC0,0x06 - ECDHE-ECDSA-NULL-SHA    SSLv3 Kx=ECDH     Au=ECDSA Enc=None      Mac=SHA1
          0x00,0x83 - GOST2001-NULL-GOST94    SSLv3 Kx=GOST     Au=GOST01 Enc=None      Mac=GOST94
          0x00,0x82 - GOST94-NULL-GOST94      SSLv3 Kx=GOST     Au=GOST94 Enc=None      Mac=GOST94
          0xC0,0x15 - AECDH-NULL-SHA          SSLv3 Kx=ECDH     Au=None Enc=None      Mac=SHA1
          0xC0,0x0B - ECDH-RSA-NULL-SHA       SSLv3 Kx=ECDH/RSA Au=ECDH Enc=None      Mac=SHA1
          0xC0,0x01 - ECDH-ECDSA-NULL-SHA     SSLv3 Kx=ECDH/ECDSA Au=ECDH Enc=None      Mac=SHA1
          0x00,0x3B - NULL-SHA256             TLSv1.2 Kx=RSA      Au=RSA  Enc=None      Mac=SHA256
          0x00,0x02 - NULL-SHA                SSLv3 Kx=RSA      Au=RSA  Enc=None      Mac=SHA1
          0x00,0x01 - NULL-MD5                SSLv3 Kx=RSA      Au=RSA  Enc=None      Mac=MD5
     0x00,0x00,0x00 - NULL-MD5                SSLv2 Kx=RSA(512) Au=RSA  Enc=None      Mac=MD5  export
```
