---
layout: post
title:  "区块链和分布式总帐 --- Blockchains and Distributed Ledgers"
date:   2018-4-18 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

# **本章讲解内容**

1. 货币： 数字货币的起源
2. 分布式总帐概念
3. 什么是共识
4. 区块链的“块”
5. 区块链结构
6. 共识和分布式总帐的区别

以下名词介绍`内容来源：维基百科`

**区块链**（英语：blockchain 或 block chain）是用分布式数据库识别、传播和记载信息的智能化对等网络, 也称为价值互联网。中本聪在2008年，于《[比特币白皮书](https://bitcoin.org/bitcoin.pdf)》中提出“区块链”概念，并在2009年创立了比特币社会网络，开发出第一个区块，即“创世区块”。

区块链共享价值体系首先被众多的加密货币效仿，并在工作量证明上和算法上进行了改进，如采用权益证明和SCrypt算法。随后，区块链生态系统在全球不断进化，出现了首次代币发售ICO；智能合约区块链以太坊；“轻所有权、重使用权”的资产代币化共享经济；和区块链国家。

目前，人们正在利用这一共享价值体系，在各行各业开发去中心化电脑程序(Decentralized applications, Dapp)，在全球各地构建去中心化自主组织和去中心化自主社区(Decentralized autonomous society, DAS)

**分布式总帐** (Distributedd Legers)：
> Distributed Ledger is a consensus of replicated, shared, and synchronized digital data geographically spread across multiple sites, countries, or institutions

我们只需要了解到分布式总帐不止存在于区块链中，分布式总帐也具有别的形式。

# **货币**

什么是货币？货币毋庸置疑是日常生活不可或缺的一部分，在1874年一个人可以用一只鸡来换一年的报纸订阅。这项交易成立的条件是交易双方均认为一只鸡的在1874年的价值和一年报纸订阅的价值相同，双方建立了共识（consensus）机制来完成交易。经历过以物易物的阶段后，人们发现以物易物的方式效率很低，而且共识的建立必须是双方面对面确认了交易物品价值，因此黄金、白银作为了第一代的货币出现了。

我们这里拿出来货币的三种性质来对以下几种货币进行比较：
1. 一种交换的媒介。 --- A medium of exchange
2. 用来给物品或者服务标价。 --- A unit of amount
3. 价值的载体。--- A store of value

**第一代货币：实体货币（Money 1.0)** 黄金、白银、贝壳是过去所有人都 *承认* 的高价值物品。

货币本质是一种convention，其本身并不一定自身具有价值，货币之所以被称为货币是因为其被大多数人认为可以代表价值。因此在货币的定义期间，共识是十分重要的组成部分。下面我们来分析第一代货币的性质：
1. 作为交换的媒介 --- 一般，只能用于面对面交易
2. 用来给物品标价 --- 一般，可以被替代，不容易被分割价值，易于被仿造
3. 价值的载体 --- 差，有些物品可能具有额外价值，或者有些物品可能内部损坏（拿贝壳来举例）

**第二代货币：Trested Entity (Money 2.0)** 银行是人们都 *信任* 的实体机构

1. 作为交换的媒介 --- 很好，非线下交易，不用现金交易，转账
2. 用来给物品标价 --- 很好
3. 价值的载体 --- 一般，和机构实体绑定，不排除银行破产等状况

**第三代货币: Cryptocurrenty (Money 3.0)**

1. 去中心化规避了第三方的风险,例如银行倒闭
2. PoW使得数字货币具有一定价值,代表了计算资源价值
3. 数字化交易,避免了面对面交易

# **分布式总帐**

在具体涉及到比特币之前，我们要了解一个问题，什么是总帐？简而言之总帐就是一系列转账的集合，里面记录了发生在系统里面的每一笔转账，我们可以将其比喻成一本书，书里面每一页都记录了转账信息。那么中心化的账本和去中心化的账本有什么区别呢？

中心化的账本的安全依赖于账本的持有者。我们举一个连锁店的例子，所有的连锁店的账本都被总店掌握，因此只要总店的账本被偷偷更改了或者丢失了，整个连锁店就没有账本记录了。并且在实际情况中我们并不能保证总店总是安全的，因此就有了去中心化的账本。

去中心化账本安全依赖于整个系统。同样连锁店的例子，现在所有分店都有了所有店的总账本，每个一段时间分店就会***同步***他们拥有的账本，因此账本一致性是可以保持的，在这种情况下除非入侵者有能力修改半数以上的账本才可能完成攻击，而在实际情况下是很难发生的。因此去中心化更加安全可靠。

