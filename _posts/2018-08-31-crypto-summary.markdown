---
layout: page
title:  "Summary of Modern Cryptography"
date:   2018-6-7 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}
---
# Before the beginning...

Cryptography can be divided into three catogories: ***Classic Cryptography***, ***Symmetric Cryptography*** and ***Asymmetric Cryptography (or Public Key Cryptography)***. And in the following part, this post will disscuss modern cryptography: symmetric cryptography and asymmetric crytography.

Most of the asymmetric cryptography algorithms base on mathematical problems for security issue, they are ***Discrete Logarithm***, ***Integer Factorization*** and ***Elliptic Curve***. Those problems can not be solved in polynomial time and thus can be used to buid a "trapdoor" function, which only allows one-way calculation. They can also be considered as ***cryptographic primitives***, which are frequently used to build cryptographic protocols and computer security systems. (It also includes trapdoor hash function, authentication, symmetric key cryptography, public key cryptography, digital signatures, mix network, commitment scheme etc.) 

From a more abstract perspective, the mathematical problems can be summarized as the ***"NP"*** problem, which is the findamental open problems in computer science. NP problem relates to two classes P and NP. P is the set of problems that can be solved in polynomial time and NP is the set of problems can be verified in polynomial time. Despite the significant effort from researchers, it is still unknown if P!=NP. However, ***many proofs of security would imply P!=NP***.

> "Find factors, get money" - Notorious T.K.G.

As for ***cryptographic protocols***, they are abstract or concrete protocols that performs a security-related function and applies cryptographic methods, often as sequences of cryptographic primitives. 

Cryptographic protocols can sometimes be verified formally on an abstract level. When it is done, there is a necessity to formalize the environment in which the protocol operate in order to identify threats. This is frequently done through the Dolev-Yao (姚期智) model.

---
# Discrect Logarithm

There are 3 related mahematical problems: DL, CDH, DDH.

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

有人评价椭圆曲线是“一群大神们苦心搜索，终于找到一个诡异的加法运算”。其实所有密码学问题的核心都是找到哪个trapdoor，并且保证trapdoor的安全，目前有DL, Intefer factorization and Elliptic Curve。下图给出了一个椭圆曲线的例子以便于更好的阐述椭圆曲线trapdoor的原理。

<img src="{{site.url}}{{site.baseurl}}/img/EC.jpg" alt="Drawing" style="width: 200px;"/>　

<!-- 
上图为方程：y2 = x3–x的曲线。参考资料：[沃通SM2椭圆曲线](https://www.wosign.com/SM2/SM2.htm)

1. P点为基点;
2. 通过P点做切线，交与点 2P点，在2P’点做竖线，交与2P点，2P点即为P点的2倍点;
3. 进一步，P点和2P点之间做直线，交与3P’点，在3P’点做竖线，交与3P点，3P点即为P点的3倍点;
4. 同理，可以计算出P点的4、5、6、… 倍点;
5. 如果给定图上Q点是P的一个倍点，请问Q是P的几倍点呢?
6. 直观上理解，正向计算一个倍点是容易的，反向计算一个点是P的几倍点则困难的多。

在椭圆曲线算法中，将倍数d做为私钥，将Q做为公钥。当然，椭圆曲线算法还有更严格的计算过程，相对图示要复杂的多。 -->


- ECDH
- ECDSA


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
