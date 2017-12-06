---
layout: post
title:  "洋葱路由的起点 --- Hiding Routing Information"
date:   2017-12-06 12:00:00 +0000
categories: jekyll update
---
这篇post我们会提及一下洋葱路由的发展过程，尤其是在刚提出匿名网络时洋葱网络的初始化设计方案。
这篇post是基于这篇论文：[Hiding routing information (Goldschlag, D.M. ; Reed, M.G. ; Syverson, P.F.)][Hiding-routing-information]

废话不多说我们来开始介绍洋葱路由的第一个版本（之后均简称做洋葱路由）。

# **简单介绍** 

洋葱路由是一种搭建在现有网络之上的一种高层次网络(Overlay Network)，他为建立连接的双方提供了实时(real-time)的双向(bi-directional)匿名(anonymous)服务。洋葱路由的目的是防范攻击者对网络流量的嗅探分析，也就是我们常说的被动入侵者(Passive attacker)，洋葱网络的特性保证了网络中每个节点只会知道下一个节点和上一个节点的身份信息，因此进行网络嗅探的入侵者不会找到整条完整的通信链，进而保证了通信双方的身份保密。

洋葱网络可以应用于匿名的的代理连接，因为洋葱网络是在[OSI七层模型][OSI]中的应用层定义的，洋葱网络对应用层的许多协议进行了适配以应用到代理服务器之上。在论文中作者的模型是在HTTP代理服务器上应用了洋葱路由，这里只是指出了洋葱路由的一些应用。

接下来让我们看一下洋葱网络到底长什么样子，Initiator是通信的发起方而responder是通信的回应方，Node 1是第一个节点也是用户连接到洋葱网络的入口，经过Node 1之后的线路是由Node 1来进行选择的，因此Node 1是最敏感也是最容易遭受到入侵的节点。但是即使Node 1 被入侵之后（也就是被入侵者控制），只要在洋葱网络中还存在一个诚实的节点，整个网络都是匿名的（但是之后有一些论文推翻了这种论断，在这篇文章中我们暂且认为这个是对的）。

<img src="{{site.url}}{{site.baseurl}}/img/OnionRouter.png" alt="Drawing" style="width: 600px;"/>

我们必须注意到洋葱网络并不是一个匿名网络，通信双方如果想知道对面的身份还是可以请求到的，洋葱网络只是将通信的socket包加密因此无法通过分析网络流量来得到通信双方的身份信息。

# **番外：其他的匿名网络**

Dr. Chaum 1981年发表了一篇论文[Untraceable Electronic Mail, Return Addresses and Digital Pseudonyms][Chaum]，记录了一种使用混淆器(mixers)使网络变成匿名的办法。

在密码学中和区块链我们也会见到混淆器的应用，使得追踪信息来源变得很难。但是混淆器是一个基于第三方的服务，我们要使用的话必须信任第三方的机构，因此洋葱网络（使用P2P）相比来说就更加可靠。再者混淆器依赖于将很多信息整合重新同时发出，因此网络延迟相当于P2P来说要高很多，而且越不Popular的第三方服务提供商延迟会越高。

# **洋葱**

接下来我们介绍为什么洋葱网络能做到让每个节点都只能知道前一个节点的信息和后一个节点的信息。我们使用密码学中介绍过得加密和解密方程，首先每个节点（看上图）Node 1,2,3,4,5 都有一个自己的Secret Key（用来解密）和对应的Public Key（用来加密）。

Node 1 在受到 Initiator 发送的信息 m 之后，会随机建立一条匿名网络之内的信息传播路径

| Node No. | Public Key |
|----------|------------|
| Node 1   | pk1        |
| Node 2   | pk2        |
| Node 3   | pk3        |
| Node 4   | pk4        | 
| Node 5   | pk5        |

Node 1从Initiator那里接收到信息m，


[Hiding-routing-information]:https://link-springer-com.ezproxy.is.ed.ac.uk/chapter/10.1007/3-540-61996-8_37
[OSI]:https://baike.baidu.com/item/OSI/5520?fr=aladdin
[Chaum]:https://dl-acm-org.ezproxy.is.ed.ac.uk/citation.cfm?doid=358549.358563s