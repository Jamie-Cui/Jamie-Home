---
layout: post
title:  "Blockchains and Distributed Ledgers"
date:   2018-4-19 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

**本章内容**

1. 作为平台的分布式总帐
2. 智能合约
3. 以太坊

# **智能合约**

合约(Contract)规定了利益方之间的关系并且合约包含了利益方之间的承诺。智能合约(Smart Contract)在1994年被Nick Szabo创造，是一种在数字货币中建立利益双方联系的协议，相比于传统的合约它更加安全，更标准化，功能性更加强大。

1. 智能合约是由代码编写而成运行在去中心化的区块链上，因此智能合约也具有区块链的特点，规避第三方信任的风险。对于建立合约的双方来说减少了花费。
2. 运用了密码学和其他的安全及值保证了算法安全和合约安全。

许多去中心化的数字货币，例如比特币、以太坊，都为智能合约的实现提供了良好的底层结构(infrastructure)。智能合约的代码会被在区块链系统中运行共识协议的矿工运行，共识协议保证了矿工一定会正确履行智能合约。简而言之我们可以认为智能合约被一个虚拟的系统公认的安全设备诚实运行。

# **比特币转账**

比特币的转帐是从转账发起者的账户中转移比特币到接收者的账户中。转账是由广播的形式到在特币系统中传播。

是用户在发起转账的时候只需要生成转账信息，签上自己的数字签名，然后将转账信息发送到网络中。接收到转账信息的矿工会首先用数字签名校验转账信息的正确性，然后将转账信息添加到自己本轮生成的区块链的“块”中。每隔一段时间，矿工就开始互相竞争计算一个数学难题，赢家将自己的“块”广播给别的矿工，诚实的矿工会将赢家的“块”连接到自己的区块链上。

<img src="{{site.url}}{{site.baseurl}}/img/bitcoin-structure.png" alt="Drawing" style="width: 600px;"/>

**转账格式**

因为区块链也就是账本中只存在各个账户的转账信息，并不存在账户余额查询类似服务，因此在发起转账时转账发起者必须证明自己有足够余额去支付转账金额。比特币给出的解决方案是在转账中需要发起人额外提供上一笔转账的address和index信息（进而查询是否上一笔转账接收人为当前的发起人）。

<img src="{{site.url}}{{site.baseurl}}/img/transaction.png" alt="Drawing" style="width: 350px;"/>

1. Previous transaction address(Txid)：上一个转账的hash值
2. Index：上一笔转账的index信息
3. Signature Script(ScriptSig)：转账发起人对自己拥有比特币来源的证明
4. Value：转账的比特币数量
5. Public Key Script(ScriptPubKey)：转账接收人

在检查transaction是否合法的时候，矿工会将两个脚本结合起来ScriptSig\|\|ScriptPubKey然后执行结合之后的脚本，如果没有问题，检查格式等其他项。

**比特币脚本(bitcoin script)**

比特币脚本为了转账而生。和正常的脚本语言相同，是一种从左向右的基于栈的简单语言。比特币脚本中有256种运算，可以被分为以下几类：

1. Arthimetic 数学类: OP_ABS, OP_ADD
2. Stack 栈操作类: OP_DROP, OP_SWAP
3. Flow Control 流操作类: OP_IF, OP_ELSE
4. Bitwise logic 比特操作: OP_EQUAL, OP_EQUALVERIFY
5. Crypto 密码类操作: OP_SHA1, OP_SHA256, OP_CHECKSIG, OP_CHECKMULTISIG
6. Locktime 锁定时间: OP_CHECKLOCKTIMEVERIFY, OP_CHECKSEQUENCEVERIFY

下面给出了一个具体的比特币脚本运行的例子：

<img src="{{site.url}}{{site.baseurl}}/img/bitcoinscript.png" alt="Drawing" style="width: 700px;"/>

- 多签名(Multisignature)：至少需要获取m个密钥来授权比特币转账。
- CoinJoin：为了匿名的实现，会将一些比特币的转账用类似Chuam's Mix的方式将诶合起来。
- Micropayment/payment途径：用最低的费用同时进行的多笔转账

当然，比特币脚本也拥有很多缺陷：

1. 不是图灵完备：不支持循环语句，这是由于比特币协议的自身选择导致的
2. 不支持任意的状态变量：例如状态变量记录当前转账进度，人话也就是说不支持脚本给变量任意赋值。

# **以太坊(Ethereum)**

[以太坊](https://www.ethereum.org/)是一个开源的区块链平台，在以太坊平台上用户自己创建运行分布式应用。和比特币类似，以太坊基于分布式的区块链，P2P网络和共识协议。

"Ether"是以太坊的货币单位。以太坊中有两种帐号：(1) *外部拥有帐号*; (2) *智能合约帐号*

<img src="{{site.url}}{{site.baseurl}}/img/ethereum-accounts.png" alt="Drawing" style="width: 600px;"/>

因此contract帐号之间的相互交流是通过unsigned transaction，没有用到public key 形式。而外部拥有帐号不持有智能合约代码也不能建立里unsigned transaction。ps.需要了解到合约可以建立新的合约帐号。

**状态机复制(State Machine Replication)**

定义不是很理解，上图：

<img src="{{site.url}}{{site.baseurl}}/img/state-machine.png" alt="Drawing" style="width: 600px;"/>

比特币状态机复制：

1. 比特币是一种特殊的状态机
2. 总账本可以被认为是一个状态机标注了所有比特币当前的归属情况
3. 比特币状态机的最初状态是创世块
4. 改变状态时收到的input为当前状态和新的转账，然后输出一个新的状态

以太坊状态机复制：

1. 以太坊的状态机包含所有帐号的信息。
2. 改变状态时收到的input为当前状态和新的转账，然后输出一个新的状态
3. 矿工会追踪比特币帐号的余额并且实时更新

**Gas**

因为以太坊支持图灵完备的语言，因此几乎所有代码都可以转变成语言写入到以太坊的合约中，即使让所有矿工一直工作下去的代码。因此为了防止一只工作不会停下这种情况的发生，以太坊规定了Gas用以强制停止代码执行。

Gas也是一种POW证明。在以太坊中智能合约的运行和转账的发生都会消耗掉一定量的gas。如果一笔转账消耗的gas超过了gas limit,超过的部分会被退还。

**以太坊的智能合约(Ethereum Smart Contract)**

以太坊中智能合约是一个存有代码的帐号，智能合约不能调用自己并且智能合约智能被以太坊帐号调用。而对于存在与以太坊网络中矿工来说，他们会 (1)运行和校验接收到的转账 (2)运行调用的合约脚本 (3)用区块链使共识协议进入下一个状态

语言

- Solidity(首选，面对对象语言，类似javascript)
- Serpent
- LLL
- Mutant

以太坊虚拟机(EVM)是智能合约运行的安全环境，每个矿工都在运行EVM。以太坊的智能合约由高级的编程语言完成实现，通过ABI转换成EVM可以理解的低层次语言。类似于Java。

以下给出了两个solidity编写的合约：

- (1) 基础合约

<img src="{{site.url}}{{site.baseurl}}/img/solidity-1.png" alt="Drawing" style="width: 600px;"/>

- (2) Token 系统

<img src="{{site.url}}{{site.baseurl}}/img/solidity-2.png" alt="Drawing" style="width: 600px;"/>

- (3) Event 和 Log

<img src="{{site.url}}{{site.baseurl}}/img/solidity-3.png" alt="Drawing" style="width: 600px;"/>

- (4) 复杂的合约

<img src="{{site.url}}{{site.baseurl}}/img/solidity-4.png" alt="Drawing" style="width: 600px;"/>






