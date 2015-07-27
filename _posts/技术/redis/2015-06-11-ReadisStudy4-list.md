---
layout: post
title: Redis[四] @List 列表
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

http://redisdoc.com/s

`Redis`列表是简单的字符串列表，排序插入顺序。您可以在头部或列表的尾部Redis的列表添加元素。

1、使用`lpush`命令 添加一个list1 拥有元素 1 2 3 并返回list1的长度*注意*取出顺序是**先进后出**

2、并使用`lrange`命令获取list中的指定索引元素 0 -1表示从0开始 到len结束 格式：LRANGE key start stop 

3、`llen`命令取得list1长度 

{% highlight HTML %}
redis 127.0.0.1:6379> lpush list1 1 2 3
(integer) 3
redis 127.0.0.1:6379> lrange list1 0 -1
1) "3"
2) "2"
3) "1"
redis 127.0.0.1:6379> llen list1
(integer) 3
redis 127.0.0.1:6379>
 {% endhighlight %}
这种很笨的先进后出做法确实不够好，那么我想最后添加的最先出来怎么办呢？当然了 效率如此高的redis肯定是ok的。
使用`lpushx`命令 可以添加到当前列表的最前面 输出结果可以看到我们最后添加的4被排到了最前面去了。
{% highlight HTML %}
redis 127.0.0.1:6379> lpushx list1 4
(integer) 4
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "3"
3) "2"
4) "1"
 {% endhighlight %}
根据指定键 指定索引获取列表中的数据 注意 索引是从0开始的。
{% highlight HTML %}
redis 127.0.0.1:6379> lindex list1  0
"4"
 {% endhighlight %}
使用`linsert key before|after oldval newval`命令可以实现向指定key 指定一个元素值得前面 | 后面 插入一个值
如下实现的是 向list1中元素4之前插入a 并遍历元素 
 然后 又向元素4之后插入b 并遍历显示元素
{% highlight HTML %}
redis 127.0.0.1:6379> linsert list1 before 4 a
(integer) 5
redis 127.0.0.1:6379> lrange list1 0 -1
1) "a"
2) "4"
3) "3"
4) "2"
5) "1"
redis 127.0.0.1:6379> linsert list1 after 4 b
(integer) 6
redis 127.0.0.1:6379> lrange list1 0 -1
1) "a"
2) "4"
3) "b"
4) "3"
5) "2"
6) "1"
 {% endhighlight %}

`LPOP key ` 删除列表中的第一个元素 并返回。
{% highlight HTML %}
redis 127.0.0.1:6379> lpop list1
"a"
 {% endhighlight %}
`LREM key count value `从列表中删除元素 count表示删除的个数 value表示删除的需要删除的元素值
如下 先添加b list1列表中就具有3个b 那么我们就执行`lrem list1 2 b`删除2个b。
{% highlight HTML %}
redis 127.0.0.1:6379> linsert list1 after 3 b
(integer) 6
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "b"
3) "3"
4) "b"
5) "2"
6) "1"
redis 127.0.0.1:6379> linsert list1 after 2 b
(integer) 7
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "b"
3) "3"
4) "b"
5) "2"
6) "b"
7) "1"
redis 127.0.0.1:6379> lrem list1 2 b
(integer) 2
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "3"
3) "2"
4) "b"
5) "1"
 {% endhighlight %}

LSET key index value 更新指定索引的值
如下是 为list1中索引为3的重新赋值为b2
{% highlight HTML %}
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "3"
3) "2"
4) "b"
5) "1"
redis 127.0.0.1:6379> lset list1 3 b2
OK
redis 127.0.0.1:6379> lrange list1 0 -1
1) "4"
2) "3"
3) "2"
4) "b2"
5) "1"
 {% endhighlight %}
 ltrim命令可以实现为裁剪指定索引区间的数据
如下是裁剪list1 中1下标——2下标中的数据
{% highlight HTML %}
redis 127.0.0.1:6379> ltrim list1 1 2
OK
redis 127.0.0.1:6379> lrange list1 0 -1
1) "3"
2) "2"
 {% endhighlight %}
rpush为列表添加元素 并实现 **先进先出**的原则 与lpush相反
如下先添加1 2 3 4  返回的顺序也是 1 2 3 4 
{% highlight HTML %}
redis 127.0.0.1:6379> rpush list2 1 2 3 4
(integer) 4
redis 127.0.0.1:6379> lrange list2 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
 {% endhighlight %}

RPOP key取出列表中最后一个元素
如下可知 list2中最后一个元素是4
{% highlight HTML %}
redis 127.0.0.1:6379> rpop list2
"4"
 {% endhighlight %}

RPOPLPUSH source destination  删除最后一个元素的列表，将其附加到另一个列表并返回它
如下list3并没有声明。而是把list2的最后一个元素保存到了list3列表中
{% highlight HTML %}
redis 127.0.0.1:6379> rpoplpush list2 list3
"3"
redis 127.0.0.1:6379> lrange list3 0 -1
1) "3"
 {% endhighlight %}