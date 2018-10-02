---
layout: page
title:  "Multi-Party Computation"
date:   2018-6-7 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

Secure multi-party computation can also be called ***multi-party computation (MPC)*** or privacy-presenting computation. It aims at creating methods for parties to jointly compute a function over their inputs while keeping those inputs private.

> Formal Definition: In an MPC, a given number of participants, p1, p2, ..., pN, each have private data, respectively d1, d2, ..., dN. Participants want to compute the value of a public function on that private data: F(d1, d2, ..., dN) while keeping their own inputs secret.

From a higher perspective, MPC tries to solve a calculation problem where each participant does not trust each other. Therefore we need somehow do the calculation without knowing participants' secrets, the computation is based on ***secret sharing*** of all the inputs and ***zero-knowledge proofs*** for potentially malicious case, where majority of honest players in the malicious adversary case assure that bad behavior is detected and the computation continues with the dishonest person eliminated or his input revealed. [1]

# Secret Sharing

In a secrect sharing scheme, a set of players posses pieces of information that combine to form a common "secret". Unless the number of involved players exceeds a certain threshold, none can recontruct or obtain any information about the secret.

**Shamir's Secret Sharing Scheme**

<img src="{{site.url}}{{site.baseurl}}/img/shamir.png" alt="Drawing" style="width: 700px;"/>

# Homomorphic Encryption

Homomorphic encryption is a form of cryptographic encryption algorithm which is capable of computation of plaintexts by manipulating ciphertexts. The purpose of homomorphic encryption is to allow compuation plaintext without any prior knowledge about the plaintext.	

However homomorphic encryption schemes are inherently [malleable](https://en.wikipedia.org/wiki/Malleability_(cryptography)), so homomorphic schemes have weaker security proteries than non-homomorphic schemes.

<img src="{{site.url}}{{site.baseurl}}/img/group_hm.png" alt="Drawing" style="width: 300px;"/>

>Reference: Figure 2.1, Chapter 2.1, Yi, X., Bertino, E., & Paulet, R. (2014). Homomorphic encryption and applications (SpringerBriefs in computer science). Cham: Springer.

**Unpadded RSA**

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(m)= m^e\quad mod\quad n" style="border:none;">, where *e* is the private key and *n* is the group order.

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= x_1^e \cdot x_2^e\quad mod\quad n = E(x_1 \cdot x_2)" style="border:none;">

Unpadded RSA is multiplicative homomorphic.

**ElGamal**

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(m)= (g^r, m \cdot h^r)" style="border:none;">, where <img src="http://chart.googleapis.com/chart?cht=tx&chl= r \in (0,\quad \dots\quad, m-1),\quad h = g^x" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= (g^{r_1}, m \cdot h^{r_1})\cdot (g^{r_2}, m \cdot h^{r_2}) = (g^{r_1\quad %2B \quad r_2}, m \cdot h^{r_1\quad %2B \quad r_2}) = E(x_1 %2B x_2)" style="border:none;">

**Goldwasser-Micali**

1. Key Generation: 

---
# Reference

[1]  Wikipedia, Secure multi-party computation, url: [https://en.wikipedia.org/wiki/Secure_multi-party_computation](https://en.wikipedia.org/wiki/Secure_multi-party_computation)
