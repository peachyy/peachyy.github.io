---
layout: post
title: 利用selenium做网页自动测试
categories : [技术,selenium,测试]
tags : [selenium] 
---


 最近在做一个简单新浪微博抓取功能,遇到了请求被新浪拦截,正因为如此了解到selenium。

 [selenium](http://docs.seleniumhq.org/ "selenium") 是一个web的自动化测试工具。运用好它能够完成很多自动化测试功能甚至会让你感到很多出乎意料的事情。

 接下来做一件简单而大气的事情（这里使用java语言）,模拟一个chrome浏览器访问 http://www.weibo.com 自动输入微博用户名和密码 自动点击登录,当登录成功后给用户一个反馈。

 
 必须需要一个浏览器驱动包 可以到http://docs.seleniumhq.org/download/下载 需要匹配自己的操作系统 以及浏览器 下载对应的版本。

 比如 我的驱动放在了本地磁盘 E:\\Dev\\devPlugins\\chromedriver.exe 路径。

{% highlight java %}

public class RunApp {

    public static void main(String[] args)throws Exception{
        //创建一个chrome驱动
        ChromeDriverService service = new ChromeDriverService.Builder()
                .usingDriverExecutable(
                        new File(
                                "E:\\Dev\\devPlugins\\chromedriver.exe"))
                .usingAnyFreePort().build();
        service.start();
        WebDriver driver = new RemoteWebDriver(service.getUrl(),
                DesiredCapabilities.chrome());

        //访问weibo.com站点
        driver.get("http://www.weibo.com?t="+System.currentTimeMillis());

        //等待网站是否刷新完成  如果30秒还没有响应则认为超时了
        (new WebDriverWait(driver,30)).until(new ExpectedCondition<Boolean>() {
            public Boolean apply(WebDriver d) {
                //使用域名判断是否跳转到登录页面
                return d.getCurrentUrl().toLowerCase().startsWith("http://weibo.com");
            }
        });
        // 显示搜索结果页面的 title
        System.out.println("打印一下网页的TITLE: " + driver.getTitle());
        //点击用户登录的窗口
        //这里的path可以开发开发人员工具 复制xpath
        WebElement element = driver.findElement(
		  By.xpath("//*[@id=\"pl_login_form\"]/div[2]/div[1]/div/a[2]"));
        element.click();
        //自动输入用户名数据
        WebElement userId=element.findElement(
		  By.xpath("//*[@id=\"loginname\"]"));
        WebElement password=element.findElement(
          By.xpath("//*[@id=\"pl_login_form\"]/div[2]/div[3]/div[2]/div/input"));
        WebElement submit= element.findElement(
          By.xpath("//*[@id=\"pl_login_form\"]/div[2]/div[3]/div[6]/a"));
        //你的微博账号
        userId.sendKeys("22222");
        //你的微博密码
        password.sendKeys("xxxxxxx");
        //触发登录动作
        submit.click();
        System.out.println("点击登录了");
        //等待是否登录成功 超时时间600秒
        (new WebDriverWait(driver, 600)).until(new ExpectedCondition<Boolean>() {
            public Boolean apply(WebDriver d) {
                //根据网页标题来辨别是否登录成功 如果成功后新浪微博标题为 我的首页 ************
                return d.getTitle().startsWith("我的首页");
            }
        });
        System.out.println("登录完成 你可以开始刷微博了   准备获取cookies");
        Collection set= driver.manage().getCookies();

        // 关闭浏览器
       // driver.quit();
        // 关闭 ChromeDriver 接口
       // service.stop();
    }
}


{% endhighlight %}

修改自己的微博账号和密码后，直接使用控制台运行这个程序 就可以自动打开浏览器 自动输入账号密码 自动登录。有时候会出现验证码的情况需要自己输入验证码。
	


 源代码地址:[https://github.com/peachyy/demos/blob/master/selenium-demo.rar](https://github.com/peachyy/demos/blob/master/selenium-demo.rar "https://github.com/peachyy/demos/blob/master/selenium-demo.rar")