---
title: BugKu-Web-WriteUp-From-Pro15-to-Pro18
date: 2019-09-27 08:29:06
tags: 
- Sec
- CTF
- Web
- BugKu
categories: 
- Sec
- CTF
- Web题目
- BugKu
---

# Pro15  Flag在index里 #
> [http://123.206.87.240:8005/post/index.php](http://123.206.87.240:8005/post/index.php)

看名字第一反应就是文件包含，但是怎么利用不清楚，试着用file://发现不行。点开链接发现进入[http://123.206.87.240:8005/post/index.php?file=show.php](http://123.206.87.240:8005/post/index.php?file=show.php)。这大概算是证实了要利用文件包含吧。

趁着这个机会去学习了下文件包含的问题。这里可以用filter伪协议来处理，首先进入的网页就是index.php了，但是题目说flag还是在index.php内，可是，找不着啊。。。那试试读下源码0.0

使用诸如include等函数进行文件包含时会忽略后缀名，如果有php代码会直接解析。当没有时，会直接显示出源代码。

很明显index是有php代码的，对应到显示的页面，就是解析完成后的。可以通过编码的形式，使其不被认为是合理代码，比如在[http://123.206.87.240:8005/post/index.php?file=php://filter/read=convert.base64-encode/resource=index.php](http://123.206.87.240:8005/post/index.php?file=php://filter/read=convert.base64-encode/resource=index.php)
使用以Base64编码的形式读取index.php文件，可以直接放出源代码。显示出一堆编码后的字符串，拉到BP里用Decoder可以解码。Flag在注释里：`flag{edulcni_elif_lacol_si_siht}`


<!-- more -->
----------

# Pro16 输入密码查看flag #
> [http://123.206.87.240:8002/baopo/](http://123.206.87.240:8002/baopo/)

看下URL，baopo...大概就有点思路了。。。
然后看进来，没有验证码，只让输入密码。无脑拉BP，从00000到99999进行暴力破解。
发现密码是`13579`
进去后发现Flag： `flag{bugku-baopo-hah}`

----------

# Pro17 点击一百万次 #
> [http://123.206.87.240:9001/test/](http://123.206.87.240:9001/test/)

看了下源码，有个JS记录点击次数。当点击次数大于等于一百万次后，发送POST请求。
那。。。直接用Hackbar发送POST请求。`clicks=1000000`
可以直接看到Flag： `flag{Not_C00kI3Cl1ck3r}`

----------

# Pro18 备份是个好习惯 #
> [http://123.206.87.240:8002/web16/](http://123.206.87.240:8002/web16/)

显示出了一个很长的字符串，看了下是两个字符串拼接的，都扔去MD5解密。提示是空的。然后。。。然后就没然后了，想不到怎么利用备份这个条件，通过备份获得Webshell？目前没法进行交互啊。。。

看WP。备份的话，找bak后缀的文件，在这里找到了index.php.bak
下载后看到php代码：
	
	include_once "flag.php";
	ini_set("display_errors", 0);
	$str = strstr($_SERVER['REQUEST_URI'], '?');
	$str = substr($str,1);
	$str = str_replace('key','',$str);
	parse_str($str);
	echo md5($key1);
	
	echo md5($key2);
	if(md5($key1) == md5($key2) && $key1 !== $key2){
	    echo $flag."取得flag";
	}
	?>

代码流程是：
1. 查找URL里是否有"?",有就提取"?"及之后的内容
2. 过滤掉"？"
3. 顺序查找key，把key过滤成""(空)
4. 解析str，把键值对解析成变量和值
5. 输出key1和key2的md5
6. 当key1和key2的md5相等，但key1和key2的本值不同时，输出flag

那么构造URL呢？
- 首先传输进去key1和key2的值。
对应来看，首先用"？"开头，然后在第三步会过滤掉key，这时可以用"kkeyey"，这样过滤掉key后依然有个key...
- md5相等，但本值不等，有两种方式。
	1.数组，对数组md5加密会返回null
    `http://123.206.87.240:8002/web16/?kkeyey1[]=1&kkeyey2[]=2`
    2.当加密后形成0eXXX的形式，会当成科学计数法，即0乘10的XXX次幂。结果都是0自然是相等的。这里有些例子。`http://123.206.87.240:8002/web16/?kkeyey1=QNKCDZO&kkeyey2=s155964671a
`
    


> 	QNKCDZO
> 	240610708
> 	s878926199a
> 	s155964671a
> 	s214587387a
> 	s214587387a


得到Flag：`Bugku{OH_YOU_FIND_MY_MOMY}`



----------
# 总结 #

整理了几题，觉得越来越好玩了，整理东西也多了起来，因为不会的多了。。。0.0

也好，不会的多了，学了，就会了0.0
哈哈哈，加油吧~

**人一我百，人十我万，永不放弃，加油！**