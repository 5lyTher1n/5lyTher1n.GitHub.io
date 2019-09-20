---
title: Ubuntu虚拟机安装Burpsuite_Pro_1.7.36记录
date: 2019-09-19 19:06:36
tags: 
- Sec
- Tools
categories: 
- Sec
- Tools
---
# 起因 #
额，为什么想写这个呢？大概是因为去年安装Burp Suite破解版时费了好多事，感觉好麻烦。这个软件也确实好用~~尽管现在不怎么会用~~，所以以前想要记录下如果换电脑了该怎么安装，昨天在Ubuntu的虚拟机里安装了下，发现，好像也还行，之前没记录，这次就记录下吧。

# 下载安装 #
## Java环境 ##
[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "下载Java环境")。注意需要登录，选择jar文件。下载时选中 Accept License Agreement。

之后敲入如下代码。

	 tar -xzvf jdk-8u191-linux-i586.tar.gz
	 mv jdk1.8.0_191/ /usr/local/lib/jvm
	 cd /usr/local/lib
	 mv jvm jdk1.8
	 export JAVA_HOME=/usr/local/lib/jdk1.8/ 
	 export JRE_HOME=JAVAHOME/jreexportCLASSPATH=.:{JAVA_HOME}/lib:JREHOME/libexportPATH={JAVA_HOME}/bin:$PATH
	 update-alternatives --install /usr/bin/java java /usr/local/lib/jdk1.8/bin/java 1 
	 update-alternatives --install /usr/bin/javac javac /usr/local/lib/jdk1.8/bin/javac 1
	 update-alternatives --set java /usr/local/lib/jdk1.8/bin/java
	 update-alternatives --set javac /usr/local/lib/jdk1.8/bin/javac
	 java -version 

看到
> java -version
> 
> java version "1.8.0_221"

> Java(TM) SE Runtime Environment (build 1.8.0_221-b11)

> Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)

就成功了。
## Burp Suite ##
然后从官网下载Burp Suite。[https://portswigger.net/burp](https://portswigger.net/burp "Burp")。我是下载的Community版的，一是因为，Pro版本免费试用下载需要申请，我昨天晚上用edu学生邮箱申请了，现在还没看到下载链接。或许是我搞得有问题？二是，看了52pojie的帖子好像使用破解补丁后好像没影响。*（见参考链接1评论）*

之后下载破解补丁。

我是之前从52pojie找到的补丁，然后**好像**找到了 原作者发布的补丁，之后比较了校验和。只记得相对来说应该算是比较可靠的，就用了之前备份的补丁。这里用到的是burp-loader-keygen.jar和burpsuite_pro_v1.7.36.jar.然后把*burpsuite_pro_v1.7.36.jar*改名为 **burpsuite_community_v1.7.36.jar**

拉到虚拟机里和Burp放入同一个目录里。

	java -jar burp-loader-keygen.jar

1. 点击`Run` *（可以先修改`License Text`）*
2. 复制 `License` 粘贴入输入框。然后 `Next`
3. 点击 `Manual activation`
4. 复制第二个文本框的内容或者 `Copy Request`，粘贴进 `License`下的文本框 `Activation Requests`里，在下边的框会自动生成内容。粘贴回注册的第三个文本框或者 `Paste response`
5. 然后，应该就OK了。

之后启动也可以用 `java -jar burp-loader-keygen.jar`来启动，或者用条命令。

至此结束。


> 参考链接：
> 
> [Kali破解版burpsuite pro配置](https://www.jianshu.com/p/a4556f12af54)
> 
> [Ubuntu中安装burp suite](https://www.jianshu.com/p/3e7bb41a1464)
> 
> [Burp Suite Pro Loader&Keygen By surferxyz（更新新版，附带v1.7.37原版）](https://www.52pojie.cn/thread-691448-1-1.html )
<!--more-->


# 无关安装的碎碎念 #
行文将至加个私话，把“阅读全文”的设置放在了这节之前。
这款软件真的太棒了，有时在做CTF时，可以重放请求。之前也试过爆破校园Wi-Fi登陆密码。但用的都不太好，可能以后也会记录些学习使用Burp的内容，但看起来网上也不缺，加上现在表达不好。就纯当笔记了，底线大概是我以后回来看还能看懂。
