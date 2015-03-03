---
layout: post
title: "Rails 中如何搜索 Postgresql 的特殊类型"
date: 2015-03-04 00:30
categories: database
---

搜索 jsonb

{% highlight ruby %}

Hehe(id: integer, name: string, lala: string, created_at: datetime, updated_at: datetime, test_json: json, test_jsonb: jsonb)

h = Hehe.last
=> #<Hehe id: 1, name: "a", lala: nil, created_at: "2015-03-03 13:32:51", updated_at: "2015-03-03 16:31:47", test_json: nil, test_jsonb: {"hehe"=>{:a=>"a", :b=>"b", :c=>{:a=>"a", :b=>"b", :c=>{:a=>"a", :b=>"b"}}}, "haha"=>"hhh", "hoho"=>"ooo"}>

Hehe.where("test_jsonb @> ?", {'haha': 'hhh'}.to_json)
=> #<ActiveRecord::Relation [#<Hehe id: 1, name: "a", lala: nil, created_at: "2015-03-03 13:32:51", updated_at: "2015-03-03 16:31:47", test_json: nil, test_jsonb: {"haha"=>"hhh", "hehe"=>{"a"=>"a", "b"=>"b", "c"=>{"a"=>"a", "b"=>"b", "c"=>{"a"=>"a", "b"=>"b"}}}, "hoho"=>"ooo"}>]>

Hehe.where("test_jsonb @> ?", {'haha': 'aaa'}.to_json)
=> #<ActiveRecord::Relation []>

{% endhighlight %}

搜索 Array

{% highlight ruby %}
Hehe(id: integer, name: string, lala: string, created_at: datetime, updated_at: datetime, test_json: json, test_jsonb: jsonb, test_array_str: string, test_array_int: integer) # 最后两个字段分别是字符串数组和整数数组。

h = Hehe.last
=> #<Hehe id: 2, name: "b", lala: nil, created_at: "2015-03-03 13:33:01", updated_at: "2015-03-03 17:34:18", test_json: nil, test_jsonb: nil, test_array_str: ["ni", "hao", "a"], test_array_int: [0, 1, 2, 3]>

# 通过数组中的单个元素搜索
Hehe.where("'ni' = ANY (test_array_str)")
=> #<ActiveRecord::Relation [#<Hehe id: 2, name: "b", lala: nil, created_at: "2015-03-03 13:33:01", updated_at: "2015-03-03 17:34:18", test_json: nil, test_jsonb: nil, test_array_str: ["ni", "hao", "a"], test_array_int: [0, 1, 2, 3]>]>

# 通过数组中的多个元素进行搜索
Hehe.where("test_array_str @> ARRAY[?]::varchar[]", %w(ni hao))
=> #<ActiveRecord::Relation [#<Hehe id: 2, name: "b", lala: nil, created_at: "2015-03-03 13:33:01", updated_at: "2015-03-03 17:34:18", test_json: nil, test_jsonb: nil, test_array_str: ["ni", "hao", "a"], test_array_int: [0, 1, 2, 3]>]>

# 整数数组的搜索类似

{% endhighlight %} 
