---
layout: page
title:  "多方安全计算 --- Multi-Party Computation"
date:   2018-12-18 13:00:00 +0000
categories: jekyll update
---
{% include add_favicon.html %}
{% include add_views.html %}

Secure multi-party computation can also be called ***multi-party computation (MPC)*** or privacy-presenting computation. It aims to create methods for parties to jointly compute a function over their inputs while keeping those inputs private.

> Formal Definition: In an MPC, a given number of participants, p1, p2, ..., pN, each have private data, respectively d1, d2, ..., dN. Participants want to compute the value of a public function on that private data: F(d1, d2, ..., dN) while keeping their own inputs secret.

From a higher perspective, MPC tries to solve a calculation problem where each participant does not trust each other. Therefore we need somehow do the calculation without knowing participants' secrets, the computation is based on ***secret sharing*** of all the inputs and ***zero-knowledge proofs*** for potentially malicious case, where majority of honest players in the malicious adversary case assure that bad behavior is detected and the computation continues with the dishonest person eliminated or his input revealed. [1]

1. Obliviou Transfer
2. Garbled Circuit

# Secret Sharing

In a secrect sharing scheme, a set of players posses pieces of information that combine to form a common "secret". Unless the number of involved players exceeds a certain threshold, none can recontruct or obtain any information about the secret.

**Shamir's Secret Sharing Scheme**

<img src="{{site.url}}{{site.baseurl}}/img/shamir.png" alt="Drawing" style="width: 700px;"/>

**Blakley's scheme**

Two nonparallel lines in the same plane intersect at exactly one point. Three nonparallel planes in space intersect at exactly one point. More generally, any n nonparallel (n − 1)-dimensional hyperplanes intersect at a specific point. The secret may be encoded as any single coordinate of the point of intersection. If the secret is encoded using all the coordinates, even if they are random, then an insider (someone in possession of one or more of the (n − 1)-dimensional hyperplanes) gains information about the secret since he knows it must lie on his plane. If an insider can gain any more knowledge about the secret than an outsider can, then the system no longer has information theoretic security. If only one of the n coordinates is used, then the insider knows no more than an outsider (i.e., that the secret must lie on the x-axis for a 2-dimensional system). Each player is given enough information to define a hyperplane; the secret is recovered by calculating the planes' point of intersection and then taking a specified coordinate of that intersection. 

**Chinese remainder theorem**

# Homomorphic Encryption

Homomorphic encryption is a form of cryptographic encryption algorithm which is capable of computation of plaintexts by manipulating ciphertexts. The purpose of homomorphic encryption is to allow compuation plaintext without any prior knowledge about the plaintext.	

However homomorphic encryption schemes are inherently [malleable](https://en.wikipedia.org/wiki/Malleability_(cryptography)), so homomorphic schemes have weaker security proteries than non-homomorphic schemes.

<img src="{{site.url}}{{site.baseurl}}/img/group_hm.png" alt="Drawing" style="width: 300px;"/>

>Reference: Figure 2.1, Chapter 2.1 [2]

**Unpadded RSA**

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(m)= m^e\quad mod\quad n" style="border:none;">, where *e* is the private key and *n* is the group order.

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= {x_1}^e \cdot {x_2}^e\quad mod\quad n = E(x_1 \cdot x_2)" style="border:none;">

**ElGamal**

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(m)= (g^r, m \cdot h^r)" style="border:none;">, where <img src="http://chart.googleapis.com/chart?cht=tx&chl= r \in (0,\quad \dots\quad, m-1),\quad h = g^x" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= (g^{r_1}, m \cdot h^{r_1})\cdot (g^{r_2}, m \cdot h^{r_2}) = (g^{r_1\quad %2B \quad r_2}, m \cdot h^{r_1\quad %2B \quad r_2}) = E(x_1 %2B x_2)" style="border:none;">

**Goldwasser-Micali**

In the Goldwasser–Micali cryptosystem, if the public key is the modulus m and quadratic non-residue x, then the encryption of a bit b is <img src="http://chart.googleapis.com/chart?cht=tx&chl= E(b) = x^b\quad r^2\quad mod\quad m" style="border:none;">, where <img src="http://chart.googleapis.com/chart?cht=tx&chl= r \in (0, \quad m-1)" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(b_1)\cdot E(b_2)= x^{b_1}\quad {r_1}^2\quad x^{b_2}\quad {r_2}^2\quad mod \quad m = x^{b_1 %2B b_2} \quad (r_1 r_2)^2\quad mod \quad m " style="border:none;">


**Benaloh**

An extension of Goldwasser-Micali cryptosystem. In the Benaloh cryptosystem, if the public key is the modulus m and the base g with a blocksize of c, then the encryption of a message x is <img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x) = g^x\quad r^c\quad mod\quad m" style="border:none;">, where <img src="http://chart.googleapis.com/chart?cht=tx&chl= r \in (0, \quad m-1)" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= g^{x_1}\quad {r_1}^c\quad g^{x_2}\quad {r_2}^c\quad mod \quad m = g^{x_1 %2B x_2} \quad (r_1 r_2)^c\quad mod \quad m " style="border:none;">


