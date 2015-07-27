---
layout: post
title:  js常见面试题
category : 技术
tag : javascript
kw : js面试题,面试题,javascript,javascript面试
---


列出几道js面试中的题,无意中在什么站点看到的，便记录一下。看完这些题后 你还敢说js很简单么,简直是最优雅 博大精深的语言了。

**1、函数声明优先于var**

 {% highlight javascript %}
	var a;
	function a() { 
	}
	alert(typeof a); // ?
 {% endhighlight %}

**2、this scope  arguments scope**

 {% highlight javascript %}
		var length = 10
		function fn(){
			alert(this.length)
		}
		var obj = {
			length: 5,
			method: function(fn) {
				fn() // ?
			    arguments[0]() // ?
		    }
		}
		obj.method(fn)
 {% endhighlight %}


**3、函数表达式具名（函数声明同时赋值给另一个变量）或函数声明立即执行时，名仅在该函数内可访问 (不考虑IE 6 7 8)**

 {% highlight javascript %}
	~function() {
	    alert(typeof next) // ?
	    ~function next() {
	        alert(typeof next) // ?
	    }()
	}()
 {% endhighlight %}


**4、隐式的全局变量**

 {% highlight javascript %}
	var a = 1
	function func() {
	    a = b = 2
	}
	func()
	alert(a)
	alert(b) // ?
 {% endhighlight %}

**5、变量声明早于代码运行（Scoping and Hoisting）**

 {% highlight javascript %}
	var uname = 'jack'
	function change() {
	    alert(uname) // ?
	    var uname = 'lily'
	    alert(uname)
	}
	change()
 {% endhighlight %}


还敢说自己JS很NB么

来源于网络、具体在哪记不得了 后来补上出处。由于自己也并非是专业的前端 所以也做错了好几题。