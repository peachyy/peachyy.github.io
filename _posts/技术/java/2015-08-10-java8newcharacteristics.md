---
layout: post
title: java8中的一些新特性
categories : [技术,java]
tags : [java,java8,新特性]
kw : java,java8,java8中的新特性,groovy 
---


最近看了一下`java8`中的新增特性 新添加的东西个人认为并不多，主要是`lambda`表达式 其他的比较少。

因为之前看过`Groovy` 它里面有一种语法叫`闭包/Closure` 而java8中的lambda和Groovy中所谓的闭包非常相似，官方也称lambda表达式也可以叫闭包 难道是相互模仿? 不过`ruby`这些语言的写法确实非常优雅 未来的java语法也会像`ruby`一样么 这个是未知的。至少Groovy的出现 证明是可以这么做的。

**函数的引用**

这里所说的函数 包含普通函数 和构造函数

java8中使用 `::` 关键字来传递方法或者构造函数引用

  示例如下  这里引用了`Integer.valueOf()`方法
{% highlight java %}

public class FunctionDemo {
	public static void main(String[] args) {
		Function<String,Integer> fordate=Integer::valueOf;
		Integer l=fordate.apply("25");
		System.out.println(l);
	}
}
{% endhighlight %}

**Lambda 表达式**
 
其实会了Groovy中的闭包以后 java中Lambda表达式基本差不多。

*语法*:
- 	()->{}
- 	(int x, int y) -> x + y;
- 	(x, y) -> { return x + y; } //显式指明返回值

可见由3部分组成 参数列表，箭头（->），以及一个表达式或语句块

先给出一个demo 在后面会频繁的使用到Lambda表达式 一个foreach循环 只需要一行代码就搞定。是不是又节约了代码了呢
{% highlight java %}
public class LambdaDemo {
	public static void main(String[] args) {
		List<String> list=new ArrayList<>();
		list.add("a1");
		list.add("a2");
		list.add("a3");
		list.add("a4");
		list.add("a5");
		list.add("a6");
		list.add("a7");
		list.forEach(o -> {System.out.println(o);});
		//...
	}
}
{% endhighlight %}

**在接口(interface)中定义默认方法实现**

{% highlight java %}
public interface foo {
	public void sayHell(String hello);
	//默认方法
	default double abs(int a) {
        return Math.abs(a);
    }
	//impl
	public class fooimpl implements foo{
		@Override
		public void sayHell(String hello) {
			System.out.println("say:"+hello);
		}
		public static void main(String[] args) {
			foo f=new fooimpl();
			f.sayHell("fff");
			System.out.println(f.abs(20));

		}
		
	}
}
{% endhighlight %}

**函数式接口**

首先介绍一个注解 `@FunctionalInterface` 用来表示这个接口类是不一般的。这是一个函数式接口 如果你违反了函数式接口的规范 那么编译器就会报错,当然这个不是必须的、为了方便阅读代码尽量应该标识。

*什么是函数式接口呢?*
 
 接口中可以额外定义多个抽象方法、需要注意的是这些抽象方法的修饰签名必须和Object的public一样

举个例子

示例一 **是函数式接口**

{% highlight java %}
	@FunctionalInterface
	public interface ccc {
		void sum();
		//来自父类public的toString方法
		@Override
		String toString();
	}
{% endhighlight %}

示例二 **不是函数式接口** 因为clone不是public 

{% highlight java %}
	@FunctionalInterface
	public interface ccc {
		void sum();
		//来自父类protected的clone方法
		@Override
		Object clone();
	}
{% endhighlight %}

列出java8之前已有的函数接口

1. java.lang.Runnable
2. java.util.concurrent.Callable
3. java.security.PrivilegedAction
4. java.util.Comparator 
5. java.io.FileFilter
6. java.nio.file.PathMatcher
7. java.lang.reflect.InvocationHandler 
8. java.beans.PropertyChangeListener
9. java.awt.event.ActionListener
10. javax.swing.event.ChangeListener

java8新定义的函数式接口 在rt.jar  java.util.function包中