**Paillier**

In the Paillier cryptosystem, if the public key is the modulus m and the base g, then the encryption of a message x is <img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x) = g^x\quad r^m\quad mod\quad m^2" style="border:none;">, where <img src="http://chart.googleapis.com/chart?cht=tx&chl= r \in (0, \quad m-1)" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl= E(x_1)\cdot E(x_2)= g^{x_1}\quad {r_1}^m\quad g^{x_2}\quad {r_2}^m\quad mod \quad m^2 = g^{x_1 %2B x_2} \quad (r_1 r_2)^m\quad mod \quad m^2 " style="border:none;">

**Fully homomorphic encryption**

Fully homomorphic encryption (FHE) techniques allow one to evaluate both addition and multiplication of plaintext, while remaining encrypted. The concept of FHE was introduced by Rivest under the name privacy homomorphisms. The problem of constructing a scheme with these properties remained unsolved until 2009, when Gentry presented his breakthrough result. His scheme allows arbitrary computation on the ciphertexts and it yields the correct result when decrypted.

For integers,  Marten van Dijk, Craig Gentry, Shai Halevi and Vinod Vaikuntanathan presented a homomorphic encryption scheme in 2010 [3].

# Making Hommomorphic Encryption Pratical

**Electronical Medical Records**

Consider a futuristic scenario where devices continuously collect vital health information, and stream them to
a server who then computes some statistics (over these measurements, and over the course of time) and presumably
decides on the course of treatment (e.g., whether the dosage of medicine should be changed). The volume of the data involved is large, and thus, the patient presumably does not want to store and manage all this data locally; she may prefer to use cloud storage and computation. To protect patient privacy, all the data is uploaded in encrypted form, and thus the cloud must perform operations on the encrypted data in order to return (encrypted) alerts, predictions, or summaries of the results to the patient.[4]

*For these functions, it suffices to have a somewhat homomorphic encryption system which
computes many additions and a small number of multiplications on ciphertexts.*

---
# Reference

[1]  Wikipedia, Secure multi-party computation, url: [https://en.wikipedia.org/wiki/Secure_multi-party_computation](https://en.wikipedia.org/wiki/Secure_multi-party_computation)

[2] Yi, X., Bertino, E., & Paulet, R. (2014). Homomorphic encryption and applications (SpringerBriefs in computer science). Cham: Springer.

[3] Marten, van Dijk; Gentry, Craig; Halevi, Shai; Vinod, Vaikuntanathan. "Fully Homomorphic Encryption over the Integers". EUROCRYPT 2010. Springer.

[4] Michael Naehrig, Kristin Lauter, and Vinod Vaikuntanathan. 2011. Can homomorphic encryption be practical?. In Proceedings of the 3rd ACM workshop on Cloud computing security workshop (CCSW '11). ACM, New York, NY, USA, 113-124. DOI=http://dx.doi.org/10.1145/2046660.2046682 
