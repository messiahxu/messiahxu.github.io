---
layout: post
title:  "Javascript 中判断数字是否为浮点数"
date:   2015-06-11 13:38:34
categories: javascript
---

{% highlight coffeescript %}
is_float = (number) ->
    if parseInt(number) == parseFloat(number) then true else false
{% endhighlight %}
