---
layout: post
title: Redis[一] @环境搭建
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

**Redis**是一个开源，先进的key-value存储数据库。

 `.........`直接进入正题


*注*

 此次Redis说明是在Window上进行的(因为公司配的电脑比较烂，安装虚拟机有点问题)。

资源地址:[http://redis.io/download](http://redis.io/download "Redis下载地址")

下载后解压到一个目录 如我的目录是`D:\redis-2.6\`

解压后的目录结构：

* `bin\`
* `deps\`
* `msvs\`
* `src\`
* `btests\`
* `utils\`
* `redis.conf`

进入`D:\redis-2.6\bin\release\`会有两个`ZIP`压缩包这个编译后的可执行文件 分别针对32位和64系统。解压所对应的操作系统 此处我处理的是64为操作系统 所有我解压`redisbin64.zip` 然后把解压后的文件 放在`D:\redis-2.6\`目录下面 

打开CMD 执行进入`D:\redis-2.6\` 执行 `redis-server.exe redis.conf`

 {% highlight HTML %}
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 2.6.12 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in stand alone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 1996
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

 {% endhighlight %}

**出现这拉风图形说明环境搭建成功了.**

*启动时的参数配置*

我们在启动的时候用到了`redis-server.exe redis.conf`命令  那么编辑`D:\redis-2.6\redis.conf`可以配置一些参数[http://www.cnblogs.com/linjiqin/archive/2013/05/27/3102040.html](http://www.cnblogs.com/linjiqin/archive/2013/05/27/3102040.html "具体参数配置可以参考这里")


end 


