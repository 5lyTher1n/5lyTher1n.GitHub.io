---
title: JarvisOJ_PWN_Level4
date: 2019-11-18 20:23:51
tags:
- Sec
- CTF
- PWN
- JarvisOJ
categories:
- Sec
- CTF
- PWN
---
# 独立解题 #
- check了一下，32位,NX保护开启了。
- IDA 看了下，和level3差不多。
- 但是，没有给指定的库。上一题的操作方法就不能使用了
- ~自然而然的~不知道该怎么做了0.0
# 题解 #

**前排提醒，文章记录很乱，大概率对别人没什么帮助。愿意看就点下面吧，做好右上角的准备**
<!-- more -->
## DynELF ##
知道了一个工具叫做DynELF：
    
作用：处理远程目标时，在未指定库文件的情况下，通过泄露一些libc文件信息，进行枚举对比猜解出所使用的库文件版本之类的信息。

前提：存在可以泄露信息的漏洞。该漏洞可以被反复触发，不会因为一次触发导致崩溃。  

利用：最好是有write函数存在，来输出信息。相比print、puts等函数不会被'\n','\0'干扰。

使用DynELF函数需要先构造一个leak函数，可以泄露地址信息。最后会获得一个实例，可以用lookup方法得到远程目标里想要的函数地址。
## 第一个payload ##
在leak函数里，构造第一个payload = pad + p32(write_addr) + p32(vuln_addr) + p32(1) + p32(addr) + p32(8).

然后调用函数d = DynELF(leak,elf = elf)。获取得到使用的库文件信息。使用d.lookup('system', 'libc')获取system地址。
## 第二个payload ##
没有'/bin/sh'。好像lookup没法找到数据的地址。

这里可以利用read函数，把这个字符串读入到bss段去。是稳定不受干扰的。

构造第二个payload = pad + p32(read_addr) + p32(vuln_addr) + p32(0) + p32(bss_addr) + p32(8).
conn.send(payload)
conn.sendline('/bin/sh')#这里因为sendline会多加一个'\n'所以正好可以配齐8个字节。
## 第三个payload ##
最后的利用了.
payload = pad + p32(system_addr) + p32(0xdeaddead) + p32(bss_addr)
 
看到有些题解，可以只要两个payload就可以了。

把第二个和第三个综合起来。因为read函数是三个参数，可以找是否有段代码是连续3个pop加上一个rtn。找到这个地址替换vuln_addr。之后去掉pad拼接上第三个payload.

详见[Write Up](http://www.mamicode.com/info-detail-1971189.html#7%E3%80%81%5BXMAN%5Dlevel4%EF%BC%88DynELF%E6%B3%84%E9%9C%B2system%E5%9C%B0%E5%9D%80%EF%BC%89)

大致是这样的
payload = pad + p32(read_addr)

payload+= p32(p3t) + p32(0) + p32(bss_addr) + p32(8)

payload+= p32(system_addr) + p32(0xdeaddead）+ p32(bss_addr)

太有趣了~~~


# 总结 #

首先，DynELF我在Ubuntu16.04下用python 3.5.2一直运行不成功，提示
> string argument without an encoding

但是
    d = DynELF(leak, elf = elf)
两个参数一个是函数，一个elf类型。与str和bytes没什么关系啊，跟进到pwntools的python文件里看了看，嗯。。。学艺不精不知道该怎么改。

感觉上不像是我的问题。。。

最后无奈换了python2.7的环境，一下就成功了0.0

然后，记录的时候发现很多东西不敢写，因为，很多东西我不懂。我可能能跟着WP复现出来，也知道整个步骤是什么意思，为什么这样做，之后遇到类似的问题能解决，但是对本身的基础的知识我就不太懂了，甚至包括很多常识性的知识点也是不太明白的。

现在就先刷刷题，然后有个基本的印象，再根据情况哪儿需要恶补才回去补吧。估计，大概率，这份Blog很长时间只能作为记录使用，对别人应该没什么帮助，质量偏差。┑(￣Д ￣)┍
	

**人一我百，人十我万，永不放弃，加油！**