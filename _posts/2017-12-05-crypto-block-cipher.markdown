---
layout: post
title:  "块加密"
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
- D_3DES((K1,K2,K3),M) = D_DES(K3,D_DES(K2,D_DES(K1,C)))
- 密钥长度 3*56 = 168 bits

同时，因为3DES涉及到的运算是DES的3倍，虽然它保证了加密不会受到暴力破解，但是缺导致了运行缓慢。因此3DES的主要应用是需要绝对安全，不注重效率(ATM就是一个很好的例子)。

因为3DES具有2^168的密钥空间，已经远远超过目前计算机的算力了，为什么不用2DES？2DES也同样具有2^112的密钥空间，同样远超目前计算机的算力。
