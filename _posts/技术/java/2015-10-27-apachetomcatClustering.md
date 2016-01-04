---
layout: post
title: apache + tomcat实现集群session同步
categories : [技术,java]
tags : [apache,集群,session同步]
kw :  apache,tomcat,apachetomcat,apachetomcat怎么实现集群,tomcat集群session同步
---

 这次试验是用1台Apache2.2 、 2台tomcat服务器 、并使用Tomcat Connectors--mod_jk.so作为传送介质实现负载均衡。








项目之前在单机上跑 切换到集群环境中 发现一堆的问题出现了,当然还有未知没有发现的。

遗留的问题

1. 如果集群节点多了，发布项目的时候用什么方式发布会比较方便
2. 文件存储的问题怎么解决
3. 系统日志怎么才能实现统一
4. 缓存服务怎么实现同步
5. 数据库并发问题 

针对这些问题将在后面的博文中提供解决方案！！！