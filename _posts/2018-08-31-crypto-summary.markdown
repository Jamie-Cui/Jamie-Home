---
layout: page
title:  "Summary of Modern Cryptography"
date:   2018-6-7 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}
---
# Terms summary

1. Commitment Scheme
2. Symmetric cryptosystems
3. DH key exchange
4. Digital Signatures, including *Key Generation*, *Signing*, *Verifying*
5. Zero Knowledge Proof, including 
6. Public-Key encryption
7. ...

---
# Symmetric Cryptographic Algorithms, originally designed for encryption

Before the secure communication between parties, the encryption key must be exchanged beforehand. Generally the exchange process can be competed by public key cryptography, which will be talked about in the next section.

The advantages for symmetric cryptographuc algrothms is simple and fast, because they ususally depends on XOR or bit operations, which makes it the perfect match for long-time encryption.

1. Stream Cipher (see [Blog about Stream Cipher]({{site.url}}{{site.baseurl}}/crypto-stream-cipher.html))
- OTP
- RC4
- WEP
- LFSR
- CSS using LFSR

2. Block Cipher (see [Blog about Block Cipher]({{site.url}}{{site.baseurl}}/crypto-block-cipher.html))
- DES, key size: 56.
- 2DES, key size: 112 .
- 3DES, key size: 168.
- AES, block size: 128; key size: 128, 192, 256.

Cryptographic primitives (other than encryption)

1. Message Authentication Code (MAC)
2. Hash Functions

---
# Asymmetric Cryptographic Algorithms (public key cryptography)

Often related to mathematical problems, such as integer factorization, discrete logarithm and elliptic curve. Do not require a initial secret exchange process. Because of the computational complexity of asymmetric encryption, it is usually used only for small blocks of data, typically the transfer of a symmetric encryption key.

1. RSA, based on extreme difficulty of factoring large integers
2. Diffie-Hellman Key exchage, based on DDH
3. EIGamal, based on DL
4. Elliptic curve, based on DL

Cryptographic primitives (other than encryption)

- Digital Signature

---
# RSA

RSA cryptosystem is developed in 1977 by Rivest, Shamir and Adleman. It was the first public-key encryption scheme which could both sign and encrypt messages. As with the Diffie-Hellman key exchange protocol, the RSA cryptosystem enables two parties to communicate over a public channel.

**Encryption** 

Suppose Alice wants to send Bob a secure message over an insecure public channel.

1. Parameter Generation (Bob): Choosing two distinct large prime number *p* and *q*, computing *n = p\*q*, *n* is used later as the modulus, the serilised length of *n* is also the key length (or security parameter).
2. Key Generation (Bob): Choosing an integer from *(1, (p-1)(q-1))* as e and find *e*'s modular multiplicative inverse *d*. In the other words, *e\*d mod n = 1*.
3. Encrypt message m (Alice): <img src="http://chart.googleapis.com/chart?cht=tx&chl= y=m^e\quad mod\quad n" style="border:none;">, send *y* to Bob.
4. Decrypt cipher y (Bob): <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=y^d" style="border:none;">

**Why large number -> e and d?**

For an attacker, the simpliest way of getting the message is to factor 

**Proof of Correctness** 

Dec(Enc(m)) : <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=(m^e\quad mod\quad n)^d = m^{e*d}\quad mod\quad n = m^n\quad mod\quad n" style="border:none;">

Is m equals to <img src="http://chart.googleapis.com/chart?cht=tx&chl= m^n\quad mod\quad n" style="border:none;">? Using group theory, we may notice that "m^x mod n" operation generates a cyclic group ranging (0, n-1). According to number thery, we have <img src="http://chart.googleapis.com/chart?cht=tx&chl= \varphi(n)" style="border:none;"> indicating the number of non-negative integers less than n that are relatively prime to n.