- 那么在区块链系统中出现不同账本到底怎么办？

<img src="{{site.url}}{{site.baseurl}}/img/ledgers.png" alt="Drawing" style="width: 300px;"/>

区块链的解决方案是选择最长的个账本，如果长度相同就随机选择一个账本作为自己同步后的账本。

- 区块链是怎么实现分布式账本的？

在区块链中所有的节点（矿工）都有自己的一个账本，每隔一个固定时间矿工们会收到其他矿工广播来的区块链信息（其他人的账本）。从而在自己的账本上衍生出其他的分支。如下图矿工经历过三次“同步”，第一次是矿工1-2-3-4'，发现同步链为1-2-3-4-5或更长，因此抛弃原来的链更新为新链。同理6‘，7’。

<img src="{{site.url}}{{site.baseurl}}/img/branches.png" alt="Drawing" style="width: 500px;"/>

- 如何增加账本长度？

通过计算区块链协议规定的一个数学难题，从而争夺增加链长度的机会，成功的矿工会将自己生成的新块广播给其他矿工（密码学保证了只有计算出答案的矿工可以广播）。

<img src="{{site.url}}{{site.baseurl}}/img/blockchian-structure.png" alt="Drawing" style="width: 600px;"/>

上图描述了最简单的区块链概念，我们假定一个人购买自行车的场景。首先买家B需要向卖家S转帐x个比特币，买家B发起了一笔转账，之后这笔转账被买家B的比特币钱包软件发送到比特币网络之中。在这时，比特币网络中存在无数个矿工通过计算一个数学问题去争夺这笔转账记录的执笔权（谁将这笔转账写入分布式账本中），最先计算出来的矿工将这笔转账写入到账本。这时我们就可以说这笔交易初步完成了，卖家S可以通过自己的比特币钱包来观察网络中有没有这笔转账从而确认转账。（需要了解到不同区块链协议的确认转账机制不同）。

在这个系统中
- 账本为区块链
- 矿工为将转账信息写入block的计算设备
- 生成块意味着去计算一个数学问题

既然分布式账本有这么多好处，那么最基础的问题：如何保持整个系统中账本的一致性？

# **共识问题**

共识问题是为了保证账本在系统中的一致性。共识存在与矿工之间，因为不同矿工的账本内容可能有差距，拥有了共识协议就可以保证转账记录的一致性。下图从抽象的角度描述了共识问题。

<img src="{{site.url}}{{site.baseurl}}/img/consensus.png" alt="Drawing" style="width: 300px;"/>

假设我们有n个参与者参与了共识协议，每个参与者在参与之前都持有不同内容v1....vn，在共识协议结束之后参与者所持有的内容会变为u1....un。

这也是一个经典的拜占庭问题。（假设有t个参与者不诚实）

我们先给出共识协议的定义：
- Termination： <img src="http://chart.googleapis.com/chart?cht=tx&chl= \forall i \in H, u_i" style="border:none;"> is defined 
- Agreement： <img src="http://chart.googleapis.com/chart?cht=tx&chl= \forall i, j \in H, u_i=u_j" style="border:none;">
- Validity： <img src="http://chart.googleapis.com/chart?cht=tx&chl= \exists v(\forall i \in H, v_i=v),(\forall i \in H, u_i=v)" style="border:none;">
- Strong Validity: <img src="http://chart.googleapis.com/chart?cht=tx&chl= \forall i \in H, \exists j \in H(u_i=V_j)" style="border:none;">

在上述定义中H代表着诚实参与者，翻译成人话如下：（可结束）对于所有诚实参与者i和j，i和j的输出ui一定会输出，也就是一定会结束。（全认同）对于所有诚实参与者i，i的输出ui是固定的。（值合法）诚实参与者的输入与输出相同。（强值合法）每个诚实参与者的输出一定可以做某个诚实参与者的输入。

我们假设现在有两类诚实的参与者A1和A0,分别以0.5的概率持有1和0。

- 如果入侵者入侵了A0，那么根据共识协议A1中的诚实参与者输出应该为1。
- 如果入侵者入侵了A1,那么根据共识协议A0中的诚实参与者输出应该为0。
- 如果入侵者没有举动，那么根据共识协议所有参与者输出结果应该相同。

因此在共识协议中当大部分参与者诚实的时候就可以做到账本在整个系中的一致性。

# **区块链的“块”**

