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



[1]  Reference from wikipedia: [https://en.wikipedia.org/wiki/Secure_multi-party_computation](https://en.wikipedia.org/wiki/Secure_multi-party_computation)