1. java.util.function.Predicate
2. java.util.function.Consumer 
3. java.util.function.Function
4. java.util.function.Supplier
5. java.util.function.UnaryOperator 
6. java.util.function.BinaryOperator 

那么接下来自定义一个简单的函数接口，你会发现与其他的普通java接口的区别莫过于实现具体的方式变了、变得更加简单 要是jdk1.8之前 会使用到匿名类或者写一个实现类 看到这种写法 以前简直就弱爆了。这就是函数式接口的魅力所在。

{% highlight java %}

@FunctionalInterface
//定义一个函数接口ccc
public interface ccc {
	int sum(int x,int y);
	@Override
	String toString();
	//这是一个测试类
	public class cccTest{
		public static void main(String[] args) {
			//一行代码告别匿名类
			ccc target=( x, y)->{ int sum=x+y;return sum;};
			//调用sum方法
			System.out.println(target.sum(9, 4));
		}
	}
}
{% endhighlight %}

 *Predicate 接口 示例* 

当传入的参数为null或者长度=0 或者长度>3 那么返回true 否则返回false

接口里面默认提供几个默认方法

- default Predicate<T> and(Predicate<? super T> other)   追加and条件
-  default Predicate<T> negate()                         从源码来看 应该是取反值得意思 如test()返回true 那么negate返回false 反之
-  default Predicate<T> or(Predicate<? super T> other)   追加or条件 示例有用到
{% highlight java %}
public class PredicateDemo {
	public static void main(String[] args) {
		Predicate<String> lengthgt3=(s)->{return s!=null &&s.length()>3;};
		lengthgt3=lengthgt3.or((s)->{return s==null|| s.length()==0;});
		boolean b=false;
		b=lengthgt3.test(null);
		
		System.out.println(b);
	}
}
{% endhighlight %}



 *Function  接口 示例* 

{% highlight java %}

public class FunctionDemo {
	public static void main(String[] args) {
		Function<String,Integer> fordate=Integer::valueOf;
		Integer l=fordate.apply("25");
		System.out.println(l);
	}
}
{% endhighlight %}

 *Supplier   接口 示例* 实例化对象 

需要注意的是 对象必须要拥有一个无参的构造函数 否则会编译出错。supplier紧紧只有一个get方法
 
{% highlight java %}

public class SupplierDemo {
	public static void main(String[] args) {
		Supplier<User> personSupplier = User::new;
		User u=personSupplier.get();
		System.out.println(u.getClass());
	}
}
{% endhighlight %}



*Consumer   接口 示例* 操作单个对象处理

具有一个默认方法
- default Consumer<T> andThen(Consumer<? super T> after) 
{% highlight java %}
public class ConsumerDemo {
	public static void main(String[] args) {
		User u1=new User("xx1",20);
		User u2=new User("xx2",21);
		Consumer<User> opt=(u)->{System.err.println(u.getName()+"年龄是"+u.getAge());};
		opt.accept(u2);
	}
}

{% endhighlight %}



操作单个数据 感觉用处并不是很大。

*Comparator   接口 示例* 

 比较的时候用得比较多 JDK8中又新增一些方法。以下示例实现了使用compareTo 方法对user对象按照age字段排序
{% highlight java %}
public class ComparatorDemo {
	public static void main(String[] args) {
		User u1=new User("xx1",25);
		User u2=new User("xx2",21);
		List<User> uList=new ArrayList<>();
		uList.add(u1);
		uList.add(u2);
		uList.sort((t ,t1)->{
			return t.getAge().compareTo(t1.getAge());
		});
		for (int i = 0; i < uList.size(); i++) {
			System.out.println(uList.get(i).getName()+"~"+uList.get(i).getAge());
		}
	
	}
}
{% endhighlight %}

还有一些JDK8自带的 函数式接口 不再一一列举 使用方式都差不多一样的。

**Stream序列**

*Stream*接口拥有很多方法。都是很方便操作数据的。如 Filter 、Sort、count、min、max等等。

