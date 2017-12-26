---
layout: post
title:  "第二代洋葱路由 --- Tor： 2nd generation Onion Router"
date:   2017-12-24 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

今天再开一个新坑，上一篇post提到了洋葱路由的开始，这篇文章我们介绍一下第二代洋葱路由Tor。有兴趣的可以看以下seed paper： [Tor: The Second-generation onion router][paper]

# **简单介绍Tor**

和第一代洋葱网络的结构相似，第二代洋葱网络Tor目的是提供匿名的基于TCP协议的应用（比如说网页浏览，实时通信QQ等等）Tor相比于第一代的主要优势就是优化了网络结构更新了一些安全的协议，并且在时延和匿名性中做到了平衡。Seed Paper中具体阐述了每一个的细微改动，在这篇博客里我们主要关注于Tor关于安全性和匿名性方面的改动。

# **Perfect Forward Secrecy**

我们先来回顾一下洋葱路由的私密通信模型

<img src="{{site.url}}{{site.baseurl}}/img/SimpleDiagram.png" alt="Drawing" style="width: 400px;"/>

第一代洋葱路由运用的密码学协议是public key encryption，这意味着 Node2 , Node3 , Node4 的 public key 和 private key 是预先生成，且固定。

容易受到**replay攻击**。

假设我们通过洋葱路由来进行简单的密码登录，上图的m就代表了用户的密码。首先我们要先假设Initiator的匿名代理，也就是Node1没有被入侵（如果被入侵密码是一定泄露的），如果Node2被入侵者侵入，入侵者就获得了一个被后续节点public key加密过的密码密文，因为有密码学机制保证，入侵者无论如何也无法将密码的密文还原成明文。但是入侵者控制Node2截取到密文并保存下来，等到这次通信结束后冒充用户将使Node2发送密码的密文，这时服务器就会认为入侵者是Node2从而建立与入侵者的通信。

我们可以认为第一代洋葱路由使每个通信方都与自己的前一个节点和后一个节点建立了私密通信，以此来建立Initiator和Responder之间的私密通信。上段证明了这种机制是有“漏洞”的，因为该机制的最终目的是建立Initiator和Responder之间的私密通信，而第一代并没有直接建立，而是用一种迭代的方法建立的。

在[Diffie-Hellman Key Exchange][Diffie-Hellman]出现之后，情况就发生了改变，

[paper]:http://ezredirector.is.ed.ac.uk/redirect?url=http://handle.dtic.mil/100.2/ADA465464
[Diffie-Hellman]:https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange