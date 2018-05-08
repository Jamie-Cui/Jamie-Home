---
layout: post
title:  "区块链帐号和密钥 --- Account and Keys"
date:   2018-4-21 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

# **本章内容**

1. Sharing responsibility for accounts and keys
    - multi-sig transacions in bitcoin
    - secret-sharing
    - sharing a secret-key
    - publicly verifiable secret-sharing
2. Secure multiparty computation
3. Fair swap of values and fair MPC

# **比特币的多签名转账(MultiSig Transactions)**

现实中存在一种组织公有的“公有帐号”，比如公司的帐号。这类公有帐号在转账时需要多方签名实现，这就是比特币中多签名转账的由来。下面为m-out-of-n 多签名转账模型：

`<OP_m> <A1 pubkey> <A2 pubkey> ... <A3 pubkey> <OP_n> <OP_CHECKMULTISIG>`

例：

- 2-out-of-2帐号：两人共有帐号，需要两个数字签名来完成转账
- 1-out-of-2帐号：两人共有帐号，值需要一个数字签名来完成转账
- 2-out-of-3帐号：三个人共有帐号，需要其中任意两人数字签名来完成转账

<!-- **Sharing Secret-Key**

比特币使用了共享私钥。 -->

**Finite Cyclic Group**

一个Cyclic Group G定义如下：<img src="http://chart.googleapis.com/chart?cht=tx&chl= G= <g^0,g^1,\quad...\quad,g^q-1>" style="border:none;">， 其中q是G的order，g是G的generator。

在G中，<img src="http://chart.googleapis.com/chart?cht=tx&chl= g^q=1" style="border:none;">， <img src="http://chart.googleapis.com/chart?cht=tx&chl= g^x=g^{x\quad mod\quad q}" style="border:none;">

求公因子算法：<img src="http://chart.googleapis.com/chart?cht=tx&chl= gcd(q,y)=1}" style="border:none;">。剩下的都是密码学概念。

**DSA数字签名**

假设H为hash方程，y是public key，x是private key，y = g^x。注意在下面的算法中m是信息，(r,s)为数字签名。

<img src="{{site.url}}{{site.baseurl}}/img/DSA.png" alt="Drawing" style="width: 500px;"/>

Consistency: <img src="http://chart.googleapis.com/chart?cht=tx&chl= g^{u1} y^{u2} = g^{H(m)w}y^{rw} = g^{(H(m)\quad plus\quad xr)s^{-1}} = g^k}" style="border:none;">

**Secret Sharing**

假设现在有N个随机变量使得<img src="http://chart.googleapis.com/chart?cht=tx&chl= \sum_{i=1}^{N} x_i = x" style="border:none;">，这叫做Secret-sharing。

在Secret-sharing中只知道N-1个值并不能知道x的值，因此这个机制保证了只有同时具有N个随机变量才可以Retrive x。那么如何share secret key呢？

我们首先假设在n个人中share secret key(必须所有人都签名)，每个人的secret key为xi

<img src="http://chart.googleapis.com/chart?cht=tx&chl= \sum_{i=1}^{N} x_i = x\quad mod\quad q" style="border:none;">

相应的总public key也变为了之前的和。






