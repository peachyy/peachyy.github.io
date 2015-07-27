---
layout: post
title:  浏览器对Notification通知的支持
category : 技术
tag : javascript
kw : html5,Notification,通知,浏览器通知,notification api
---


记得在上一次的项目中需要实现一个BS的IM通信小功能、当新信息到达时 会在当前网站的右下角弹出一小弹框来提示用户有新的信息了。当时是在站内弹出一个类似WINDOW这样的窗口来实现的。其实这也可以满足用户的需求、当客户又提出来如果在浏览其他网站的时候、或者浏览器最小化的时候 我怎么知道是否有新消息呢? 当时认为这就是CS对于BS而言的优势所在。当然问题一直都没有得到改善 直到最近在W3C上看到[http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113/ecma-script-binding.html](http://www.w3.org/TR/2000/REC-DOM-Level-2-Core-20001113/ecma-script-binding.html "w3c")

[http://www.w3.org/TR/notifications/](http://www.w3.org/TR/notifications/)
这篇文章时候、很有自信的说 这种问题已经可以解决了。遗憾的是已经离开了那一家公司。

**Notation**被`W3C`提出是浏览器通知的一个标准，最初的草案叫做`webkitNotifications` 在网上说`Notation`是未来的标准化，目前还有很多浏览器厂商不支持这个标准。根据测试和网上所知 目前只有`Chrome` 、`Opera`支持，相信很快其他浏览器也会被支持，因为真的很有用。比如像人人网、微信网页版 都使用了这种技术，给用户带来了极高的体验。

在chrome console所知 notification拥有一个构造函数 具备以下的一些属性和方法
 {% highlight HTML %}
	Notification {icon: "", tag: "", body: "I'm an enginneer!", lang: "", dir: "auto"…}
	body: "I'm an enginneer!"
	dir: "auto"
	icon: ""
	lang: ""
	onclick: null
	onclose: null
	onerror: null
	onshow: null
	tag: ""
	title: "Hello Notification"
	__proto__: Notification
	close: function close() { [native code] }
	arguments: null
	caller: null
	length: 0
	name: "close"
	__proto__: function Empty() {}
	<function scope>
	constructor: function Notification() { [native code] }
	arguments: null
	caller: null
	length: 1
	name: "Notification"
	permission: "default"
	prototype: Notification
	requestPermission: function requestPermission() { [native code] }
	toString: function toString() { [native code] }
	__proto__: function EventTarget() { [native code] }
	<function scope>
	__proto__: EventTarget
 {% endhighlight %}


构造函数一共有2个参数 一个是title 另一个则是配置项 如下
 
|    属性        |  含义           | 默认值  |
|---------------|:---------------:| ------:|
| dir           | 未知             | auto   |
| lang          | 应该是国际化用的  |   null  |
| body          | 通知内容         |   null  |
| tag           | 标记/ID          |  null  |
| icon          | 图标 异常显示     |  null  |


**Permission查看权限值**

 {% highlight HTML %}
  Notification.permission
  "default"
 {% endhighlight %}

`permission`是一个静态属性 这个属性比较特殊是不可以手动去修改值的，只能用户通过授权提示来控制，他的值一共有3个如下

1. default   拒绝
2. denied    用户不需要通知功能
3. granted   启动通知了

要使用通知 必须要得到用户的授权后才可以使用，使用`Notification.requestPermission()`向用户请求通知授权 会出现一个是否确认授权的确认框，已经启动通知了将被忽略。

 {% highlight javascript %}
		new Notification('notifi',{body:'这是一个优雅的提醒',icon:'http://blog.peachyy.com/public/images/logo.png'});
 {% endhighlight %}

执行代码后 会在右下角弹出一个浏览器级别的通知框

demo地址：[http://cqweclick.github.io/notification.html](http://cqweclick.github.io/notification.html)

