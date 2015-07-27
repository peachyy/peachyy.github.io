---
layout: post
title: Redis[五] @Set 无序集合
categories : [技术,Redis,NoSql]
tags : [Redis] 
---


`redis`提供了2种规则的`SET`集合 1、有序集合 2、无序集合。


`sadd` 添加 -- 添加一个set key为set1 添加 a-e 当返回1表示添加成功，返回0或其他表示添加失败。 

 {% highlight HTML %}
redis 127.0.0.1:6379> sadd set1 a
(integer) 1
redis 127.0.0.1:6379> sadd set1 b
(integer) 1
redis 127.0.0.1:6379> sadd set1 c
(integer) 1
redis 127.0.0.1:6379> sadd set1 d
(integer) 1
redis 127.0.0.1:6379> sadd set1 e
(integer) 1
redis 127.0.0.1:6379>
 {% endhighlight %}

`smembers` 查看 这个set1的集合 使用命令 smembers 语法:smembers key 

  添加值一次添加a b c d e ，而返回给我们的value并非是添加顺序的列表。所以这是一个无序列表；结果如下。

 {% highlight HTML %}
redis 127.0.0.1:6379> smembers set1
1) "a"
2) "d"
3) "b"
4) "c"
5) "e"
redis 127.0.0.1:6379>
 {% endhighlight %}

`srem` 删除 --删除set中的元素 命令 srem 语法:`srem set value`

如下试着删除set1中a元素。

 {% highlight HTML %}
redis 127.0.0.1:6379> srem set1 a
(integer) 1
redis 127.0.0.1:6379> smembers set1
1) "d"
2) "b"
3) "c"
4) "e"
redis 127.0.0.1:6379>
 {% endhighlight %}

从结果可以看出执行srem命令后 返回1 同样 表示删除成功，成功删除1个元素。当然可以同时删除多个元素 如`srem set1 b c d `返回的数值是多少就表示成功删除了多少个元素。


 `spop`随机弹出一个元素

  这个命令的效果是删除源集合的一个元素。并将这个元素返回。

  如`spop set1 `

 `sdiff` 取差集 以set1为基准 获取2个集合中 不一样的数据。语法:`sdiff set1 set2` 
  

   {% highlight HTML %}
redis 127.0.0.1:6379> smembers set1
1) "d"
2) "b"
3) "c"
4) "e"
redis 127.0.0.1:6379> sadd set2 a b d f
(integer) 4
redis 127.0.0.1:6379> sdiff set1 set2
1) "e"
2) "c"
redis 127.0.0.1:6379>		
   {% endhighlight %}

 结果打印e c 因为以set1为基准 只有c e 在set2没有。所以差集就是c e

 `sdiffstore ` 获取差集并存储在一个集合中 语法:`sdiffstore  dist set1 set2` 同样是以set1(第一个集合)为基准 返回被存储集合长度 

   {% highlight HTML %}
redis 127.0.0.1:6379> sdiffstore dist set1 set2
(integer) 2
redis 127.0.0.1:6379> smembers dist
1) "e"
2) "c"
redis 127.0.0.1:6379>
   {% endhighlight %}

`sinter` 获取交集 语法 `sinter set1 set2` 以set1(第一个集合)为基准获取2个集合的相同的数据。返回值为 2个集合的交集

`sinterstore` 获取交集并把返回值存储在指定的集合中。语法:`sinterstore dist set1 set2` 返回值为返回新集合的长度

   {% highlight HTML %}
redis 127.0.0.1:6379> sinter set1 set2
1) "d"
2) "b"
redis 127.0.0.1:6379> sinterstore dist2 set1 set2
(integer) 2
redis 127.0.0.1:6379> smembers dist2
1) "b"
2) "d"
redis 127.0.0.1:6379>
   {% endhighlight %}

`sunion` 取并集 语法:sunion set1 set2
 
简而言之就是把2个集合合并，并且排除相同的数据。

`sunion` 取并集 、而且把结果存储在指定的对象中。语法:`sunionstore dist3 set1 set2`

   {% highlight HTML %}
redis 127.0.0.1:6379> sunion set1 set2
1) "f"
2) "b"
3) "d"
4) "c"
5) "a"
6) "e"
   {% endhighlight %}

还有一些常用的命令如下:

`smove` 语法 `smove set1 set b`

将第1个集合里面的指定元素移动到第2个集合中去。注意是移动 也就是说第1个集合中的被移动的数据将会被删除。

`scard` 语法 ·`scard set1`

获取集合元素的个数

`sismember` 语法 `sismember set a`

试着判断 a 是否存在于set中 返回值 1标识存在。0标识不存在

`srandmember` 语法 `srandmember set`

随机返回集合中的一个元素 。注意只是返回并不会移动源集合的数据。