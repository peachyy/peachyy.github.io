---
layout: post
title: form表单默认回车事件的问题
category : 技术
tag : from
kw : 表单回车,表单默认提交
---


经常写form表单以前没有怎么注意 无意中注意到一些元素组成的默认事件。

在form表单中 目前发现具备以下情况会默认支持回车事件提交表单

1. 所有的表单元素中只有1个text 无论提交按钮是否为type=submit
2. 表单中的提交按钮type=submit

 {% highlight html %}

	//此代码默认就具有了回车提交表单的事件  需要格外小心
	 <form>
        <input type="text" value="" name="name">
        <input type="button" value="button"/>
    </form>

 {% endhighlight %}

如果我们的表单中仅有一个text的话怎么阻止默认回车事件呢?

可以在表单中多加一个隐藏的text

 {% highlight html %}

	 <form>
        <input type="text" value="" name="name">
        <input type="text" style="display: none;">
        <input type="button" value="button"/>
    </form>

 {% endhighlight %}

示例demo 具有默认事件：[http://peachyy.github.io/demos/htmlformenterevent/index.html](http://peachyy.github.io/demos/htmlformenterevent/index.html "http://peachyy.github.io/demos/")

示例demo 不具有默认事件：[http://peachyy.github.io/demos/htmlformenterevent/index.html](http://peachyy.github.io/demos/htmlformenterevent/index2.html "http://peachyy.github.io/demos/")





