---
layout: page
title: About
description: 打码改变世界
keywords: 希罗卡, 小卡,郝晓飞
comments: true
menu: 关于
permalink: /about/
---

我是希罗卡，相信程序的力量。

仰慕「优雅编码的艺术」。

坚信熟能生巧，努力改变人生。

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