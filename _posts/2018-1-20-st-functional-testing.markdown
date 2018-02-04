---
layout: post
title:  "Functional Testing 功能性测试"
date:   2018-1-19 12:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span> 

# **Learning Objective**
通过学习功能性测试我们能了解到什么？

- Understand the rationale for systematic (non-random) selection of test cases. Understand the basic concept of partition testing and its underlying assumptions
- 理解系统（非随机）选择测试用例的基本原理，尤其是了解分区测试的基本概念和假设。

- Understand why functional test selection is a primary, base-line technique. Why we expect a specification-based partition to help select valuable test cases
- 了解为什么功能测试选择是软件测试中主要的基本技术，理解为什么我们期望一个基于规范的分区来选择有价值的测试用例。

- Distinguish functional testing from other systematic testing techniques
- 区分功能测试和其他测试。

# **Definition 概念**

首先引用[百度百科：功能测试](https://baike.baidu.com/item/%E5%8A%9F%E8%83%BD%E6%B5%8B%E8%AF%95/10921202?fr=aladdin)的概念
> 功能测试是指根据产品特性、操作描述和用户方案，测试一个产品的特性和可操作行为以确定它们满足设计需求。

因此功能测试只是为了检查软件是否满足了用户的各项需求，不必考虑软件的结构或者代码漏洞。

- Functional testing: Deriving test cases from program specifications 功能测试就是从程序的规范中提取测试用例，功能测试也被称作“黑盒测试”。

