---
layout: page
title: 用PHP对获得淘宝客的真实地址和内容
tagline: 
category : php
tags : [php, header, curl, 淘宝客]
---
{% include JB/setup %}

利用PHP CURL获取淘宝客链接的真实地址和内容，其实淘宝那边只是做了几次跳转，外加判断了一次refer，这样的话，普通的curl或者一些其他函数就不能得到最终的结果了。

## 代码
<script src="https://gist.github.com/cnxzcxy/82210caf6fd0b5e720c4.js"></script>
