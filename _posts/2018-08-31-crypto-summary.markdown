---
layout: page
title:  "Cryptography --- Introduction"
date:   2018-6-7 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

# Summary of Modern Cryptography

## Terms summary

1. Commitment Scheme
2. Symmetric cryptosystems
3. DH key exchange
4. Digital Signatures
5. Zero Knowledge Proof
6. Public-Key encryption
7. ...

## Symmetric Cryptographic Algorithms, originally designed for encryption

Initially requires a secure key exchange channel (such as DH key exchange). Simpler and faster.

1. Stream Cipher
- OTP

2. Block Cipher
- DES
- AES

Cryptographic primitives (other than encryption)

1. Message Authentication Code (MAC)
2. Hash Functions

## Asymmetric Cryptographic Algorithms (public key cryptography)

Often related to mathematical problems, such as integer factorization, discrete logarithm and elliptic curve. Do not require a initial secret exchange process. Because of the computational complexity of asymmetric encryption, it is usually used only for small blocks of data, typically the transfer of a symmetric encryption key.

1. RSA | based on extreme difficulty of factoring large integers
2. Diffie-Hellman Key exchage | based on DDH (not really a asymmetric algorithm, but a key exchange method for symmetric keys)
3. EIGamal | based on DL
4. Elliptic curve | based on DL

Cryptographic primitives (other than encryption)

- Digital Signature



