---
layout: post
title: Hbase Shell在secureCRT中退格键问题解决方法
categories : [技术,hbase]
tags : [hbase,secureCRT]
kw : secureCRT hbase shell退格的问题,secureCRT hbase shell不能退格
---

 使用secureCRT操作Hbase Shell的时候 难免会按错字符,当输入出错后 一般会使用BackSpace(退格)键删除字符。很不巧的是BackSpace键不会当做delete命令,这时需要设置一下CRT 。


 勾选下面2个checkbox 
	
  在选项->会话选项->终端->仿真->映射键->其他设置 

- Backspace发送delete(B)
- Delete发送backspace(S) 
	
	
	

