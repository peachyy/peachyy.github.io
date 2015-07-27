---
layout: post
title: Redis[二] @String
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

启动数据库客户端
`redis-cli.exe`

在Redis 数据中目前拥有以下几种数据类型

1. [String 字符串]({{ site.url }}{{ site.baseurl }}/2015/06/01/ReadisStudy2-string/)
2. [Hashes 哈希]({{ site.url }}{{ site.baseurl }}/2015/06/11/ReadisStudy3-hash/)
3. [List   列表]({{ site.url }}{{ site.baseurl }}/2015/06/11/ReadisStudy4-list/)
4. [set    无序集合]({{ site.url }}{{ site.baseurl }}/2015/06/11/ReadisStudy5-set/)
5. [sets   有序集合]({{ site.url }}{{ site.baseurl }}/2015/06/11/ReadisStudy6-sets/)



`String`最常用过的数据类型

 Redis提供一些命令来操作数据库 格式如:`COMMAND KEY_NAME`
 
 设置一个key为cname ,value为123的数据 并获取cname的value
 {% highlight HTML %}
redis 127.0.0.1:6379> set cname 123
OK
redis 127.0.0.1:6379> get cname
"123"
redis 127.0.0.1:6379>
 {% endhighlight %}

删除key为cname的字符串数据 执行了2次`del cname`命令。第一次返回1 代表删除成功，第二次返回0代表删除失败 在redis中执行命令一般情况下如果返回0则 表示执行失败。当然特殊除外  最后在获取key为cname的值 因为被删除了 所以返回`nil`
 {% highlight HTML %}
redis 127.0.0.1:6379> del cname
(integer) 1
redis 127.0.0.1:6379> del cname
(integer) 0
redis 127.0.0.1:6379> get cname
(nil)
redis 127.0.0.1:6379>
 {% endhighlight %}
 其他一些常用命令
 {% highlight HTML %}
//重新定义个Key cname
redis 127.0.0.1:6379> set cname 123
OK
//返回存储在指定键的值的序列化版本。
redis 127.0.0.1:6379> dump cname
"\x00\xc0{\x06\x00\xde\x0f;a\xf5/[*"
//检查该键是否存在 返回1表示存在 0表示不存在
redis 127.0.0.1:6379> exists cname
(integer) 1
//设置cname的过期时间为60秒
redis 127.0.0.1:6379> expire cname 60
(integer) 1
//查看cname的过期剩余时间
redis 127.0.0.1:6379> ttl cname
(integer) 42
redis 127.0.0.1:6379> ttl cname
(integer) 40
//移除cname的过期剩余时间 那么cname将一直存在
redis 127.0.0.1:6379> persist cname
(integer) 1
//移除cname后，查看cname的过期为-1 表示一直存在了
redis 127.0.0.1:6379> ttl cname
(integer) -1
redis 127.0.0.1:6379> ttl cname1
(integer) -1
//查看所有键 可以使用通配符
redis 127.0.0.1:6379> keys c*
1) "cname"
//随机返回一个键
redis 127.0.0.1:6379> randomkey
"cname"
redis 127.0.0.1:6379> randomkey
"cname"
//对cname进行重命名为cname2
redis 127.0.0.1:6379> rename cname cname2
OK
//我们查看了cname2在列表中。说明重命名成功了
redis 127.0.0.1:6379> keys *
1) "money"
2) "name"
3) "cname2"
//重命名不存在的key会出错
redis 127.0.0.1:6379> rename cname55 cname2
(error) ERR no such key
redis 127.0.0.1:6379> renamenx cname55 cname2
(error) ERR no such key
//查看cname2的数据类型
redis 127.0.0.1:6379> type cname2
string
redis 127.0.0.1:6379>
 {% endhighlight %}