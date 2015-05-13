---
layout: post
title:  "上传图片前显示"
date:   2015-05-13 13:02:34
categories: javascript
---

{% highlight javascript %}
function readURL(input) {

    if (input.files && input.files[0]) {
        var reader = new FileReader();

        reader.onload = function (e) {
            $('#blah').attr('src', e.target.result);
        }

        reader.readAsDataURL(input.files[0]);
    }
}

$("#imgInp").change(function(){
    readURL(this);
});
{% endhighlight %}

{% highlight html %}
<form id="form1" runat="server">
    <input type='file' id="imgInp" />
    <img id="blah" src="" alt="your image" />
</form>
{% endhighlight %}
