---
layout: post
title:  "Blockchains and Distributed Ledgers --- Intro"
date:   2018-1-19 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

`内容来源：维基百科`

**区块链**（英语：blockchain 或 block chain）是用分布式数据库识别、传播和记载信息的智能化对等网络, 也称为价值互联网。中本聪在2008年，于《[比特币白皮书](https://bitcoin.org/bitcoin.pdf)》中提出“区块链”概念，并在2009年创立了比特币社会网络，开发出第一个区块，即“创世区块”。

区块链共享价值体系首先被众多的加密货币效仿，并在工作量证明上和算法上进行了改进，如采用权益证明和SCrypt算法。随后，区块链生态系统在全球不断进化，出现了首次代币发售ICO；智能合约区块链以太坊；“轻所有权、重使用权”的资产代币化共享经济；和区块链国家。

目前，人们正在利用这一共享价值体系，在各行各业开发去中心化电脑程序(Decentralized applications, Dapp)，在全球各地构建去中心化自主组织和去中心化自主社区(Decentralized autonomous society, DAS)

**分布式总帐** (Distributedd Legers)：
> Distributed Ledger is a consensus of replicated, shared, and synchronized digital data geographically spread across multiple sites, countries, or institutions

我们只需要了解到分布式总帐不止存在于区块链中，分布式总帐也具有别的形式。

# 货币

什么是货币？货币毋庸置疑是日常生活不可或缺的一部分，在1874年一个人可以用一只鸡来换一年的报纸订阅。这项交易成立的条件是交易双方均认为一只鸡的在1874年的价值和一年报纸订阅的价值相同，双方建立了共识（consensus）机制来完成交易。经历过以物易物的阶段后，人们发现以物易物的方式效率很低，而且共识的建立必须是双方面对面确认了交易物品价值，因此黄金、白银作为了第一代的货币出现了。

我们这里拿出来货币的三种性质来对以下几种货币进行比较：
1. 一种交换的媒介。 --- A medium of exchange
2. 用来给物品或者服务标价。 --- A unit of amount
3. 价值的载体。--- A store of value

**第一代货币：实体货币（Money 1.0）** 黄金、白银、贝壳是过去所有人都 *承认* 的高价值物品。货币本质是一种convention，货币并不一定自身具有很高价值，只是大多数人们公认货币具有一定价值，因此在货币的定义期间，共识是十分重要的组成部分。下面我们来分析第一代货币的性质：
1. 作为交换的媒介 --- 一般，只能用于面对面交易
2. 用来给物品标价 --- 一般，可以被替代，不容易被分割价值，易于被仿造
3. 价值的载体 --- 差，有些物品可能具有额外价值，或者有些物品可能内部损坏（拿贝壳来举例）

**第二代货币：Trested Entity (Money 2.0)** 银行是人们都 *信任* 的实体机构

1. 作为交换的媒介 --- 很好，非线下交易，不用现金交易，转账
2. 用来给物品标价 --- 很好
3. 价值的载体 --- 一般，和机构实体绑定，不排除银行破产等状况

**第三代货币: Cryptocurrenty (Money 3.0)**

# 密码学基础知识

**Hash Funcions**

- An algorithm that produces a fingerprint to a file.
- H({1,0}*) -> {1,0}

哈希方程的特点就是可以将任意长度的数字序列转换成一个固定长度的序列，并且无法通过简便方法找到两个相同的input对应同一个ouput(collision)

Collision Attack: Find x,y H(x)=H(y)

Second pre-image attack: Find H(x)=H(y) for given x

Birthday paradox:



