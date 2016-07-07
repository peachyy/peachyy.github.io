---
layout: post
title: supervisor配置和使用
categories : [技术,linux]
tags : [linux,supervisor]
kw : supervisor,supervisord,supervisorctl
---


 supervisor是一个制作守护进程的工具,用户可以在UNIX系统中监控、管理进程。常用于管理与某个用户或项目相关的进程。

 
###安装
 
 采用源码安装的方式,切换到3.1.3这个版本安装。

 {% highlight HTML %}

	git clone https://github.com/Supervisor/supervisor.git
	cd supervisor
    git checkout 3.1.3
    python setup.py install
	
 {% endhighlight %}

 生成默认的supervisor配置文件

 {% highlight HTML %}
	
	echo_supervisord_conf > /etc/supervisord.conf

 {% endhighlight %}

###修改自定义配置
   
   vim /etc/supervisord.conf 
   
  提供web界面访问需要设置 inet_http_server 

  需要配置一下supervisorctl的serverurl 这个地址必须匹配inet_http_server节点的port

  另外要去掉include的注释 用include的方式来引入supervisor进程, 而不需要每次都去修改 /etc/supervisord.conf,files指定为/etc/supervisor/*.conf,只要在/etc/supervisor目录下.conf结尾的文件都需要被supervisor加载。

 {% highlight HTML %}
    
   [inet_http_server]         ; inet (TCP) server disabled by default
	port=0.0.0.0:9001        ; (ip_address:port specifier, *:port for all iface)
	;username=user              ; (default is no username (open server))
	;password=123 
	 
   
	[supervisorctl]
	;serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
	serverurl=http://127.0.0.1:9001 ; use an http:// url to specify an inet socket

	[include]
	files=/etc/supervisor/*.conf

 {% endhighlight %}

###示例

 1、首先编写一段测试程序,这段代码是从网络上下载的,并非自己编写的。
  
 vim test_http.py 
 
 {% highlight python %}
   
	import sys
	import BaseHTTPServer
	from SimpleHTTPServer import SimpleHTTPRequestHandler
	HandlerClass = SimpleHTTPRequestHandler
	ServerClass = BaseHTTPServer.HTTPServer
	Protocol = "HTTP/1.0"
	
	if __name__ == "__main__":
	    if sys.argv[1:]:
	        port = int(sys.argv[1])
	    else:
	        port = 8000
	
	    server_address = ('0.0.0.0', port)
	    HandlerClass.protocol_version = Protocol
	    httpd = ServerClass(server_address, HandlerClass)
	
	    sa = httpd.socket.getsockname()
	    print "Serving HTTP on", sa[0], "port", sa[1], "..."
	    httpd.serve_forever()
 {% endhighlight %}

 2、编写进程守护.conf文件
  
  vim /etc/supervisor/test_http.conf 

  program：后面进程名称
 
 {% highlight HTML %}
	
	[program:test_http]
	command=python test_http.py 501  ; 被监控的进程路径
	directory=/root/supervisor    ; 执行前先cd到目录去
	priority=1                    ;数字越高，优先级越高
	numprocs=1                    ; 启动几个进程
	autostart=true                ; 随着supervisord的启动而启动
	autorestart=true              ; 自动重启。。当然要选上了
	startretries=10               ; 启动失败时的最多重试次数
	exitcodes=0                   ; 正常退出代码（是说退出代码是这个时就不再重启了吗？待确定）
	stopsignal=KILL               ; 用来杀死进程的信号
	stopwaitsecs=10               ; 发送SIGKILL前的等待时间
	redirect_stderr=true          ; 重定向stderr到stdout
	stdout_logfile=/tmp/log/supervisor.log  ;日志目录

 {% endhighlight %}

###启动并加载配置信息
 
  不输出其他异常信息则表示启动并加载配置成功,接下来可以使用 supervisorctl 查看状态信息。看到一个进程名称为test_http。
  可以在客户端远程访问这个supervisor网页控制台。curl http://127.0.0.1:9001 能看到网页信息就证明是OK的。
  当然可以在远程浏览器访问http://xxxxx:9001 可以启动 停止 甚至是查看日志等功能。

 {% highlight HTML %}

	 supervisord -c /etc/supervisord.conf 
	 supervisorctl status
	 test_http                        RUNNING   pid 16013, uptime 0:14:22
     
 {% endhighlight %}

 
 supervisorctl 常用的命令有 start,restart,stop,stop all,reload等。

 另外还有一个很重要的概览group
 {% highlight HTML %}
	[group:test_gourp];定义分分组 test_gourp
	programs=test_http   ；分组中包含程序 test_http 多个需要使用,分隔。
	priority=1
	log_stderr=true
	logfile_maxbytes=1MB
 {% endhighlight %}

