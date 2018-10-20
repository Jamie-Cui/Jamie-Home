---
layout: page
title:  "Summary of Modern Cryptography"
date:   2018-6-7 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

- [Before the beginning...](#before-the-begining)
- [Terminology](#terminology)
- [Discrect Logarithm](#discrect-logarithm)
- [Elliptic Curve](#elliptic-curve)
- [Symmetric Cryptographic Algorithms](#symmetric-cryptographic-algorithms)
- [Asymmetric Cryptographic Algorithms](#asymmetric-cryptographic-algorithms)

# Before the beginning

Cryptography can be divided into three catogories: *Classic Cryptography*, *Symmetric Cryptography* and *Asymmetric Cryptography (or Public Key Cryptography)*. And in the following part, this post will disscuss modern cryptography: symmetric cryptography and asymmetric crytography.

Most of the asymmetric cryptography algorithms base on mathematical problems for security issue, they are ***Discrete Logarithm***, ***Integer Factorization*** and ***Elliptic Curve***. Those problems can not be solved in polynomial time and thus can be used to buid a "trapdoor" function, which only allows one-way calculation. They can also be considered as *cryptographic primitives*, which are frequently used to build cryptographic protocols and computer security systems. (It also includes trapdoor hash function, authentication, symmetric key cryptography, public key cryptography, digital signatures, mix network, commitment scheme etc.) 

From a more abstract perspective, the mathematical problems can be summarized as the ***"NP"*** problem, which is the findamental open problems in computer science. NP problem relates to two classes P and NP. P is the set of problems that can be solved in polynomial time and NP is the set of problems can be verified in polynomial time. Despite the significant effort from researchers, it is still unknown if P!=NP. However, ***many proofs of security would imply P!=NP***.

> "Find factors, get money" - Notorious T.K.G.

As for *cryptographic protocols*, they are abstract or concrete protocols that performs a security-related function and applies cryptographic methods, often as sequences of cryptographic primitives. 

Cryptographic protocols can sometimes be verified formally on an abstract level. When it is done, there is a necessity to formalize the environment in which the protocol operate in order to identify threats. This is frequently done through the Dolev-Yao (姚期智) model.

# Terminology

**Encryption and Decryption** (protecting confidenntiality) A message is *plaintext*. The process of disguising a message in such a way as to hide its substance is encryption. An encrypted message is *ciphertext*. The process of turning ciphertext back into plaintext is decryption. The encryption process is *Enc(plaintext) = ciphertext*; Decryption process is *Dec(ciphertext) = plaintext*; The *correctness* of Encryption and Decryption is *Dec(Enc(plaintext)) = plaintext*.

**Authentication, Integrity and Nonrepudiation** Other than confidentiality, social interactions also require cryptography to provide other functions such as authentication, integrity and nonrepudication. Related protocols are ***Message Authentication Code (MAC)***, which is a samll piece of informtion used to autenticate a message, protecting both authentication and integrity.

**Algorithms and Keys** A cryptographic algorithm, also called a cipher, is the mathematical function used for encryption and decryption (Usually there are encryption function and decryption function for one cipher). Sometimes to encrypt or decrypt, an algorithm requires an addional infromation, which is denoted as the keys (encryption key and decryption key). Encryption key is used to encrypt messages while decryption key is used to decrypt the corresponding ciphertext (Remeber they appear in pairs). A cryptosystem is an algorithm, plus all possible plain texts, cipher texts, and keys.

**Symmetric Algorithms**, sometimes called conventional algorithms, are algorithms where the encryption key can be calculated from the decryption key and vice versa (usually public key = private key). Symmetric algorithms can be divided into two catogories: ***Stream cipher*** and ***Block cipher***.

**Public-Key Algorithms** (also called asymmetric algorithms) are designed so that the key used for encryption is different from the key used for decryption. Furthermore, the decryption key cannot (at least in any reasonable amount of time) be calculated from the encryption key.

**Cryptanalysis** is the science of recovering the plaintext of a message without access to the key. Successful cryptanalysis may recover the plaintext or the key. It also may find weaknesses in a cryptosystem that eventually lead to the previous results. There are four general types of cryptanalytic attacks. Of course, each of them assumes that the cryptanalyst has complete knowledge of the encryption algorithm used. ***Ciphertext-only attack***: Cryptanalysis has the ciphertext of several messages using the same encryption secheme and same key, the objective is to recover the plaintext message. ***Known-plaintext attack***: The cryptanalyst has access not only to the ciphertext of several messages, but also to the plaintext of those messages, objective is to get the encrypt key or build a decrypt function decrypting the future ciphertexts. ***Chosen-plaintext attack***: The cryptanalyst not only has access to the ciphertext and associated plaintext for several messages, but he also chooses the plaintext that gets encrypted, the objective is to deduce the key. ***Adaptive-chosen-plaintext attack***: Not only can the cryptanalyst choose the plaintext that is encrypted, but he can also modify his choice based on the results of previous encryption.

---

# Discrect Logarithm

Discrete logarithms are quickly computable in a few special cases. However, no efficient method is known for computing them in general. Several important algorithms in public-key cryptography base their security on the assumption that the discrete logarithm problem over carefully chosen groups has no efficient solution. There are 3 related mahematical problems: DL, CDH, DDH.

**Discrete Logarithm (DL)**: Find a interger x, such that g^x = y. There is no proof that this problem is hard, to the best of our knowledge, the number of steps necessary to find a solution is super-polynomial in the size of the group element, assuming the group is chosen appropriately.

**Computational Diffie-Hellman (CDH)**: Given a cyclic group G with generator g, order m. Given g^a, g^b, compute g^ab, where a, b are random numbers smaller than m. It s clear that if an advarsary could solve DL, he could also solve CDH with a single exponentiation. This would imply that CDH <= DL

**Decisioanl Diffie-Hellman (DDH)**:  Given a cyclic group G with generator g, order m. Given g^a, g^b, g^c, decide if c=ab, where a, b, c are random numbers smaller than m.

Computational complexity: DL>=CDH>=DDH

# Elliptic Curve

EC is one of the most difficult cryptographic algorithms to understand, so the following part may contain Chinese for me to have a better understanding.

- Encryption Decryption
- Key exchange (ECDH)
- Digital Signature (ECDSA)

> **Definition**: In a finite filed F, which is not of characteristic 2 or 3, the elliptic curve is defined by: Y^2=X^3+aX+b, where a,b belong to F.

As an additive group, any two points on a curve add to produce a third point on the curve (due to its property). Notice this is not a vector addition. On the elliptic curve, adding points p1 and p2 is viewed as projecing a line between p1 and p2 to form the point of joint with the curve. The joining point is the resulting point p3.

换句话说，在椭圆曲线中任意选取曲线上的两个点(p1, p2)作延长线，一定有另一个与曲线相连的交点(p3)，除非该线与x轴垂直。我们将这个操作成为“相加” (p1+p2=p3)。利用这个性质，我们定义了一个点的“逆”：如果存在一个点p(x,y)，那么它的逆就是(x,-y)。这是因为椭圆曲线是根据x轴完全对称的。再者我们定义0为椭圆曲线的无穷远，因此对于任意三个在椭圆曲线上的点来说 p1+p2+p3=0 是一定成立的(如果不能组成一条线也算作成立)。

有人评价椭圆曲线是“一群大神们苦心搜索，终于找到一个诡异的加法运算”。其实所有密码学问题的核心都是找到哪个trapdoor，并且保证trapdoor的安全，目前有DL, Intefer factorization and Elliptic Curve。下图给出了一个椭圆曲线的例子以便于更好的阐述椭圆曲线trapdoor的原理。

<img src="{{site.url}}{{site.baseurl}}/img/EC.jpg" alt="Drawing" style="width: 200px;"/>　


上图为方程：y2 = x3–x的曲线。参考资料：[沃通SM2椭圆曲线](https://www.wosign.com/SM2/SM2.htm)

1. P点为基点;
2. 通过P点做切线，交与点 2P点，在2P’点做竖线，交与2P点，2P点即为P点的2倍点;
3. 进一步，P点和2P点之间做直线，交与3P’点，在3P’点做竖线，交与3P点，3P点即为P点的3倍点;
4. 同理，可以计算出P点的4、5、6、… 倍点;
5. 如果给定图上Q点是P的一个倍点，请问Q是P的几倍点呢?
6. 直观上理解，正向计算一个倍点是容易的，反向计算一个点是P的几倍点则困难的多。

在椭圆曲线算法中，将倍数d做为私钥，将Q做为公钥。当然，椭圆曲线算法还有更严格的计算过程，相对图示要复杂的多。

- ECDH
- ECDSA

# Lattice-based cryptography

Lattice-based cryptography is the general form of constructions of cryptographic primitives that involves lattice, either in the construction itself or in the security proof. Lattice based cryptography is a important candicate of post-quantum cryptography, which may still remains secure on the presence of quantum computer.

> Definition of Lattice: A lattice L belongs to n-dimention real numebr space is the set of all integers liner combination of basis vector b1, b2 .... bn.

The most crucial problem in lattice based cryptography is [Shortest Vector Problem(SVP)](https://en.wikipedia.org/wiki/Lattice_problem)


# Symmetric Cryptographic Algorithms

Before the secure communication between parties, the encryption key must be exchanged beforehand. Generally the exchange process can be competed by public key cryptography, which will be talked about in the next section.

The advantages for symmetric cryptographuc algrothms is simple and fast, because they ususally depends on XOR or bit operations, which makes it the perfect match for long-time encryption.

1. Stream Cipher (see [Blog about Stream Cipher]({{site.url}}{{site.baseurl}}/crypto-stream.html))
- OTP
- RC4
- WEP
- LFSR
- CSS using LFSR

2. Block Cipher (see [Blog about Block Cipher]({{site.url}}{{site.baseurl}}/crypto-block.html))
- DES, key size: 56.
- 2DES, key size: 112 .
- 3DES, key size: 168.
- AES, block size: 128; key size: 128, 192, 256.

# Asymmetric Cryptographic Algorithms

<!-- 
---
# Zero Knowledge Proof

---
# Mix net

---
# Blind Signature

---
# Distributing trust

1. Secret Sharing
2. Shamir’s Secret Sharing Scheme
3. Distributing Decryption Capabilities
4. Publicly Verifiable Secret Sharing
5. Distributing the Dealer

---
# Private information retrival

# Homomorphic Encryption

In general: Homomorphic encryption is a form of encryption that allows computation on ciphertexts, generating an encrypted result which, when decrypted, matches the result of the operations as if they had been performed on the plaintext. [wiki](https://en.wikipedia.org/wiki/Homomorphic_encryption)

**Key Feature: enables computation over encrypted data**

1. linear-homomorphic

- Goldwasser-Micali cryptosystem
- Pailler's cryptosytem -->
