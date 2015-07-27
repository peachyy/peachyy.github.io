---
layout: post
title: mongo[一] @环境搭建
categories : [技术,MongoDB,NoSql]
tags : [MongoDB] 
---

 之前研究过一个Nosql数据库 `redis` 性能真的很好。不用为数据结构的标准规范为烦恼，这也许就是K V结构的真正意义。

 mongo也是一种Nosql数据库 但它是基于文档的方式。听起来感觉比redis更好操作 也是当前nosql 比较优的产品，当然redis也是非常优秀的，客户量也很大，所以在这里记录我对mongo的一些使用经历。

 mongo几乎支持所有的操作系统，Window、Linux、Mac、Solaris 官方比较建议在linux中使用 具体原因不知。为了方便 这里是使用Window。

**下载**

在[https://www.mongodb.org/downloads](https://www.mongodb.org/downloads) 这里使用的版本是2.0.8 for window    附上链接-> http://share.weiyun.com/0d378b0bee389ea583025d9728a5bea0

这是一个ZIP版本,把文件解压到`E:\Dev\` 并把目录名称改为`mongodb`

使用命令行进入 `E:\Dev\mongodb\bin` 目录 也可以把这个加入到计算机的path

加入已经成功把`E:\Dev\mongodb\bin`加入计算机path

 {% highlight HTML %}
E:\Dev\mongodb>mongod --version
db version v2.0.8-rc1-pre-, pdfile version 4.5
Wed Jul 15 17:24:12 git version: 1d3d3ba938b9ac0d627fd8039ad8aee79c6968c4
 {% endhighlight %}

输入命令 mongod --version出现上面的回复。则说明环境安装成功了。

mongod具有很多命令 使用`mongod --help`可以查看到mongod具备的所有命令。下面是命令的详细说明摘自网络

 {% highlight HTML %}
基本配置

--------------------------------------------------------------------------------

--quiet	# 安静输出
--port arg	# 指定服务端口号，默认端口27017
--bind_ip arg	# 绑定服务IP，若绑定127.0.0.1，则只能本机访问，不指定默认本地所有IP
--logpath arg	# 指定MongoDB日志文件，注意是指定文件不是目录
--logappend	# 使用追加的方式写日志
--pidfilepath arg	# PID File 的完整路径，如果没有设置，则没有PID文件
--keyFile arg	# 集群的私钥的完整路径，只对于Replica Set 架构有效
--unixSocketPrefix arg	# UNIX域套接字替代目录,(默认为 /tmp)
--fork	# 以守护进程的方式运行MongoDB，创建服务器进程
--auth	# 启用验证
--cpu	# 定期显示CPU的CPU利用率和iowait
--dbpath arg	# 指定数据库路径
--diaglog arg	# diaglog选项 0=off 1=W 2=R 3=both 7=W+some reads
--directoryperdb	# 设置每个数据库将被保存在一个单独的目录
--journal	# 启用日志选项，MongoDB的数据操作将会写入到journal文件夹的文件里
--journalOptions arg	# 启用日志诊断选项
--ipv6	# 启用IPv6选项
--jsonp	# 允许JSONP形式通过HTTP访问（有安全影响）
--maxConns arg	# 最大同时连接数 默认2000
--noauth	# 不启用验证
--nohttpinterface	# 关闭http接口，默认关闭27018端口访问
--noprealloc	# 禁用数据文件预分配(往往影响性能)
--noscripting	# 禁用脚本引擎
--notablescan	# 不允许表扫描
--nounixsocket	# 禁用Unix套接字监听
--nssize arg (=16)	# 设置信数据库.ns文件大小(MB)
--objcheck	# 在收到客户数据,检查的有效性，
--profile arg	# 档案参数 0=off 1=slow, 2=all
--quota	# 限制每个数据库的文件数，设置默认为8
--quotaFiles arg	# number of files allower per db, requires --quota
--rest	# 开启简单的rest API
--repair	# 修复所有数据库run repair on all dbs
--repairpath arg	# 修复库生成的文件的目录,默认为目录名称dbpath
--slowms arg (=100)	# value of slow for profile and console log
--smallfiles	# 使用较小的默认文件
--syncdelay arg (=60)	# 数据写入磁盘的时间秒数(0=never,不推荐)
--sysinfo	# 打印一些诊断系统信息
--upgrade	# 如果需要升级数据库  * Replicaton 参数

--------------------------------------------------------------------------------

--fastsync	# 从一个dbpath里启用从库复制服务，该dbpath的数据库是主库的快照，可用于快速启用同步
--autoresync	# 如果从库与主库同步数据差得多，自动重新同步，
--oplogSize arg	# 设置oplog的大小(MB)  * 主/从参数

--------------------------------------------------------------------------------

--master	# 主库模式
--slave	# 从库模式
--source arg	# 从库 端口号
--only arg	# 指定单一的数据库复制
--slavedelay arg	# 设置从库同步主库的延迟时间  * Replica set(副本集)选项：

--------------------------------------------------------------------------------

--replSet arg	# 设置副本集名称  * Sharding(分片)选项

--------------------------------------------------------------------------------
--configsvr	# 声明这是一个集群的config服务,默认端口27019，默认目录/data/configdb
--shardsvr	# 声明这是一个集群的分片,默认端口27018
--noMoveParanoia	# 关闭偏执为moveChunk数据保存
 {% endhighlight %}

要启动数据库 必须要为mongo指定一个数据库数据文件的存放的目录 需要使用`mongod --dbpath`命令来完成，在d:\dev\ 创建一个mongodbDATA文件夹 用来存储mongo的数据库文件

{% highlight HTML %}
	mongod --dbpath E:\Dev\mongodbDATA
	Wed Jul 15 17:35:26 [initandlisten] MongoDB starting : pid=9368 port=27017 dbpat
	h=E:\Dev\mongodbDATA 64-bit host=pc-PC
	Wed Jul 15 17:35:26 [initandlisten] db version v2.0.8-rc1-pre-, pdfile version 4
	.5
	Wed Jul 15 17:35:26 [initandlisten] git version: 1d3d3ba938b9ac0d627fd8039ad8aee
	79c6968c4
	Wed Jul 15 17:35:26 [initandlisten] build info: windows sys.getwindowsversion(ma
	jor=6, minor=1, build=7601, platform=2, service_pack='Service Pack 1') BOOST_LIB
	_VERSION=1_42
	Wed Jul 15 17:35:26 [initandlisten] options: { dbpath: "E:\Dev\mongodbDATA" }
	Wed Jul 15 17:35:26 [initandlisten] journal dir=E:/Dev/mongodbDATA/journal
	Wed Jul 15 17:35:26 [initandlisten] recover : no journal files present, no recov
	ery needed
	Wed Jul 15 17:35:26 [initandlisten] waiting for connections on port 27017
	Wed Jul 15 17:35:26 [websvr] admin web console waiting for connections on port 2
	8017
 {% endhighlight %}

到这里 mongo数据库算是启动OK了。
注意控制台最后两句Log 

Wed Jul 15 17:35:26 [initandlisten] waiting for connections on port 27017

Wed Jul 15 17:35:26 [websvr] admin web console waiting for connections on port 28017

数据库连接端口27017 

数据库web控制台端口28017

既然是web应该可以使用浏览器访问试试。http://localhost:28017/ 能访问成功能证明我们的数据库是真的启动成功了。

下面将使用客户端连接数据库试试

新开一个CMD命令行窗口 输入`mongo` 命令 mongodb会自动连接到test数据库 如下所示

{% highlight HTML %}
C:\Users\pc>mongo
MongoDB shell version: 2.0.8-rc1-pre-
connecting to: test
>
 {% endhighlight %}

说明连接数据库服务器成功了。

环境搭建非常简单。需要熟悉一个mongod的一些常用命令 为后续更深入的了解做铺垫。