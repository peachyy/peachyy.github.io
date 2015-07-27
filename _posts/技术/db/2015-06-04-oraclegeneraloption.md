---
layout: post
title: oracle常用操作记录
categories : [技术,oracle]
tags : [oracle] 
---
有一个表名为tb，字段段名为name，数据类型nchar(20)。
 {% highlight HTML %} 
        假设字段数据为空，则不管改为什么字段类型，可以直接执行
		   alter table tb modify (name nvarchar2(20));
		假设字段有数据，则改为nvarchar2(20)可以直接执行
		  alter table tb modify (name nvarchar2(20));
		假设字段有数据，则改为varchar2(40)执行时会弹出“ORA-01439:要更改数据类型,则要修改的列必须为空”，这时要用下面方法来解决这个问题
		修改原字段名name为name_tmp
		alter table tb rename column name to name_tmp;
		增加一个和原字段名同名的字段name
		alter table tb add name varchar2(40);
		将原字段name_tmp数据更新到增加的字段name
		update tb set name=trim(name_tmp);
		更新完，删除原字段name_tmp
		alter table tb drop column name_tmp;
 {% endhighlight %}
总结：

1、当字段没有数据或者要修改的新类型和原类型兼容时，可以直接modify修改。
2、当字段有数据并用要修改的新类型和原类型不兼容时，要间接新建字段来转移。
