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
7. Hash functions, including OWF, Collision resistent function

Cryptographic primitives:

- Hash
- Authentication
- Symmetric Key cryptography
- Public Key cryptography (Asymmetric)
- Digital Signature (identification)
- Mix network (for privacy)
- Commitment scheme (related to ZKP)

Mathematical Problems:

- Integer factorization
- Discrete Logarithm
- Elliptic Curve

---
# Hash

Security game:
- Hiding
- Binding

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

1. RSA
3. EIGamal, IND-CPA resistent

Security Game:

- AON-CPA (All-or-nothing under chosen plaintext attack) --- encryption
- IND-CPA (indistinguishability chosen plaintext attack) --- encryption
- UF-CMA (Unforgeability under a chosen message attack) --- digital signature
- EU-CMA (Existential Unforgeability under a Chosen Message Attack) --- digital signature

---
# RSA

RSA cryptosystem is developed in 1977 by Rivest, Shamir and Adleman. It was the first public-key encryption scheme which could both sign and encrypt messages. As with the Diffie-Hellman key exchange protocol, the RSA cryptosystem enables two parties to communicate over a public channel.

**For Encryption** 

Suppose Alice wants to send Bob a secure message over an insecure public channel.

- Bob selects two distinct large prime number *p* and *q*, and compute *n = p\*q*.
- Bob randomly chooses an integer *e* *(1 < e < <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;">)*, then find *d* which satisfies *e\*d mod <img src="http://chart.googleapis.com/chart?cht=tx&chl=\varphi (n)" style="border:none;"> = 1*.
- Encrypt message m: <img src="http://chart.googleapis.com/chart?cht=tx&chl= y=m^e\quad mod\quad n" style="border:none;">, send *y* to Bob.
- Decrypt cipher y: <img src="http://chart.googleapis.com/chart?cht=tx&chl= m=y^d" style="border:none;">

Nonsensitive data: n, e; Confidential data: p, q, toient function of n, d. The security parameter is the serilised number of key, which equals to the serilised phi(n).

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

**My thought** The security of RSA encryption bases on both *DL* and *integer factorization*. For DL, it makes it impossible to calculated message from cipher under PPT. For integer factorization, it is impossible to calculated the private key by taking advatage of the public key (unless the factorization of encryption modular n is known).

---
# ELGamal

Based on DH Key exchange, and also proved secure in IND-CPA (Indistinguishble Chosen-plaintext attack) model.

**Encryption** Suppose Alice want to establish a secure communication channel with Bob.

1. Alice generates parameters, share them publicly: (G, q, g), in which G is the cyclic group, q is group order(large prime) and g is group generator.
2. Alice choose a random x (private key) from (1, q-1), and computes his public key h=g^x.
3. Bob choose a random y from (1, q-1) and calculates c1=g^y.
4. Bob calculates shared secret s=h^y and then calculates c2=m*s. (m is the message)
5. Bob send (c1, c2) to Alice
6. Alice calculates shared secret s=c1^x. And then computes m=c2*s^-1.

**Correctness**: Simple and obvious.

**Frustrating IND-CPA (while RSA fails)**

> IND-CPA: An advarsary is allowed to sumit two plaintext messages to the encryption oracle, which retruns an encryption on one of the two plaintexts at random. The advarsary must then discern which of the two plaintexts was returned. The advarsary goal IND stands for indistinguishability.

Why RSA failes? Suppose we choose two messages m0=0 m1=1, and encrypt them using RSA we have c0=m0^e mod n, c1=m1^e mod n. Clearly c0=0 and c1=1, which means if the output is 1 we can confirm it's c1 or otherwise c0. The advarsary can disinguish the output with 100% possibility. In fact, all deterministic public-key incryption schemes fail IND-CPA.

How about ElGamal? Simialrly we choose two messages m0 and m1, (c0-0 \|\| c0-1)=(g^y \|\| m0^s) and (c1-0 \|\| c1-1)=(g^y \|\| m1^s), we may notice there is a random value involved in the output cipher which makes the output differs everytime even if with the same encryption key and imput message. Also we can use mathematical method to prove the possibility of advarsary win IND-CPA game is 50%, which is euivalant to random guess.

---
# Diffie-Hellman (Discrect Logarithm)

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

**[TODO] Proof of the security DDH**

# Elliptic Curve

EC is one of the most difficult cryptographic algorithms to understand, so the following part may contain Chinese for me to have a better understanding.

- Encryption Decryption
- Key exchange (ECDH)
- Digital Signature (ECDSA)

> **Definition**: In a finite filed F, which is not of characteristic 2 or 3, the elliptic curve is defined by: Y^2=X^3+aX+b, where a,b belong to F.

As an additive group, any two points on a curve add to produce a third point on the curve (due to its property). Notice this is not a vector addition. On the elliptic curve, adding points p1 and p2 is viewed as projecing a line between p1 and p2 to form the point of joint with the curve. The joining point is the resulting point p3. 

换句话说，在椭圆曲线中任意选取曲线上的两个点(p1, p2)作延长线，一定有另一个与曲线相连的交点(p3)，除非该线与x轴垂直。我们将这个操作成为“相加” (p1+p2=p3)。利用这个性质，我们定义了一个点的“逆”：如果存在一个点p(x,y)，那么它的逆就是(x,-y)。这是因为椭圆曲线是根据x轴完全对称的。再者我们定义0为椭圆曲线的无穷远，因此对于任意三个在椭圆曲线上的点来说 p1+p2+p3=0 是一定成立的(如果不能组成一条线也算作成立)。



# TLS