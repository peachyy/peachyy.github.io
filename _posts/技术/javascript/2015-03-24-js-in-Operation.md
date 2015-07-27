---
layout: post
title: js中 `in` 运算符
categories : [技术,js,javascript]
tags : [javascript,in运算符,运算符]
---

之前在javascript中很少使用`in`运算符。在开源类库中经常看到这样子的语句。简单的接收一下怎么使用


- in运算符要求第1个（左边的）操作数必须是字符串类型或可以转换为字符串类型的其他类型
- 第2个（右边的）操作数必须是数组或对象
- **第1个操作数的值是第2个操作数的属性名，才会返回true，否则返回false。**

 {% highlight javascript %}
	//声明 了一个user对象
	var user={
		name:'user1',
		age:20,
		sex:1
	};
	//判断name是不是user对象的属性
	var b='name' in user;
	console.info(b);//true;
	b='summary' in user;
	console.info(b);//false;
	
 {% endhighlight %}


**当是数组的时候，当下标存在时候也是返回`true`的。如下面的0,1,2,length 都返回`true`**


 {% highlight javascript %}
	//声明 了一个array对象
	var user=['user1',200,1000,'peach'];
	//判断200是不是user对象的属性
	var b=200 in user;
	console.info(b);//false;
	b=1 in user;
	console.info(b);//true;因为1是user的下标。
	b='length' in user;
	console.info(b);//true;
	
 {% endhighlight %}