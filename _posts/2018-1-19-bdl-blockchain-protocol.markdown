---
layout: post
title:  "区块链协议 --- Blockchain Protocol"
date:   2018-4-20 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

# **本章内容**

1. 区块链中的数据结构
2. 比特币的Difficulty
3. 不同的POW机制
4. 区块链的激励机制
5. 一些攻击

# **区块链中的数据结构**

**递归的数据结构**

在递归的数据结构中，一个新的数据结构实例是由其他一个或者多个已存在的数据结构实例组成的。

> A new data structure instance D can be defined given one or more intances of the data structure as well as additional data.

一般情况下递归的数据结构都有四个基础要素：

1. membership 用来判断元素从属关系
2. append 添加元素到末尾
3. insert 添加元素到任意位置
4. delete 删除元素

**认证数据结构**

> A data structure instance has a short fingerprint, and operations on it can be outsourced to an untrusted prover who updates the representation as well as proves the update is correct.

**Hash Chain**

在hash链中，任意一个可以观察到hash链的人都可以用hash指针(指纹)去校验整条链的完整性。下图显示了hash链的元素末尾添加过程，新的指纹和旧的指纹都可以被任何人校验。

<img src="{{site.url}}{{site.baseurl}}/img/hashchain.png" alt="Drawing" style="width: 450px;"/>

我们假设有两个人Prover和Verifier去完成认证过程。Prover拥有整个hash链并且负责具体执行添加过程并且说服Verifier自己成功添加了元素(完成认证)。另一个人是Verifier也拥有添加之前的hash链，校验Prover的添加过程。

- 安全性：如果一个人持有fingerprint，其他人就很难改变hash chain里面的内容，因为hash方程是collision-resistent。
- 效率：hash链只支持有效的append运算，不支持有效的membership, insert, delete运算。

**Merkle Tree**

可以看作hash chain添加memebership, insert, delete运算功能。

<img src="{{site.url}}{{site.baseurl}}/img/merkletree.png" alt="Drawing" style="width: 450px;"/>

- Memebership认证：例如证明L2在Merkle Tree里面，我们只需要L2，Hash0-0和Hash1来证明L2在Merkle Tree里面。而在hash chain里面则需要将L2之后的所有值都计算一遍hash。

# **比特币的“块”**
在之前给出了区块链中块的数学表达式：<img src="http://chart.googleapis.com/chart?cht=tx&chl= B =<s, x, ctr>" style="border:none;">下图给出了比特币的块结构：

<img src="{{site.url}}{{site.baseurl}}/img/bitcoin-blockheader.png" alt="Drawing" style="width: 600px;"/>

- Version：代表了当前块的版本。
- HashPrevBlock：上一个块的hash值(区块链是一条hash链)。
- HashMerkleRoot：该块中转账信息组成的merkle tree的root，可以用来认证转账在本块中。
- Time：创建该块的时间
- Bits：当前目标，显示了difficulty
- Nonce：和POW有关，证明了矿工的计算能力

对比区块链的块和比特币的块，其中s为HashPrevBlock，ctr为Nonce，x为其他所有项。

（一定要区分清楚什么是区块链中的概念什么是比特币中的概念）

# **比特币的Proof Of Work**

基本算法（巨简单）：

<img src="{{site.url}}{{site.baseurl}}/img/bitcoin-pow.png" alt="Drawing" style="width: 350px;"/>

比特币的POW可以被平行计算：矿池
- 许多设备一起计算同一个“块”的POW
- 如果有一台设备成功了，奖励会发给矿池中的每个设备
- 奖励基于已经计算失败的hash数量
- 矿池中的成员可以通过提供计算失败的hash来证明自己是该矿池的成员

# **比特币的难度(difficulty)**

之前我们提到区块链的账本时我们提到节点会接受最长的链，但是在比特币中不是这样的，矿工只会接受具有最大难度的链。

什么是比特币的难度？根据定义，比特币的难度和<img src="http://chart.googleapis.com/chart?cht=tx&chl= \sum_i^{n} \quad \frac{1}{T_i}" style="border:none;">线性相关。(T为target值，n为当前块的数量)

我们定义了一个f为每轮竞争产生新block的概率，f和target，矿工数量和每轮持续时间相关。

- 如果f很小，虽然转账一直在发生，但是区块链增长很慢，因此每笔转账发生后很久才会出现在总帐里面。区块链的liveness(每个一段时间新的转账就会写入账本)性质就会不存在。
- 如果f很大，那么区块链就会增长过快，就会可能在同一时间很多矿工同时计算出来了结果，这样区块链中就可能存在很多不同的账本。区块链的persistence(只存在一个公认的账本)性质就会不存在。

