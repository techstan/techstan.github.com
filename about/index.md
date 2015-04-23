---
title: 关于
layout: page
comments: no
---

{{ site.about }}

----

###联系方式：

{% if site.qq %}
ＱＱ：[{{ site.qq }}](tencent://message/?uin={{ site.qq }})
{% endif %}
网站：[{{ site.name }}]({{ site.url }})

邮箱：[{{ site.email }}](mailto:{{ site.email }})

GitHub : [http://github.com/{{ site.github }}](http://github.com/{{ site.github }})

----

{% if site.weibo %}
[![新浪微博](http://service.t.sina.com.cn/widget/qmd/1932581030/2dad15cd/3.png)](http://weibo.com/u/1932581030?s=6uyXnP)
{% endif %}
