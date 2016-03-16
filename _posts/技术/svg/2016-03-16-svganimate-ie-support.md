---
layout: post
title: 让IE浏览器支持SVG动画
categories : [技术]
tag : svg
---


在IE浏览器上 无论是否=IE11 svg动画元素animate都得不到支持 接下来的事情就是让IE浏览器能够支持svg动画元素 包括 animate、animateTransform、animateMotion、animateColor、set 元素

如果是IE内核的浏览器可以引入以下脚本

在html文档中使用 引入script

{% highlight html %}
	<script type="text/javascript" src="smil.check.js"></script>
{% endhighlight %}

在svg文件中使用

{% highlight xml %}
  <svg>
   <elements..>
    <script type="text/ecmascript" xlink:href="smil.check.js" />
  </svg>
{% endhighlight %}


接下来就由奇迹发生了 在IE中svg动画元素居然可以动了。

经过测试感觉动画的偏移不是特别精确 如果需要特别精确的话 估计这个还是有些问题。

此文参考

   [http://stackoverflow.com/questions/15738752/svg-animation-not-working-ie9-ie10](http://stackoverflow.com/questions/15738752/svg-animation-not-working-ie9-ie10 "http://stackoverflow.com/questions/15738752/svg-animation-not-working-ie9-ie10")
   
另外smil.check.js托管在github地址为:

[https://github.com/FakeSmile/FakeSmile](https://github.com/FakeSmile/FakeSmile "https://github.com/FakeSmile/FakeSmile")
  



