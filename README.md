OpenSSL
================

This is a fork of the official OpenSSL github repository at https://github.com/openssl/openssl.git

The main reason of the fork is to include ChaCha20 and Poly1305 ciphers on active branches, to make it compile without any issues on mingw64 and to add some extra features to s_client.

Active branches:
* 1.0.1-chacha (aligns with official OpenSSL_1_0_1-stable branch)
  * Added ChaCha20 and Poly1305 ciphers (backported from the official 1.0.2-aead branch)
  * Minor changes to Makefiles to simplify building using the mingw (mingw64) platform on Windows

* 1.0.2-chacha (aligns with official OpenSSL_1_0_2-stable branch)
  * Added ChaCha20 and Poly1305 ciphers (backported from the official 1.0.2-aead branch)
  * Minor changes to Makefiles to simplify building using the mingw (mingw64) platform on Windows
  * Minor additions to s_client

As said, these branches can be compiled directly without any modification on Windows using mingw / mingw64 and pass all tests.

The latest binary Windows 64-bit builds of these branches can be found at http://onwebsecurity.com/cryptography/openssl
The version string of thses binaries includes the branch name and commit hash.

Please see the official OpenSSL repository for all relevant license / copyright info. This repository is merely a fork of their great work with some minimal additions and changes.
