---
layout: post
title: springMvc支持jsonp适配
categories : [技术,springmvc]
tags : [java,springmvc] 
---

这里测试的是spring mvc4的版本 

定义一个扩展类 必须要加上@ControllerAdvice增加处理
{% highlight java %}
@ControllerAdvice
public class JsonpSupportAdvice extends AbstractJsonpResponseBodyAdvice {
    public JsonpSupportAdvice() {
        //参数包含callback的时候 使用jsonp的反馈形式
		super("callback", "jsonp");
    }
}
{% endhighlight %}

并配置好映射 jsonp --application/javascript
{% highlight xml %}
	<bean
		class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver"
		p:order="0" p:defaultContentType="text/html" p:ignoreAcceptHeader="true"
		p:favorPathExtension="true" p:favorParameter="true" p:parameterName="content">
		<property name="mediaTypes">
			<map>
				<entry key="html" value="text/html" />
				<entry key="pdf" value="application/pdf" />
				<entry key="xsl" value="application/vnd.ms-excel" />
				<entry key="xml" value="application/xml" />
				<entry key="json" value="application/json" />
				<entry key="jsonp" value="application/javascript"/>
			</map>
		</property>
		.......
		.....
{% endhighlight  %}

OK 到这里配置完成 访问请求的时候只需加上callback参数即为jsonp的数据返回格式。		