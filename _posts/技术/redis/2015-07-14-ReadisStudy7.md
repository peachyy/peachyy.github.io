---
layout: post
title: redis客户端 for java
categories : [技术,Redis,NoSql]
tags : [Redis] 
---

当然在数据中直接操作`redis`命令是没有什么价值和意义的，之所以使用她 是让她为我们的应用提供数据存储服务，让客户感受到价值所在。

redis她拥有很多种客户端连接工具 她几乎支持所有语言，比如 `c`,`c#`,`c++`,`Clojure`,`Common Lisp`,`d`,` Java`,`ruby`,`php`....等等。


下面主要描述一下`java`语言对redis的支持。单单只是`java`语言的客户端就有好多种，比如`Jedis`,`lettuce`,`aredis`,`JRedis`等。都是由众多的开发者贡献出来的，将来肯定会越来越多、就当前来说，Jedis lettuce都是比较常用的，其他这么多得客户端连接工具，使用方法都差不多的。所以一般只要能使用一个其他的也是OK的。最喜欢用的还是`Jedis` 所以以下的示例代码中也将会是使用`Jedis`连接数据库。

  maven依赖

 {% highlight java %}
  	<dependency>
		<groupId>redis.clients</groupId> 
		<artifactId>jedis</artifactId>  
		<version>2.0.0</version>	
  	</dependency>
 {% endhighlight %}

帮助类

`DButi.java`
 {% highlight java %}
   public class DButil {
	//在 org.apache.commons.pool.impl.GenericObjectPool 可以看到一些参数的默认值
	//主机IP
	private static final String  HOST="localhost";
	//端口
	private static final Integer PORT=6379;
	//认证密码
	private static final String AUTH_PAS="xstao";
	//最大活动数目
	private static final Integer MAX_ACTIVE =20;
	//指定pool池 最多有多少个状态为idle 
	private static final Integer MAX_IDLE=100;
	//最长等待时间 毫秒
	private static final Integer MAX_WAIT=5000;
	//超时
	private static final Integer TIME_OUT=5000;
	
	private static JedisPool poolInstance=null;
	/***
	 * 获取jedis 池实例对象
	 * @date 2015年7月14日 上午11:38:15
	 * @author xs Tao 
	 * @return
	 */
	public static JedisPool getJedisPoolInstance(){
		if(poolInstance==null){
			//连接redis数据库 的参数对象
			JedisPoolConfig config=new JedisPoolConfig();
			config.setMaxActive(MAX_ACTIVE);
			config.setMaxWait(MAX_WAIT);
			config.setMaxIdle(MAX_IDLE);
			//创建池 拥有多个构造函数
			//如果是默认的端口号  也可以使用默认端口号的构造函数。就连主机也是可以默认的。
			poolInstance=new JedisPool(config, HOST, PORT, TIME_OUT, AUTH_PAS);			
		}
		return poolInstance;
	}
	public static Jedis getJedis(){
		if(poolInstance==null){
			DButil.getJedisPoolInstance();
		}
		//从池连接中 获取一个jedis资源
		Jedis jedis= poolInstance.getResource();
		return jedis;
	}
	/**
	 * 销毁jedis连接
	 * @date 2015年7月14日 上午11:43:03
	 * @author xs Tao 
	 * @param jedis
	 */
	public static void destory(Jedis jedis){
		if(poolInstance!=null && jedis!=null){
			poolInstance.returnResource(jedis);
		}
	}
	}
 {% endhighlight %}

由于redis是使用K V来存储 所以会存在大量的 难以维护的K 一般会定义个生产KEY的类。用来定义系统K的生成规则

 `KeyUtils.java`
 {% highlight java %}
	public class KeyUtils {
		static String UID="demo:uid";
		static String UID_AUTO_NUM="demo-uid-index";
		public static String getUID(String uid){
			return UID.concat(uid);
		}
	}
 {% endhighlight %}

用户实体类

 {% highlight java %}
	public class User implements java.io.Serializable {
		/**
		 * 
		 */
		private static final long serialVersionUID = 1L;
		private String id;
		private String name;
		private String password;
		private String age;
		private String sex;
		/**
		 * @return the id
		 */
		public String getId() {
			return id;
		}
		/**
		 * @param id the id to set
		 */
		public void setId(String id) {
			this.id = id;
		}
		/**
		 * @return the name
		 */
		public String getName() {
			return name;
		}
		/**
		 * @param name the name to set
		 */
		public void setName(String name) {
			this.name = name;
		}
		/**
		 * @return the password
		 */
		public String getPassword() {
			return password;
		}
		/**
		 * @param password the password to set
		 */
		public void setPassword(String password) {
			this.password = password;
		}
		/**
		 * @return the age
		 */
		public String getAge() {
			return age;
		}
		/**
		 * @param age the age to set
		 */
		public void setAge(String age) {
			this.age = age;
		}
		/**
		 * @return the sex
		 */
		public String getSex() {
			return sex;
		}
		/**
		 * @param sex the sex to set
		 */
		public void setSex(String sex) {
			this.sex = sex;
		}
		public Map<String, String> toMap(){
			Map<String, String> map=new HashMap<String, String>();
			map.put("id", this.id);
			map.put("name", this.name);
			map.put("password", this.password);
			map.put("age", this.age);
			map.put("sex", this.sex);
			return map;
		}
	}

 {% endhighlight %}
一个测试类

 `test.java`
 {% highlight java %}
	public class test  {
		public static void main(String[] args) {
			Jedis jedis=DButil.getJedis();
			String uid=String.valueOf(jedis.incr(KeyUtils.UID_AUTO_NUM));
			User user=new User();
			user.setAge("23");
			user.setId(uid);
			user.setName("happy");
			user.setPassword("5757124");
			user.setSex("男");
			jedis.hmset(KeyUtils.getUID(uid),user.toMap());
			System.out.println(jedis.hget(KeyUtils.getUID(uid), "name"));
		}
	}

 {% endhighlight %}

还有一个优秀封装redis客户端的项目 叫做[http://projects.spring.io/spring-data-redis/](http://projects.spring.io/spring-data-redis/ "spring-data-redis") 官方称在将来会兼容所有的redis java语言客户端

示例代码 [https://github.com/peachyy/weclickSrc/tree/master/rs-parent/rs-redis](https://github.com/peachyy/weclickSrc/tree/master/rs-parent/rs-redis)