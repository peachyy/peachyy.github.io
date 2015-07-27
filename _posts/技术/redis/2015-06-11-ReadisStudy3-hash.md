---
layout: post
title: Redis[三] @Hash 哈希
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

`Redis`的哈希值是字符串字段和字符串值之间的映射，所以他们是表示对象的完美数据类型

在Redis中的哈希值，可存储超过400十亿键值对。

redis 提供了2套操纵  一种是批量  一种是非批量

假设需要存储一个用户信息

**批量操作**

定义一个key为user1的hash 包含属性 name=xstao、age=22、sex=1、password=123 并获取user1中name的属性值
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hmset user1 name xstao age 22 sex 1 password 123
OK
redis 127.0.0.1:6379[1]> hmget user1 name
1) "xstao"
 {% endhighlight %}
获取user1的全部key 、 value、全部的key value 
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hgetall user1
1) "name"
2) "xstao"
3) "age"
4) "22"
5) "sex"
6) "1"
7) "password"
8) "123"
redis 127.0.0.1:6379[1]> hkeys user1
1) "name"
2) "age"
3) "sex"
4) "password"
redis 127.0.0.1:6379[1]> hvals user1
1) "xstao"
2) "22"
3) "1"
4) "123"
 {% endhighlight %}
获取user1这个hash的字段数量
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hlen user1
(integer) 4
 {% endhighlight %}
为hash  user1添加一个字段 并查看添加后的数据结构
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hmset user1 ext1 test
OK
redis 127.0.0.1:6379[1]> hgetall user1
 1) "name"
 2) "xstao"
 3) "age"
 4) "22"
 5) "sex"
 6) "1"
 7) "password"
 8) "123"
 9) "ext1"
10) "test"
 {% endhighlight %}
更新key为user1 sex的值为0 并返回赋值成功后的新值
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hset user1 sex 0
(integer) 0
 {% endhighlight %}
**非批量**

*从命令格式上来看批量都加了`m` 而非批量都没有 比如批量`hmset` 非批量 hset*

定义一个哈希表user2 第一次执行成功 ，第二次执行批量添加失败了。因为hset仅支持单个添加
 {% highlight HTML %}
redis 127.0.0.1:6379[1]> hset user2 name abc
(integer) 1
redis 127.0.0.1:6379[1]> hset user3 name abc age 2
(error) ERR wrong number of arguments for 'hset' command
redis 127.0.0.1:6379[1]>
 {% endhighlight %}

其他一些常用命令命令
 {% highlight HTML %}
//判断user1中name是否存在  返回1表示已存在 返回0标识没有这个name字段key
redis 127.0.0.1:6379[1]> hexists user1 name
(integer) 1
//针对user1 中age(年龄)字段自增2 返回自增后的值
redis 127.0.0.1:6379[1]> hincrby user1 age 2
(integer) 24
//针对user1 中age(年龄)字段浮点数自增2.5 返回自增后的值
redis 127.0.0.1:6379[1]> hincrbyfloat user1 age 2.5
"28.5"
 {% endhighlight %}
