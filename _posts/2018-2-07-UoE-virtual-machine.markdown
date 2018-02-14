---
layout: post
title:  "How to DO secure programming coursework on your own machine?"
date:   2018-2-06 12:00:00 +0000
categories: jekyll update
---

{% include add_favicon.html %}
{% include add_views.html %}

<span id="busuanzi_container_page_pv">
   views: <span id="busuanzi_value_page_pv"></span>
</span>

This tutorial works only on machine with `sftp` installed. (Secure File Transmission Protocol)

1. Using your favorite terminal to connect with dice
`sftp s17*****@student.ssh.inf.ed.ac.uk`
and input your password.
2. Enter the ova file path
`cd /afc/inf.ed.ac.uk/group/teaching/module-sp`
3. Download the ova file
`get SecureProgramming-Coursework.ova`
4. Exit sftp
`exit`
5. Open virtual box on your own machine, import appliance and enjoy!