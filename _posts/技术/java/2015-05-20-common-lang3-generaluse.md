---
layout: post
title: Apache -Common-lang包常用类 方法
categories : [技术,java]
tags : [common-lang] 
---
ArrayUtils – 用于对数组的操作，如添加、查找、删除、子数组、倒序、元素类型转换等；

BitField – 用于操作位元，提供了一些方便而安全的方法；

BooleanUtils – 用于操作和转换boolean或者Boolean及相应的数组；

CharEncoding – 包含了Java环境支持的字符编码，提供是否支持某种编码的判断；

CharRange – 用于设定字符范围并做相应检查；

CharSet – 用于设定一组字符作为范围并做相应检查；

CharSetUtils – 用于操作CharSet；

CharUtils – 用于操作char值和Character对象；

ClassUtils – 用于对Java类的操作，不使用反射；

ObjectUtils – 用于操作Java对象，提供null安全的访问和其他一些功能；

RandomStringUtils – 用于生成随机的字符串；

SerializationUtils – 用于处理对象序列化，提供比一般Java序列化更高级的处理能力；

StringEscapeUtils – 用于正确处理转义字符，产生正确的Java、JavaScript、HTML、XML和SQL代码；

StringUtils – 处理String的核心类，提供了相当多的功能；

SystemUtils – 在java.lang.System基础上提供更方便的访问，如用户路径、Java版本、时区、操作系统等判断；

Validate – 提供验证的操作，有点类似assert断言；

WordUtils – 用于处理单词大小写、换行等。

