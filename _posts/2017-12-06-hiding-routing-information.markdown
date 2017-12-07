---
layout: post
title:  "洋葱路由的起点 --- Hiding Routing Information"
date:   2017-12-06 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

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

接下来我们介绍为什么洋葱网络能做到让每个节点都只能知道前一个节点的信息和后一个节点的信息。我们使用密码学中介绍过得加密和解密方程（简单的RSA加密解密方程），首先每个节点（看上图）Node 1,2,3,4,5 都有一个自己的Secret Key（用来解密）和对应的Public Key（用来加密）。

*我们首先从安全的角度来考虑，如果入侵者进行网络分析去寻找相同的message就一定可以找到该message经过的每个路径，从而得到不仅该信息的通信双方身份信息，更能了解到通过的每个节点的身份信息，这是我们不想看到的。*

幸运的是利用密码学中的加密和解密，我们可以让每个节点受到和发出的信息都不同。如上图 Node 1 在收到 Initiator 发送的信息 m 之后，会随机建立一条匿名网络之内的信息传播路径，ps. 匿名网络中的所有节点都有下面者一个表格，里面存储了该匿名网络中所有节点以其对应的public key。

接下来Node 1 会生成加密信息C ：
`Enc( Enc( Enc( Enc(m, pk5), pk4), pk3), pk2)`

Node 2 发送出去的加密信息为：
`Enc( Enc( Enc(m, pk5), pk4), pk3)`

以此类推......

Node 5 发送出去的加密信息为：
`m`

| Node No. | Public Key |
|----------|------------|
| Node 1   | pk1        |
| Node 2   | pk2        |
| Node 3   | pk3        |
| Node 4   | pk4        | 
| Node 5   | pk5        |
|          |            |

*这样我们就解决了各个节点传输信息相同的问题。接下来我们来看一下个节点Forward的包的格式是怎么样的，在其中又考虑了哪些安全的因素。*

右面展示了每个节点Forward的包的格式：
<img src="{{site.url}}{{site.baseurl}}/img/ForwardPackage.png" alt="Drawing" style="width: 350px;"/>

* `exp_time`：包的有效期，包的有效期到了之后node会drop掉这个包
* `next_hop`：下一个节点的信息
* `(Ff,Kfz)`：存储了下一个节点的Forward message 和下一个节点的 public key
* `(Fb,Kby)`：存储了上一个节点恶毒Forward message 和上一个节点的 public key

下面这张图具体解释了 X -> Y -> Z 是如何向下一个节点传递包的。首先因为X是第一个节点，所以X知道所有节点的信息，因此在利用Y和Z的public key对原明文信息message进行加密的时候，向每一层填入了上一个节点和下一个节点的信息（因为通信不是一次性单向的而是多次双向的通信）。这样就保证了整条通信线路的完整性和保密性（上一个节点下一个节点的信息都是通过该节点的public key进行加密过的）。

<img src="{{site.url}}{{site.baseurl}}/img/Forward Onion.png" alt="Drawing" style="width: 400px;"/>

*我们能注意到最后一个包也就是Node5解密后的包是有一段padding的，原因是通过加入一串随机长度的字符可以让入侵者无法真正知道网络中的哪个包是最后一次传输用的包，因为如果不加padding的话最后一个包的长度就很固定，容易被发觉从而判断出通信一方。*

该信息每经过一个节点都可以解密一层，因为只有对应的secret key才可以解密，所以就保证了只有这个节点才可以解密，每次解密后的传播会使传播的包想洋葱一样脱掉一层“外皮”（实际上是加密层），这就是“洋葱网络”的由来。

# **创建通信流**



[Hiding-routing-information]:https://link-springer-com.ezproxy.is.ed.ac.uk/chapter/10.1007/3-540-61996-8_37
[OSI]:https://baike.baidu.com/item/OSI/5520?fr=aladdin
[Chaum]:https://dl-acm-org.ezproxy.is.ed.ac.uk/citation.cfm?doid=358549.358563s