---
layout: post
title:  "字符串 unicode 码转换"
date:   2015-05-15 23:08:34
categories: ruby
---

{% highlight ruby %}
str = '宁卫文文山'
code_point_array = str.unpack('U*') # => [23425, 21355, 25991, 25991, 23665]
hex_code_point_array = code_point_array.map{|e| e.to_s(16)} # => ["5b81", "536b", "6587", "6587", "5c71"]
hex_code_point_array.map{|e| e.hex} # => [23425, 21355, 25991, 25991, 23665]
code_point_array.pack('U*') # => '宁卫文文山'
{% endhighlight %}