{% highlight java %}

	import java.io.File;
	import java.io.FileNotFoundException;
	import java.io.FileOutputStream;
	import java.io.IOException;
	import java.io.InputStream;
	import java.io.OutputStream;
	import java.io.Reader;
	import java.net.URL;
	import java.util.ArrayList;
	import java.util.Arrays;
	import java.util.Calendar;
	import java.util.Collection;
	import java.util.Date;
	import java.util.Iterator;
	import java.util.List;
	
	import org.apache.commons.collections.CollectionUtils;
	import org.apache.commons.fileupload.util.Closeable;
	import org.apache.commons.io.FileUtils;
	import org.apache.commons.io.IOUtils;
	import org.apache.commons.lang3.CharSetUtils;
	import org.apache.commons.lang3.ClassUtils;
	import org.apache.commons.lang3.ObjectUtils;
	import org.apache.commons.lang3.RandomStringUtils;
	import org.apache.commons.lang3.StringEscapeUtils;
	import org.apache.commons.lang3.StringUtils;
	import org.apache.commons.lang3.math.NumberUtils;
	import org.apache.commons.lang3.time.DateFormatUtils;
	import org.apache.commons.lang3.time.DateUtils;
	import org.apache.commons.lang3.time.StopWatch;
	
	
	public class TestStr {

    public static void Commons(){
         //null 和 ""操作~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //判断是否Null 或者 ""
         System.out.println(StringUtils.isEmpty(null));
         //System.out.println(StringUtils.isNotEmpty(null));
         //判断是否null 或者 "" 去空格~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         System.out.println(StringUtils.isBlank("  "));
         System.out.println(StringUtils.isNotBlank(null));
         //去空格.Null返回null~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         System.out.println(StringUtils.trim(null));
         //去空格，将Null和"" 转换为Null
         System.out.println(StringUtils.trimToNull(""));
         //去空格，将NULL 和 "" 转换为""
         System.out.println(StringUtils.trimToEmpty(null));
         //可能是对特殊空格符号去除？？
         System.out.println(StringUtils.strip("大家好  啊  \t"));
         //同上，将""和null转换为Null
         System.out.println(StringUtils.stripToNull(" \t"));
         //同上，将""和null转换为""
         System.out.println(StringUtils.stripToEmpty(null));
         //将""或者Null 转换为 ""
         System.out.println(StringUtils.defaultString(null));
         //仅当字符串为Null时 转换为指定的字符串(二参数)
         System.out.println(StringUtils.defaultString("", "df"));
         //当字符串为null或者""时，转换为指定的字符串(二参数)
         System.out.println(StringUtils.defaultIfEmpty(null, "sos"));
         //去空格.去字符~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //如果第二个参数为null去空格(否则去掉字符串2边一样的字符，到不一样为止)
         System.out.println(StringUtils.strip("fsfsdf", "f"));
         //如果第二个参数为null只去前面空格(否则去掉字符串前面一样的字符，到不一样为止)
         System.out.println(StringUtils.stripStart("ddsuuu ", "d"));
         //如果第二个参数为null只去后面空格，(否则去掉字符串后面一样的字符，到不一样为止)
         System.out.println(StringUtils.stripEnd("dabads", "das"));
         //对数组没个字符串进行去空格。
         ArrayToList(StringUtils.stripAll(new String[]{" 中华 ", "民 国 ", "共和 "}));
         //如果第二个参数为null.对数组每个字符串进行去空格。(否则去掉数组每个元素开始和结尾一样的字符)
         ArrayToList(StringUtils.stripAll(new String[]{" 中华 ", "民 国", "国共和国"}, "国"));
         //查找,判断~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //判断2个字符串是否相等相等,Null也相等
         System.out.println(StringUtils.equals(null, null));
         //不区分大小写比较
         System.out.println(StringUtils.equalsIgnoreCase("abc", "ABc"));
         //查找，不知道怎么弄这么多查找，很多不知道区别在哪？费劲~~~~~~~~~~~~~~~~~~~
         //普通查找字符，如果一参数为null或者""返回-1
         System.out.println(StringUtils.indexOf(null, "a"));
         //从指定位置(三参数)开始查找，本例从第2个字符开始查找k字符
         System.out.println(StringUtils.indexOf("akfekcd中华", "k", 2));
         //未发现不同之处
         System.out.println(StringUtils.ordinalIndexOf("akfekcd中华", "k", 2));
         //查找,不区分大小写
         System.out.println(StringUtils.indexOfIgnoreCase("adfs", "D"));
         //从指定位置(三参数)开始查找,不区分大小写
         System.out.println(StringUtils.indexOfIgnoreCase("adfs", "a", 3));
         //从后往前查找
         System.out.println(StringUtils.lastIndexOf("adfas", "a"));
         //未理解,此结果为2
         System.out.println(StringUtils.lastIndexOf("d饿abasdafs我", "a", 3));
         //未解,此结果为-1
         System.out.println(StringUtils.lastOrdinalIndexOf("yksdfdht", "f", 2));
         //从后往前查，不区分大小写
         System.out.println(StringUtils.lastIndexOfIgnoreCase("sdffet", "E"));
         //未解,此结果为1
         System.out.println(StringUtils.lastIndexOfIgnoreCase("efefrfs看", "F" , 2));
         //检查是否查到，返回boolean,null返回假
         System.out.println(StringUtils.contains("sdf", "dg"));
         //检查是否查到，返回boolean,null返回假,不区分大小写
         System.out.println(StringUtils.containsIgnoreCase("sdf", "D"));
         //检查是否有含有空格,返回boolean
         System.out.println(StringUtils.containsWhitespace(" d"));
         //查询字符串跟数组任一元素相同的第一次相同的位置
         System.out.println(StringUtils.indexOfAny("absfekf", new String[]{"f", "b"}));
         //查询字符串中指定字符串(参数二)出现的次数
         System.out.println(StringUtils.indexOfAny("afefes", "e"));
         //查找字符串中是否有字符数组中相同的字符，返回boolean
         System.out.println(StringUtils.containsAny("asfsd", new char[]{'k', 'e', 's'}));
         //未理解与lastIndexOf不同之处。是否查到，返回boolean
         System.out.println(StringUtils.containsAny("啡f咖啡", "咖"));
         //未解
         System.out.println(StringUtils.indexOfAnyBut("seefaff", "af"));
         //判断字符串中所有字符，都是出自参数二中。
         System.out.println(StringUtils.containsOnly("中华华", "华"));
         //判断字符串中所有字符，都是出自参数二的数组中。
         System.out.println(StringUtils.containsOnly("中华中", new char[]{'中', '华'}));
         //判断字符串中所有字符，都不在参数二中。
         System.out.println(StringUtils.containsNone("中华华", "国"));
         //判断字符串中所有字符，都不在参数二的数组中。
         System.out.println(StringUtils.containsNone("中华中", new char[]{'中', '达'}));
         //从后往前查找字符串中与字符数组中相同的元素第一次出现的位置。本例为4
         System.out.println(StringUtils.lastIndexOfAny("中国人民共和国", new String[]{"国人", "共和"}));
         //未发现与indexOfAny不同之处  查询字符串中指定字符串(参数二)出现的次数
         System.out.println(StringUtils.countMatches("中国人民共和中国", "中国"));
         //检查是否CharSequence的只包含Unicode的字母。空将返回false。
         //一个空的CharSequence（长（）= 0）将返回true
         System.out.println(StringUtils.isAlpha("这是干什么的2"));
         //检查是否只包含Unicode的CharSequence的字母和空格（''）。
         //空将返回一个空的CharSequence假（长（）= 0）将返回true。
         System.out.println(StringUtils.isAlphaSpace("NBA直播 "));
         //检查是否只包含Unicode的CharSequence的字母或数字。空将返回false。
         //一个空的CharSequence（长（）= 0）将返回true。
         System.out.println(StringUtils.isAlphanumeric("NBA直播"));
         //如果检查的Unicode CharSequence的只包含字母，数字或空格（''）。
         //空将返回false。一个空的CharSequence（长（）= 0）将返回true。
         System.out.println(StringUtils.isAlphanumericSpace("NBA直播"));
         //检查是否只包含ASCII可CharSequence的字符。空将返回false。
         //一个空的CharSequence（长（）= 0）将返回true。
         System.out.println(StringUtils.isAsciiPrintable("NBA直播"));
         //检查是否只包含数值。
         System.out.println(StringUtils.isNumeric("NBA直播"));
         //检查是否只包含数值或者空格
         System.out.println(StringUtils.isNumericSpace("33 545"));
         //检查是否只是空格或""。
         System.out.println(StringUtils.isWhitespace(" "));
         //检查是否全是英文小写。
         System.out.println(StringUtils.isAllLowerCase("kjk33"));
         //检查是否全是英文大写。
         System.out.println(StringUtils.isAllUpperCase("KJKJ"));
         //交集操作~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //去掉参数2字符串中在参数一中开头部分共有的部分，结果为:人民共和加油
         System.out.println(StringUtils.difference("中国加油", "中国人民共和加油"));
         //统计2个字符串开始部分共有的字符个数
         System.out.println(StringUtils.indexOfDifference("ww.taobao", "www.taobao.com"));
         //统计数组中各个元素的字符串开始都一样的字符个数
         System.out.println(StringUtils.indexOfDifference(new String[] {"中国加油", "中国共和", "中国人民"}));
         //取数组每个元素共同的部分字符串
         System.out.println(StringUtils.getCommonPrefix(new String[] {"中国加油", "中国共和", "中国人民"}));
         //统计参数一中每个字符与参数二中每个字符不同部分的字符个数
         System.out.println(StringUtils.getLevenshteinDistance("中国共和发国人民", "共和国"));
         //判断开始部分是否与二参数相同
         System.out.println(StringUtils.startsWith("中国共和国人民", "中国"));
         //判断开始部分是否与二参数相同。不区分大小写
         System.out.println(StringUtils.startsWithIgnoreCase("中国共和国人民", "中国"));
         //判断字符串开始部分是否与数组中的某一元素相同
         System.out.println(StringUtils.startsWithAny("abef", new String[]{"ge", "af", "ab"}));
         //判断结尾是否相同
         System.out.println(StringUtils.endsWith("abcdef", "def"));
         //判断结尾是否相同，不区分大小写
         System.out.println(StringUtils.endsWithIgnoreCase("abcdef", "Def"));
         //字符串截取~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //截取指定位置的字符，null返回null.""返回""
         System.out.println(StringUtils.substring("中国人民", 2));
         //截取指定区间的字符
         System.out.println(StringUtils.substring("中国人民共和国", 2, 4));
         //从左截取指定长度的字符串
         System.out.println(StringUtils.left("说点什么好呢", 3));
         //从右截取指定长度的字符串
         System.out.println(StringUtils.right("说点什么好呢", 3));
         //从第几个开始截取，三参数表示截取的长度
         System.out.println(StringUtils.mid("说点什么好呢", 3, 2));
         //截取到等于第二个参数的字符串为止
         System.out.println(StringUtils.substringBefore("说点什么好呢", "好"));
         //从左往右查到相等的字符开始，保留后边的，不包含等于的字符。本例：什么好呢
         System.out.println(StringUtils.substringAfter("说点什么好呢", "点"));
         //这个也是截取到相等的字符，但是是从右往左.本例结果：说点什么好
         System.out.println(StringUtils.substringBeforeLast("说点什么好点呢", "点"));
         //这个截取同上是从右往左。但是保留右边的字符
         System.out.println(StringUtils.substringAfterLast("说点什么好点呢？", "点"));
         //截取查找到第一次的位置，和第二次的位置中间的字符。如果没找到第二个返回null。本例结果:2010世界杯在
         System.out.println(StringUtils.substringBetween("南非2010世界杯在南非，在南非", "南非"));
         //返回参数二和参数三中间的字符串，返回数组形式
         ArrayToList(StringUtils.substringsBetween("[a][b][c]", "[", "]"));
         //分割~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //用空格分割成数组，null为null
         ArrayToList(StringUtils.split("中华 人民  共和"));
         //以指定字符分割成数组
         ArrayToList(StringUtils.split("中华 ,人民,共和", ","));
         //以指定字符分割成数组，第三个参数表示分隔成数组的长度，如果为0全体分割
         ArrayToList(StringUtils.split("中华 ：人民：共和", "：", 2));
         //未发现不同的地方,指定字符分割成数组
         ArrayToList(StringUtils.splitByWholeSeparator("ab-!-cd-!-ef", "-!-"));
         //未发现不同的地方,以指定字符分割成数组，第三个参数表示分隔成数组的长度
         ArrayToList(StringUtils.splitByWholeSeparator("ab-!-cd-!-ef", "-!-", 2));
         //分割，但" "不会被忽略算一个元素,二参数为null默认为空格分隔
         ArrayToList(StringUtils.splitByWholeSeparatorPreserveAllTokens(" ab   de fg ", null));
         //同上，分割," "不会被忽略算一个元素。第三个参数代表分割的数组长度。
         ArrayToList(StringUtils.splitByWholeSeparatorPreserveAllTokens("ab   de fg", null, 3));
         //未发现不同地方,分割
         ArrayToList(StringUtils.splitPreserveAllTokens(" ab   de fg "));
         //未发现不同地方,指定字符分割成数组
         ArrayToList(StringUtils.splitPreserveAllTokens(" ab   de fg ", null));
         //未发现不同地方,以指定字符分割成数组，第三个参数表示分隔成数组的长度
         ArrayToList(StringUtils.splitPreserveAllTokens(" ab   de fg ", null, 2));
         //以不同类型进行分隔
         ArrayToList(StringUtils.splitByCharacterType("AEkjKr i39:。中文"));
         //未解
         ArrayToList(StringUtils.splitByCharacterTypeCamelCase("ASFSRules234"));
         //拼接~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //也是拼接。未发现区别
         System.out.println(StringUtils.join(getArrayData()));
         //用连接符拼接，为发现区别
         System.out.println(StringUtils.join(getArrayData(), ":"));
         //拼接指定数组下标的开始(三参数)和结束(四参数,不包含)的中间这些元素，用连接符连接
         System.out.println(StringUtils.join(getArrayData(), ":", 1, 3));
         //用于集合连接字符串.用于集合
         System.out.println(StringUtils.join(getListData(), ":"));
         //移除，删除~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //删除所有空格符
         System.out.println(StringUtils.deleteWhitespace(" s 中 你 4j"));
         //移除开始部分的相同的字符
         System.out.println(StringUtils.removeStart("www.baidu.com", "www."));
         //移除开始部分的相同的字符,不区分大小写
         System.out.println(StringUtils.removeStartIgnoreCase("www.baidu.com", "WWW"));
         //移除后面相同的部分
         System.out.println(StringUtils.removeEnd("www.baidu.com", ".com"));
         //移除后面相同的部分，不区分大小写
         System.out.println(StringUtils.removeEndIgnoreCase("www.baidu.com", ".COM"));
         //移除所有相同的部分
         System.out.println(StringUtils.remove("www.baidu.com/baidu", "bai"));
         //移除结尾字符为"\n", "\r", 或者 "\r\n".
         System.out.println(StringUtils.chomp("abcrabc\r"));
         //也是移除，未解。去结尾相同字符
         System.out.println(StringUtils.chomp("baidu.com", "com"));
         //去掉末尾最后一个字符.如果是"\n", "\r", 或者 "\r\n"也去除
         System.out.println(StringUtils.chop("wwe.baidu"));
         //替换~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //替换指定的字符，只替换第一次出现的
         System.out.println(StringUtils.replaceOnce("www.baidu.com/baidu", "baidu", "hao123"));
         //替换所有出现过的字符
         System.out.println(StringUtils.replace("www.baidu.com/baidu", "baidu", "hao123"));
         //也是替换，最后一个参数表示替换几个
         System.out.println(StringUtils.replace("www.baidu.com/baidu", "baidu", "hao123", 1));
         //这个有意识，二三参数对应的数组，查找二参数数组一样的值，替换三参数对应数组的值。
         //本例:baidu替换为taobao。com替换为net
         System.out.println(StringUtils.replaceEach("www.baidu.com/baidu", new String[]{"baidu", "com"}, 
                 new String[]{"taobao", "net"}));
         //同上，未发现不同
         System.out.println(StringUtils.replaceEachRepeatedly("www.baidu.com/baidu", 
                 new String[]{"baidu", "com"}, new String[]{"taobao", "net"}));
         //这个更好，不是数组对应，是字符串参数二和参数三对应替换.(二三参数不对应的话，自己看后果)
         System.out.println(StringUtils.replaceChars("www.baidu.com", "bdm", "qo"));
         //替换指定开始(参数三)和结束(参数四)中间的所有字符
         System.out.println(StringUtils.overlay("www.baidu.com", "hao123", 4, 9));
         //添加，增加~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //复制参数一的字符串，参数二为复制的次数
         System.out.println(StringUtils.repeat("ba", 3));
         //复制参数一的字符串，参数三为复制的次数。参数二为复制字符串中间的连接字符串
         System.out.println(StringUtils.repeat("ab", "ou", 3));
         //如何字符串长度小于参数二的值，末尾加空格补全。(小于字符串长度不处理返回)
         System.out.println(StringUtils.rightPad("海川", 4));
         //字符串长度小于二参数，末尾用参数三补上，多于的截取(截取补上的字符串)
         System.out.println(StringUtils.rightPad("海川", 4, "河流啊"));
         //同上在前面补全空格
         System.out.println(StringUtils.leftPad("海川", 4));
         //字符串长度小于二参数，前面用参数三补上，多于的截取(截取补上的字符串)
         System.out.println(StringUtils.leftPad("海川", 4, "大家好"));
         //字符串长度小于二参数。在两侧用空格平均补全（测试后面补空格优先）
         System.out.println(StringUtils.center("海川", 3));
         //字符串长度小于二参数。在两侧用三参数的字符串平均补全（测试后面补空格优先）
         System.out.println(StringUtils.center("海川", 5, "流"));
         //只显示指定数量(二参数)的字符,后面以三个点补充(参数一截取+三个点=二参数)
         System.out.println(StringUtils.abbreviate("中华人民共和国", 5));
         //2头加点这个有点乱。本例结果: ...ijklmno
         System.out.println(StringUtils.abbreviate("abcdefghijklmno", 12, 10));
         //保留指定长度，最后一个字符前加点.本例结果: ab.f
         System.out.println(StringUtils.abbreviateMiddle("abcdef", ".", 4));
         //转换,刷选~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
         //转换第一个字符为大写.如何第一个字符是大写原始返回
         System.out.println(StringUtils.capitalize("Ddf"));
         //转换第一个字符为大写.如何第一个字符是大写原始返回
         System.out.println(StringUtils.uncapitalize("DTf"));
         //反向转换，大写变小写，小写变大写
         System.out.println(StringUtils.swapCase("I am JianNanCun, Hello"));
         //将字符串倒序排列
         System.out.println(StringUtils.reverse("中国人民"));
         //根据特定字符(二参数)分隔进行反转
         System.out.println(StringUtils.reverseDelimited("中:国:人民", ':'));
         //计算字符串中包含某字符数.
         System.out.println(CharSetUtils.count(
                    "The quick brown fox jumps over the lazy dog.", "aeiou"));
         //删除字符串中某字符
          System.out.println(CharSetUtils.delete(
                    "The quick brown fox jumps over the lazy dog.", "aeiou"));
         //保留字符串中某字符
          System.out.println(CharSetUtils.keep(
                    "The quick brown fox jumps over the lazy dog.", "aeiou"));
         //合并重复的字符.
          System.out.println(CharSetUtils.squeeze("a  bbbbbb     c dd", "b d"));
          //Object为null时，默认打印某字符.
          Object obj = null;
          System.out.println(ObjectUtils.defaultIfNull(obj, "空"));
         //验证两个引用是否指向的Object是否相等,取决于Object的equals()方法.
          Object a = new Object();
          Object b = a;
          Object c = new Object();
          System.out.println(ObjectUtils.equals(a, b));
          System.out.println(ObjectUtils.equals(a, c));
          //用父类Object的toString()方法返回对象信息.
          Date date = new Date();
          System.out.println(ObjectUtils.identityToString(date));
          System.out.println(date);
          //返回类本身的toString()方法结果,对象为null时，返回0长度字符串.
          System.out.println(ObjectUtils.toString(date));
          System.out.println(ObjectUtils.toString(null));
          System.out.println(date);
          //生成指定长度的随机字符串,好像没什么用.
          System.out.println(RandomStringUtils.random(500));
          //在指定字符串中生成长度为n的随机字符串.
          System.out.println(RandomStringUtils.random(5, "abcdefghijk"));
          //指定从字符或数字中生成随机字符串.
          System.out.println(RandomStringUtils.random(5, true, false));
          System.out.println(RandomStringUtils.random(5, false, true));
          //获取类实现的所有接口.
          System.out.println(ClassUtils.getAllInterfaces(Date.class));
          //获取类所有父类.
          System.out.println(ClassUtils.getAllSuperclasses(Date.class));
          //获取简单类名.
          System.out.println(ClassUtils.getShortClassName(Date.class));
          //获取包名.
          System.out.println(ClassUtils.getPackageName(Date.class));
          //判断是否可以转型.
          System.out.println(ClassUtils.isAssignable(Date.class, Object.class));
          System.out.println(ClassUtils.isAssignable(Object.class, Date.class));
          //转换特殊字符.
          System.out.println("html:" + StringEscapeUtils.escapeHtml4(" <html>"));
          System.out.println("html:"+ StringEscapeUtils.unescapeEcmaScript(" "));
          //从数组中选出最大值.
          System.out.println(NumberUtils.max(new int[] { 1, 2, 3, 4 }));
          //判断字符串是否全是整数.
          System.out.println(NumberUtils.isDigits("123.1"));
          //判断字符串是否是有效数字.
          System.out.println(NumberUtils.isNumber("0123.1"));
          //格式化日期输出.
          System.out.println(DateFormatUtils.format(new Date(), "yyyy-MM-dd"));
          System.out.println(DateFormatUtils.format(System.currentTimeMillis(),
                    "yyyy-MM-dd HH:mm:ss"));
          //秒表.
          StopWatch sw = new StopWatch();
          sw.start();
          for (Iterator iterator = DateUtils.iterator(new Date(),
                    DateUtils.RANGE_WEEK_CENTER); iterator.hasNext();)
          {
              Calendar cal = (Calendar) iterator.next();
              System.out.println(DateFormatUtils.format(cal.getTime(),
                        "yy-MM-dd HH:mm"));
           }
           sw.stop();
           System.out.println("秒表计时:" + sw.getTime());

        }

    //将数组转换为List
    private static void ArrayToList(String[] str){
     System.out.println(Arrays.asList(str) + " 长度:" + str.length);
    }

    //获得集合数据
    private static List getListData(){
     List list = new ArrayList();
     list.add("1");
     list.add(null);
     list.add("2");
     list.add("3");
     return list;
    }

    //获得数组数据 
    private static String[] getArrayData(){
     return (String[]) getListData().toArray(new String[0]);
    }

    // 集合并集
     public static Collection<?> union(Collection<?> a, Collection<?> b) {
      return CollectionUtils.union(a, b);
     }
     // 集合交集
     public static Collection<?> intersection(Collection<?> a, Collection<?> b) {
      return CollectionUtils.intersection(a, b);
     }
     // 交集的补集
     public static Collection<?> disjunction(Collection<?> a, Collection<?> b) {
      return CollectionUtils.disjunction(a, b);
     }
     // 集合相减
     public static Collection<?> subtract(Collection<?> a, Collection<?> b) {
      return CollectionUtils.subtract(a, b);
     }

    //读取文件，将其转换成字节数组
     public static byte[] readFileToByteArray(File file) throws IOException {
      return FileUtils.readFileToByteArray(file);
     }
     //读取文件，将其转换成字符串
     public static String readFileToString(File file) throws IOException {
      return FileUtils.readFileToString(file);
     }
     //该方法是用data内容替换了file文件中原来的内容
     public static void writeStringToFile(File file, String data)
       throws IOException {
      FileUtils.writeStringToFile(file, data);
     }
     //将文件srcFile拷贝到desFile中
     public static void copyFile(File srcFile, File desFile) throws IOException {
      FileUtils.copyFile(srcFile, desFile);
     }
     //将文件拷贝到目录下
     public static void copyFileToDirectory(File srcDir, File destDir)
       throws IOException {
      FileUtils.copyDirectory(srcDir, destDir);
     }
     //拷贝URL类型
     public static void copyURLToFile(URL source, File destination)
       throws IOException {
      FileUtils.copyURLToFile(source, destination);
     }
     //删除该目录下的所有文件
     public static void delDirectory(File directory) throws IOException {
      FileUtils.deleteDirectory(directory);
     }
     //清除该目录下的所有文件，但目录不删除
     public static void cleanDirectory(File directory) throws IOException {
      FileUtils.cleanDirectory(directory);
     }

    // 读取流信息
     public static List<String> readLines(InputStream input) throws IOException {
      return IOUtils.readLines(input);
     }
     // 读取流信息
     public static List<String> readLines(Reader reader) throws IOException {
      return IOUtils.readLines(reader);
     }
     // 批量写入文件中，并覆盖原有的文本
     public static void writeLines(List<String> lines, File file)
       throws FileNotFoundException, IOException {
      IOUtils.writeLines(lines, null, new FileOutputStream(file));
     }
     //批量写入文件中，并在后面追加
     public static void writeOneLine(List<String> lines, File file)
       throws IOException {
      OutputStream outputStream = new FileOutputStream(file, true);
      IOUtils.writeLines(lines, null, outputStream, "UTF-8");
     }
     //将流对象转换成字符串
     public static String toString(InputStream input) throws IOException {
      return IOUtils.toString(input);
     }

    public static void main(String[] args) {
     Commons();
    }
}
{% endhighlight %}

内容非原创、感谢圈子搜集[http://www.liutime.com/javainfo/1044/](http://www.liutime.com/javainfo/1044/ "http://www.liutime.com/javainfo/1044/")