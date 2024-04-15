---
layout: page
title: About
description: 梦想·生活·工作
keywords: lanhonghao, 蓝宏浩
comments: true
menu: 关于
permalink: /about/
---

我是蓝宏浩。

工欲善其事，必先利其器。

热爱**机器人制作**，热爱**DIY创作**，热爱**开源**。

爱好阅读，足球运动。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