为了在一个动态的环境中解决这个问题，比特币每次都会重新计算target使得<img src="http://chart.googleapis.com/chart?cht=tx&chl= f(T,n) \approx f(T_0,n_0) = f_0" style="border:none;"> 从而使得f的值不会变得很大也不会变得很小。下面给出了重新计算target的具体算法。

<img src="{{site.url}}{{site.baseurl}}/img/target-cal.png" alt="Drawing" style="width: 400px;"/>

T0是最初始的target，tau为重新计算阙值参数，T为重新计算前的target，n0为准备进行计算的矿工数量。

<img src="{{site.url}}{{site.baseurl}}/img/target-cal2.png" alt="Drawing" style="width: 500px;"/>

**Bahack's Attack**

阐述了重新计算target的必要性。这种攻击的成功实施会成功将入侵者的块加入信任的账本，具体在下面的例子中介绍。

**Clay Pigeon Shooting Game(射击游戏)**

假设我们进行靶场涉及训练，我们射击命中目标概率为0.3,而我们的对手命中率是0.4。两人分别在靶场射击目标一千次，命中一次得一分，最后分数最多的人胜利。

那么我们获胜概率是多少？

通过简单的概率论计算我们知道我们命中均值为300,而对手命中均值为400，假设X为我们命中的数量，Y为对手命中数量，那么：

- 我们得分超过345的概率：<img src="http://chart.googleapis.com/chart?cht=tx&chl= Prob[\sum_{i=1}^{1000} X_i>345]<exp((-0.15)^2*300/3)<11%" style="border:none;">
- 对手得分低于348的概率：<img src="http://chart.googleapis.com/chart?cht=tx&chl= Prob[\sum_{i=1}^{1000} Y_i>348]<exp((-0.13)^2*400/3)<3.5%" style="border:none;">
- 上述同时发生概率：<img src="http://chart.googleapis.com/chart?cht=tx&chl= Prob[X>345, Y<348]<1-(1-Prob[X>345])(1-Prob[Y>348])<15%" style="border:none;">

因此和我们想象的相同，我们因为运气原因获胜的概率低于百分之十五。那么修改一下规则，如果我们可以选择不同大小的目标，为原来目标大小的1/B，相应的我们的射击得分也会变为原来的B倍，射击准确度也变为之前的1/B，那么情况会不会发生变化？

我们命中均值为E[X]=1000\*B\*0.3\*1/B=300。

对于我们得分超过345的概率：<img src="http://chart.googleapis.com/chart?cht=tx&chl= Prob[\sum_{i=1}^{1000} X_i>345]<exp((-0.15)^2*300*B/3)" style="border:none;">

结果和B的值有关，当B是1时，概率小于10.5%; 而当B为0.1时，概率小于79.8%; 因此当B无线小时，我们获胜概率可以达到很大。

现在回到target值重新计算的问题，如果比特币区块链的攻击者可以将target设置成无线小，那么即使攻击者的私有链很难获胜，但是在某一次偶然获胜时，攻击者的链难度会变成无限大，从而导致其他矿工接受攻击者的私有链。这种攻击就叫做Behack's Attack。

# **其他的区块链**

比特币目前是一个很成功的区块链应用，但是比特币转账速度很慢并且很耗电(大概一秒有7-8次转账，消耗350MW电量)。

- 不同的块生成速率，和f参数相关，但是加快速度也会导致一系列链分叉问题。
- 不同的POW机制，例如Proof-of-space、Proof-of-elapsed-time和Proof-of-stake。

比特币使用SHA256去计算POW，但是SHA256并不是专门为比特币POW计算而发明的，用Memory Hard Function会更加有效。

**Proof-of-stake**

POS是被人研究最多的POW替代者。POW是通过计算hash值来证明时间消耗(计算能力消耗)，而POS是基于货币量来生成下一个块，例如区块链中货币的持有者可以将自己的帐号变成特殊的矿工帐号，每隔一段时间就会根据账户余额来添加新的块，如果一直诚实的话会收到基于当前余额的分红(余额在挖矿期间不可买卖)。对于POW的优势是不需要浪费计算资源。

