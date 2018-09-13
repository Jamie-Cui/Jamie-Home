---
layout: post
title:  "块加密 --- Block Cipher"
date:   2018-5-07 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>

百度百科竟然都没有块加密这种东西。。。块加密是一种对称加密算法(使用相同的key进行加密解密)，我们假设k是密钥的长度，l是需要加密的明文的长度，那么：

- 加密E：{0,1}^k * {0,1}^l -> {0,1}^l
- 解密D：{0,1}^k * {0,1}^l -> {0,1}^l

和流加密不同，块加密并不需要密钥和明文具有同样的长度。

# DES

DES，全称(Data Encryption Standard)数据加密标准，是一种块加密的标准，在ATM机上被广泛应用。下面是DES的加密过程：用56比特的密钥生成16个48比特的块密钥，之后将64比特的明文输入。

<img src="{{site.url}}{{site.baseurl}}/img/DES-encryption.png" alt="Drawing" style="width: 700px;"/>

解密过程是完全的反过程：

<img src="{{site.url}}{{site.baseurl}}/img/DES-decryption.png" alt="Drawing" style="width: 700px;"/>

那么我们来分析DES的安全性，首先因为DES的密钥空间为56比特，所以寻找到正确的密钥最多需要2^56计算，基于目前的正常算力来说需要七天。再者，在实践中发现通过仿射近似，DES密钥中的14个可以在2^42复杂度内计算出来，剩下的密钥空间也只剩下2^42复杂度，因此大大节省了攻击所需要的时间，最终完成攻击所需要的时间约为2^43。

# 3DES-2DES

因此我们有了3DES，3DES的目标是阻止暴力破解，经常被用在银行卡和RFID芯片上，算法如下：

- E_3DES((K1,K2,K3),M) = E_DES(K1,E_DES(K2,E_DES(K3,M)))
- D_3DES((K1,K2,K3),C) = D_DES(K3,D_DES(K2,D_DES(K1,C)))
- 密钥长度 3*56 = 168 bits

同时，因为3DES涉及到的运算是DES的3倍，虽然它保证了加密不会受到暴力破解，但是缺导致了运行缓慢。因此3DES的主要应用是需要绝对安全，不注重效率(ATM就是一个很好的例子)。

因为3DES具有2^168的密钥空间，已经远远超过目前计算机的算力了，为什么不用2DES？2DES也同样具有2^112的密钥空间，同样远超目前计算机的算力。

和3DES类似，2DES算法：

- E_2DES((K1,K2),M) = E_DES(K1,E_DES(K2,M)))
- D_2DES((K1,K2),C) = D_DES(K2,D_DES(K1,C))

<img src="{{site.url}}{{site.baseurl}}/img/2DES.png" alt="Drawing" style="width: 500px;"/>

2DES容易收到**meet-in-the-middle-attack**，从而使破解2DES的难度从2^112减少到2^63。具体攻击方法为：首先针对第一个DES对于m和所有key空间的密钥计算出第一个DES结果的全部2^56可能性;同样我们针对第二个DES从后向前计算出第二个DES输入的2^56可能性，然后分别将这两种DES从小到大排序，复杂度为log(2^56)。因此破解的复杂度为：2^56*log(2^56)*2 < 2^63。因此2DES是不安全的。

# AES

因为DES各类的种种缺点(3DES太慢，2DES和DES不安全)，人们发明了AES(进阶加密标准)。

- 块大小128比特
- 密钥长度128,192,256比特

<img src="{{site.url}}{{site.baseurl}}/img/AES.png" alt="Drawing" style="width: 700px;"/>

其中m是4×4位的矩阵。我们将每轮的AES操作分为以下几个步骤：

1. AddRoundKey： 初始矩阵中的每个元素都和输入的key尽行XOR运算
2. SubBytes：建立当前矩阵中查询元素所需要的查询表
3. ShiftRows：将矩阵的行列进行循环移位
4. MixColumns：每行元素都和一个固定的的多项式做乘法

针对AES的攻击有

- related-key attack
- key-recovery attack

# Padding

我们可以看到上面的各种加密算法都要求明文长度固定，那么，如果我们有一个长度较短的明文怎么办？

- bit padding： 在明文后先添加一个字节的1,其余字节全部填充0
- ANSI X.923：在明文之后填充0,最后一个字节写入填充的位数
- PKCS 7：填充第一位为1,之后以此类推

## ECB: Electronic Code Book 模式

The simplest of the encryption modes is the Electronic Codebook (ECB) mode (named after conventional physical codebooks[10]). The message is divided into blocks, and each block is encrypted separately.

- 首先将输入的任意长度的M做padding让m的长度能整除可接受长度
- 将m分割成可接受长度的块
- 对于每个块进行加密得到密文
- 将密文合在一起

缺点：不适用图像加密，因为没有很好的隐藏掉空间信息

## CBC: Cipher-block chaining

 In CBC mode, each block of plaintext is XORed with the previous ciphertext block before being encrypted. This way, each ciphertext block depends on all plaintext blocks processed up to that point. To make each message unique, an initialization vector must be used in the first block.

## CFB: Cipher Feedback

The Cipher Feedback (CFB) mode, a close relative of CBC, makes a block cipher into a self-synchronizing stream cipher. Operation is very similar; in particular, CFB decryption is almost identical to CBC encryption performed in reverse:

## OFB: Output Feedback

The Output Feedback (OFB) mode makes a block cipher into a synchronous stream cipher. It generates keystream blocks, which are then XORed with the plaintext blocks to get the ciphertext. Just as with other stream ciphers, flipping a bit in the ciphertext produces a flipped bit in the plaintext at the same location. This property allows many error correcting codes to function normally even when applied before encryption.

## CTR: Counter (CTR)

Like OFB, Counter mode turns a block cipher into a stream cipher. It generates the next keystream block by encrypting successive values of a "counter". The counter can be any function which produces a sequence which is guaranteed not to repeat for a long time, although an actual increment-by-one counter is the simplest and most popular. The usage of a simple deterministic input function used to be controversial; critics argued that "deliberately exposing a cryptosystem to a known systematic input represents an unnecessary risk."[19] However, today CTR mode is widely accepted and any problems are considered a weakness of the underlying block cipher, which is expected to be secure regardless of systemic bias in its input.[20] Along with CBC, CTR mode is one of two block cipher modes recommended by Niels Ferguson and Bruce Schneier.