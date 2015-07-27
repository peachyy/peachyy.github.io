---
layout: post
title: js中parseInt()函数浏览器兼容的问题
categories : [技术,js,javascript]
tags : [兼容性]
---

一般我们在获取到日期的时候格式为`yyyy-mm-dd` 然后要得到日，会使用`var arr=split('-')`函数进行分割 取到天数就是`arr[2]`

当天数`arr[2]`为`08`的时候 `parseInt('08')`在IE浏览器中却返回`0` 在Chrome、firefox中都能正常的得到8。真是奇怪啦，当时郁闷了很久。这是一个很大的坑，所以以后再处理这种问题的时候一定要小心咯。

经过查资料 发现`parseInt(string, radix)`具备2个参数。W3C解释如下：

- string	必需。要被解析的字符串。
- radix	
可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。
如果省略该参数或其值为 0，则数字将以 10 为基础来解析。如果它以 “0x” 或 “0X” 开头，将以 16 为基数。
如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN。

把第2个参数设置为10表示待转换的为10进制数。	所以这个问题解决了。
 {% highlight javascript %}
	var c=parseInt('08',10);
	console.info(c);//8;
	//做到了，兼容现代浏览器，和以前的IE8- 
	
 {% endhighlight %}