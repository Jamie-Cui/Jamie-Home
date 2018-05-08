---
layout: post
title:  "密码学基础 --- Cryptography"
date:   2018-5-08 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

# Hash方程

首先我们需要知道有两种密码学相关的定义：

- **One-Way-Functions** A function f is a one-way functon if for all y there is no efficient algorithm which can compute x shch that f(x)=y

- **Collision-Resistent-Functions** A function f is collision resistant if there is no efficient algorithm that ca find two messages m1 and m2 such that f(m1)=f(m2)

而真正的密码学中Hash定义如下： A cryptographic hash function h: M -> T is a function that satisfies the following 4 properties:

- M >> T
- It is easy to compute the hash value for any given message
- It is hard to retrieve a message from its hashed value (OWF)
- It is hard to find two different messages with the same hash value (CRF)

那么在什么情况下我们会用到hash呢？

- Commiement
- 检查文件完整性
- 密码校验
- 生成新密钥
- 某些加密协议

# 生日悖论

生日悖论是一个著名的密码学问题，有利于理解collision-resistence。假设我们现在有一个屋子，我们需要多少人才能保证在这件屋子里选到两个人生日相同的人的概率大于百分之五十？大多数人会认为这个数字会相当的大，实际上并不是的。

**证明**

```
我们假设屋子里面有x个人可以保证我们结果成立，那么，我们有
Prob[至少有两个人同一天生日] = 1-Prob[所有人生日不同] > 50%
Prob[所有人生日不同] = 1 * 364/365 * 363/365 * ... * (365-x)/365
我们得到一个不等式：1 - 1 * 364/365 * 363/365 * ... * (365-x)/365 > 50%
之后通过数学计算我们可以得出结论 x > 22，也就是只需要23个人就可以成功。
```

放到密码学中我们就可以认为这个模型可以应用到破解hash的collision，条件也就是以50%几率找到collision所需要进行尝试的次数。因此来检查hash的优劣时我们可以利用生日悖论攻击。

# The Merkel-Damgard construction

<img src="{{site.url}}{{site.baseurl}}/img/merkle-damgard.png" alt="Drawing" style="width: 500px;"/>

和块加密结构类似，常用于文件校验的hash算法。

# MACs

MAC全名为Message Authentication Codes，MAC在发送信息时会将hash信息放一个tag里面这样接收者就可以辨认文件是否在传输过程中发生了变化。

<img src="{{site.url}}{{site.baseurl}}/img/MAC.png" alt="Drawing" style="width: 500px;"/>

# Public Key Encryption

1. Key generation: G -> pk,sk
2. Encryption: E(m,sk)=c
3. Decryption:  D(c,pk)=m

下面是数论的一些概念

1. 互为质数：当a和b没有公因数的时候我们就称a、b互为质数。gcd(a,b)=1

# DH key exchange

对称密钥交换

# RSA

- G_RSA = (pk.sk), pk = (N,e), sk = (N,d), e*d = 1(mod N)
- E(pk,m) = m^e (mod N)
- D(sk,c) = c^d (mod N)

# EIGamal

- G = (pk,sk), pk = g^d (mod p), sk = d <- {1,...p-2}
- (e,c) = E(pk,m) = (g^r(mod p), (g^d)^r(mod p)), r <- Z
- D(sk,c) = e^(-e)c(mod p)

# Logical attack

<img src="{{site.url}}{{site.baseurl}}/img/logical-attack.png" alt="Drawing" style="width: 500px;"/>

<img src="{{site.url}}{{site.baseurl}}/img/NSPK.png" alt="Drawing" style="width: 500px;"/>

<img src="{{site.url}}{{site.baseurl}}/img/nspk-attack.png" alt="Drawing" style="width: 500px;"/>