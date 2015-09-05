---
layout: post
title:  javascript事件冒泡问题
category : 技术
tag : javascript
kw : 事件冒泡,javascript事件,javascript事件冒泡,js事件,js事件冒泡
---



![]({{site.staticUrl1}}/images/2015/09/2015-09-01-eventBubbling-1.png)

一个如上图的DOM结构 首先是table->tr->td->div->span，分别使用jQuery的api对td div span 分别绑定了click事件。代码如下

 {% highlight javascript %}
	$("#join td").click(function(){
        alert("这是td");
    });
    $("#join div").click(function(){
        alert("这是div");
    });
   $("#join .abc").click(function(){
            alert("这是span");
   });
 {% endhighlight %}

 当点击td的时候弹出"这是TD"、点击div的时候弹出"这是DIV","这是TD"、点击span的时候弹出 "这是span","这是DIV","这是TD" 这种现象被称为事件的传播，也叫事件冒泡 事件冒泡通常在使用中不合理的。那么怎么解决呢?

 在W3c中，使用stopPropagation、preventDefault方法可以可以制止事件的默认行为。

 {% highlight javascript %}
    $("#nojoin td").click(function(event){
        alert("这是td  2");
        event.preventDefault();
        event.stopPropagation();
    });
    $("#nojoin div").click(function(event){
        alert("这是div 2 ");
        event.preventDefault();
        event.stopPropagation();
    });
    $("#nojoin .abc").click(function(event){
        alert("这是span 2");
        event.preventDefault();
        event.stopPropagation();
    });

 {% endhighlight %}

嘿 这次实验成功了。得到了我们期望的效果。

使用jQuery事件回调return false 也可以实现相同的效果。从而少些一些代码，何乐而不为呢。

 {% highlight javascript %}
    $("#nojoin td").click(function(event){
        alert("这是td  2");
        return false;
    });
    $("#nojoin div").click(function(event){
        alert("这是div 2 ");
        return false;
    });
    $("#nojoin .abc").click(function(event){
        alert("这是span 2");
        return false;
    });

 {% endhighlight %}

其实jQuery的事件回调中可以通过return false来阻止事件传播。翻阅jQuery源码 只要return false 其实它也是执行了 preventDefault,stopPropagation 这2个方法。

附上jQuery源码

 {% highlight javascript %}
	if ( ret !== undefined ) {
			if ( (event.result = ret) === false ) {
				event.preventDefault();
				event.stopPropagation();
			}
	}
 {% endhighlight %}


本示例演示地址:
[https://cqweclick.github.io/eventBubbling.html](https://cqweclick.github.io/eventBubbling.html)