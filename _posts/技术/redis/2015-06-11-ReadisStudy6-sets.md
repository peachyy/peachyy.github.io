---
layout: post
title: Redis[六] @sets 有序集合
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

其实redis的有序和无序集合有点类似。差别就在于有序用于一个排序的规则。官方称它为`score` 分数。默认情况下 分数越小越靠前

**zadd** 语法 ZADD key score1 member1 [score2 member2] 

添加一个或者多个成员到有序集合，如果值已经存在于这个集合中就更新这个值得分数。


**zrange** 语法 ZRANGE key start stop [WITHSCORES]  如果加了参数`withscores` 表示 展示列表的时候需要打印值得分数。 

返回指定一个索引的范围 的有序集合。stop为-1 表示返回全部索引的数据。

如下示例 演示了 添加一个myzset1集合。并使用zrange命令 展示所有的myzset1集合列表
 {% highlight HTML %}
redis 127.0.0.1:6379> zadd myzset1 0  a
(integer) 1
redis 127.0.0.1:6379> zadd myzset1 1   b 2 c 3 d
(integer) 3
redis 127.0.0.1:6379> zrange myzset1 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
redis 127.0.0.1:6379>
 {% endhighlight %}


**zrem** 语法:ZREM key member [member ...]

从有序集合中删除一个或多个成员 返回值为删除成功的个数。



**zremrangebyrank** 语法 zremrangebyrank zset 2 3 按索引（下标）

按索引删除



**zremrangebyscore** 语法  zremrangebyscore zset 2 3  按分数、也可以理解为按顺序。

按分数/顺序删除



**zrank**语法 ZRANK key member 

返回有序集合中元素 指定元素的索引

 {% highlight HTML %}
redis 127.0.0.1:6379> zrank myzset1 d
(integer) 1
 {% endhighlight %}



**zrevrange** 语法  zrevrange zset start stop [widthscores]

倒序输出集合



**zcount** 语法 zcount zset startScope stopScope

返回zset集合中 分数区间个数 如：展示了分数为0-5之前的个数

 {% highlight HTML %}
redis 127.0.0.1:6379> zcount myzset1 0 5
(integer) 2
redis 127.0.0.1:6379>
 {% endhighlight %}



**zcard** 语法  zcard myzset1

返回集合中元素的个数


还有很多常用的命令 不再一一列举 。可以到

[http://www.yiibai.com/redis/redis_sorted_sets.html](http://www.yiibai.com/redis/redis_sorted_sets.html "有序集合 教程1")

[//redis.io](//redis.io "redis官方")


查询资料。

