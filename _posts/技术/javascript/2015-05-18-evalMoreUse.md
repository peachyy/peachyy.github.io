---
layout: post
title:  eval的一些特殊用法
category : 技术
tag : javascript
---

**w3c 对`eval`说明和定义：**

该方法只接受原始字符串作为参数，如果 string 参数不是原始字符串，那么该方法将不作任何改变地返回。因此请不要为 eval() 函数传递 String 对象来作为参数。
如果试图覆盖 eval 属性或把 eval() 方法赋予另一个属性，并通过该属性调用它，则 ECMAScript 实现允许抛出一个 EvalError 异常。

`eval`有两种使用方式 一种是*直接调用*，另一个是*间接调用*

**直接调用**
{% highlight javascript %}
	eval("1+1");
	//（调用括号）之前的哪部分不只是由“eval”标识符组成
{% endhighlight %}

**间接调用**
{% highlight javascript %}
	(1 ,eval)('1+1')
	//这是一个完整的其他类型的表达式，由逗号操作符，数字常量，然后才是"eval"标识符组成
{% endhighlight %}

**下面列出一些间接调用的例子**
	{% highlight javascript %}
	(1, eval)('...')
	(eval, eval)('...')
	(1 ? eval : 0)('...')
	(__ = eval)('...')
	var e = eval; e('...')
	(function(e) { e('...') })(eval)
	(function(e) { return e })(eval)('...')
	(function() { arguments[0]('...') })(eval)
	this.eval('...')
	this['eval']('...')
	[eval][0]('...')
	eval.call(this, '...')
	eval('eval')('...')
{% endhighlight %}

原文至[http://www.oschina.net/translate/global-eval-what-are-the-options](http://www.oschina.net/translate/global-eval-what-are-the-options "eval 的一些不为人知道的用法")