我们都知道区块链是一个“链”，数据结构中的一个名词。那么区块链是由什么基本元素组成行程一个链的？答案是块。

块的数学表达式如下：<img src="http://chart.googleapis.com/chart?cht=tx&chl= B =<s, x, ctr>" style="border:none;"> s代表着指针指向之前区块; x代表着content，也就是转账记录transaction等信息;而ctrl是counter，代表着proof-of-work见证者（其实就是nonce）。

判断一个块是否有效： <img src="http://chart.googleapis.com/chart?cht=tx&chl= (H(ctr, G(s, x)) < T) and (ctr <= q)" style="border:none;">在这个判断式里面H和G分别代表了两种hash计算方法，T是target而q是upper bound 之后在proof-of-work会详细讲到。

而块与块之间的关系如下，块之间的递推关系：

<img src="http://chart.googleapis.com/chart?cht=tx&chl= s_i = H(ctr_i-1, G(s_i-1,x_i-1))" style="border:none;">

**例如**在比特币中有四个核心算法组成了比特币协议，下图给了伪码。首先就是验证块是否有效：

<img src="{{site.url}}{{site.baseurl}}/img/chain-validation.png" alt="Drawing" style="width: 600px;"/>

如何选择链作为自己的链：

<img src="{{site.url}}{{site.baseurl}}/img/chain-selection.png" alt="Drawing" style="width: 600px;"/>

Proof-of-work：

<img src="{{site.url}}{{site.baseurl}}/img/pow.png" alt="Drawing" style="width: 600px;"/>

主函数，负责循环：

<img src="{{site.url}}{{site.baseurl}}/img/backbone.png" alt="Drawing" style="width: 600px;"/>

# **区块链总览**

要分析区块链，首先就要先建立区块链的一些假设（Adversary Model）：

1. 有n个参与者在同步执行区块链协议，每个参与者都可以在同一个时间内访问q次Hash函数（Hash访问频率固定）。
2. 有t个参与者被入侵者控制。

在比特币系统中，区块链有一下几个基本性质：

**公共前缀**：

<img src="{{site.url}}{{site.baseurl}}/img/common-prefix.png" alt="Drawing" style="width: 500px;"/>

上图展示了3个不同的区块链分支，M1，M2和M3。

<img src="http://chart.googleapis.com/chart?cht=tx&chl= \forall r_1,r_2,(r_1<r_2),\qquad P_1,P_2;\qquad C_1,C_2: \qquad C_1^{\^k}<= C_2" style="border:none;">

人话：从比特币网络中任意取两个矿工p1,p2账本的任意两个回合（每个回合会有一个参与者胜出写入区块链）r1,r2，他们的链一定有相同的前缀。

**区块链增长问题**

<img src="{{site.url}}{{site.baseurl}}/img/chain-growth.png" alt="Drawing" style="width: 400px;"/>

我们假定mu为概率（至少有一个诚实的参与者计算出了POW），那么对于同一个参与者p的两个随机round，r1和r2来说，假定r2-r1=t（r2和r1之间相隔t轮），那么两个round在链上的长度差为c2-c1=mu×t。更具体一点的例子，如果我们找到第2轮和第四轮的区块链做比较，那么这两个区块链长度的插值为(4-2)×mu。

**链的质量问题**

<img src="{{site.url}}{{site.baseurl}}/img/chain-quality.png" alt="Drawing" style="width: 400px;"/>

假设mu为概率（本轮区块链由诚实参与者生成），那么对于长度为l的区块链来说为入侵者生成的概率为(1-mu)×l。

<img src="http://chart.googleapis.com/chart?cht=tx&chl= \mu \approx \frac{n-2t}{n-t}" style="border:none;">

那么这三个性质带来了什么？具体来说提供了区块链的persistence和liveness。

- Persistence: Transactions are organized in a 'log' and honest nodes agree on it. --- Common Prefix
- Liveness: New transactions are included in the log nodes, after a suitable period of time. --- Chain Growh and Chain Quality

# **比较共识和分布式总帐**

它们有什么区别？首先总帐是一个一直执行的一个协议，而共识协议是一瞬间执行。总而言之总帐是大于等于共识的关系。

比特币对共识采取的方式和正常的不同，有以下几点：
1. 比特币的共识是用广播完成的，不是P2P
2. 协议setup不是一个private correlated setup，比特币用的数字签名，并没有使用数字签名来校验矿工身份。

因此我们使用总帐来解决共识问题。共识一共有三个特性：可结束，全认同，值合法，观察为什么区块链满足了共识的这三个特性。