简单来说，就是一个根据你持有货币的量和时间，给你发利息的一个制度，在股权证明POS模式下，有一个名词叫币龄，每个币每天产生1币龄，比如你持有100个币，总共持有了30天，那么，此时你的币龄就为3000，这个时候，如果你发现了一个POS区块，你的币龄就会被清空为0。你每被清空365币龄，你将会从区块中获得0.05个币的利息(可理解为年利率5%)，那么在这个案例中，利息 = 3000 * 5% / 365 = 0.41个币。[[来源]](https://zhidao.baidu.com/question/245222304007133484.html)

如何更加深入理解这个过程？ 

[Proof of Stake by bitcoin.wiki](https://en.bitcoin.it/wiki/Proof_of_Stake)

所以在每一轮选取获胜的股权持有人的时候，必须要保证随机，也就是不可被股权所有人预测结果。

<img src="{{site.url}}{{site.baseurl}}/img/PoS.png" alt="Drawing" style="width: 300px;"/>

有许多算法可以实现这个过程，最简单的一个方法叫做"follow the satoshi"。将网络中所有的satoshi做成一个数据结构中的树，从根节点开始翻硬币选择左右分叉，最后到的分支satoshi的拥有者就是本轮的赢家。

<img src="{{site.url}}{{site.baseurl}}/img/PoS2.png" alt="Drawing" style="width: 200px;"/>

但是这样是不够的，其中的翻硬币机制应该有以下两个特点。一，没有人可以影响掷硬币的结果，结果应该是全随机; 二，掷硬币的结果应该是可以全网校验的，也就是所有参与者都应该同意掷硬币结果。

最初的设想：利用前一个块某些字节的hash去掷硬币，但是这样会存在矿工只会接受或者发布下一笔对自己有利的块。

解决方案是运用"Blum's coin flipping protocol"。现代密码学最初讲到的掷硬币的commitment和verification机制。

# **总帐的激励机制**

之前在第一篇博客里面提到了区块链的安全性是建立在大多数参与者诚实的基础上的。那么，为什么参与者也就是矿工会自愿保持诚实？因为矿工会因为自己的诚实获得奖励。

比特币中有两个激励机制：

1. 创建“块”的固定工资(随时间递减)。固定工资存在“块”的一个transaction中，矿工在创建块时会填上自己的public key来接受这笔钱。
2. 转账人付给矿工的转账费(转账人自愿支付从转账的钱里面扣除，最少1000 satoshi)。

那么比特币激励机制是不是"incentive compatible"？Incentive compatible有两个性质。

- **激励协议是主导策略**

主导策略是指遵循主导策略会达到最大利益。经典的囚徒困境，假设我们审判两个共犯，囚犯可以选择背叛或者选择保守秘密，具体判机制如下：

<img src="{{site.url}}{{site.baseurl}}/img/dominant-strategy.png" alt="Drawing" style="width: 450px;"/>

在这种情况下对于个人来说背叛是主导策略，因为背叛有几率不服刑(最大利益)。

- **激励协议是纳什平衡**

纳什平衡是是指如果所有参与者都遵循协议达到了纳什平衡，其中任意一个参与者不能通过欺骗得到更大利益。在上图的囚徒困境中，双方共同背叛是纳什平衡的。

对于比特币来说我们定义矿池机制和挖矿机制为两种挖矿策略，进而讨论比特币激励机制是不是incentive compatible。比特币的奖励机制可以分为两类：

1. 绝对奖励：比特币最长(最大difficulty)区块链中的链会给创建块的固定工资。
2. 相对奖励：自己的绝对奖励除以所有参与者共同的绝对奖励。

历史上的比特币奖励机制(utility为最后得到比特币数量)：（看不懂）

1. Bitcoin utility is equivalent to the exepected value of absolute rewards.(2013)
2. Bitcoin utility is equivalent to the exepected value of relateive rewards.(2014)
3. Bitcoin utility is equivalent to the exepected value of relateive rewards.(2016)

# **针对比特币的攻击**

**自私挖矿(Selfish Mining)**

矿池中的合作设备违反协议获取更多的相对奖励。

首先攻击者接受目前区块链中的公有链，然后开始计算。之后

1. 攻击者没有最先算出来，继续接受共有链，继续下一个难题。
2. 攻击者最先算出来数学难题，不广播这个块，而是等有其他人算出来之后同时公布计算出来的块。

因为攻击者首先计算出目前的块之后会继续计算下一个块，所以攻击者始终 占有先计算的优势，从而导致在公有链上更多的块是由攻击者生成的。或者在另一个视角看，攻击者的私有链会远远的长于公有链，因此会收到更多的相对奖励。

[不像上课讲的那么蠢的解释](https://www.zhihu.com/question/21976182)

**Block reward 0**

**Bribery Attack**
