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

**For Encryption** 

Suppose Alice wants to send Bob a secure message over an insecure public channel.

- Bob selects two distinct large prime number *p* and *q*, and compute *n = p\*q*.
- Bob randomly chooses an integer *e* *(1 < e < <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;">)*, then find *d* which satisfies *e\*d mod <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;"> = 1*.
- Encrypt message m: <img src="http://chart.googleapis.com/chart?cht=tx&chl= y=m^e\quad mod\quad n" style="border:none;">, send *y* to Bob.
- Decrypt cipher y: <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=y^d" style="border:none;">

Nonsensitive data: n, e; Confidential data: p, q, toient function of n, d.

**Proof of Correctness** 

Dec(Enc(m)) : <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=(m^e\quad mod\quad n)^d = m^{e*d}\quad mod\quad n" style="border:none;">

As we have defined, e\*d mod <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;"> =1. Then e\*d = 1+ <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;">*k. So we now have <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=m*m^{k*\varphi (n)}\quad mod\quad n" style="border:none;">, and using Euler's Theorem we know that <img src="http://chart.googleapis.com/chart?cht=tx&chl= m^{\varphi (n)}\quad mod\quad n" style="border:none;"> is 1. Correctness proofed.

> [Euler's Theorem](https://en.wikipedia.org/wiki/Euler%27s_theorem): 
if *n* and *a* are coprime, then  <img src="http://chart.googleapis.com/chart?cht=tx&chl= a^{\varphi (n)}\quad mod\quad n \equiv 1" style="border:none;"> 

- Q: Why large prime? Otherwise it is esay to find phi(n) in PPT.
- Q: Why using phi(n) (totient function)? Because it is mathematical-difficult to find phi(n) using the large number n. We may recall the relationship between e, d and phi(n) (acts as the modular), if the modular is exposed, it is easy to find the inverse private key in polynomial time.
- Q: Why e has to be smaller than phi(n)? Because encryption key e and decryption key d are elements in group phi(n).
- Q: Is there always a corresponding d?  Yes, according to the definition of group, a set G is recognised as a group under the condition that G is closed under inversion, which means for all elements in G, there exists a inverse element.
- [Q: Why toitent function is credential?](https://crypto.stackexchange.com/questions/5791/why-is-it-important-that-phin-is-kept-a-secret-in-rsa)

---
# Diffie-Hellman

Key-exchange protocols, which makes it possible to exchange credential secret over a public channel. There are 3 related mahematical problems: DL, CDH, DDH.

> Discrete Logarithm problem: Find a interger x, such that g^x = y

> Computational Diffie-Hellman problem: Given a cyclic group G with generator g, order m. Given g^a, g^b, compute g^ab, where a, b are random numbers smaller than m.

> Decisioanl Diffie-Hellman problem:  Given a cyclic group G with generator g, order m. Given g^a, g^b, g^c, decide if c=ab, where a, b, c are random numbers smaller than m.

Computational complexity: DL>=CDH>=DDH

**Key-exchange**

1. Alice and Bob agree on the algorithm parameters p (group order) and g (group generater).
2. Alice and Bob generates their own secret indepently, naming a and b.
3. Alice computes g^a mod p to Bob, and Bob then computes (g^a)^b mod p
4. Bob computes g^b mod p to Alice, and Alice then computes (g^b)^a mod p

**Correctness** (g^a)^b mod p = (g^b)^a mod p

**Proof of DDH** ...

---
# ELGama