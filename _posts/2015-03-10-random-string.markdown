---
layout: post
title:  "Ruby 生成随机字符串"
date:   2015-03-10 04:38:30
categories: css
---

{% highlight ruby %}
Digest::SHA1.hexdigest(SecureRandom.urlsafe_base64.to_s)
{% endhighlight %}
