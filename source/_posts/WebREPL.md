---
title: WebREPL
date: 2019-10-27 20:37:01
thumbnail: /gallery/MicroPython.jpg
tags: 
    - ESP8266 
    - MicroPython
    - 物联网
categories: 物联网
---
WebREPL客户端是MicroPython官方推荐的更新方式，功能很强大，通过网页的方式读取ESP8266/ESP32的文件系统，可以上传文件或者下载开发板中已存在的文件，可以输入指令并实时查看开发板中的输出状态，完全取代串口调试。配置完就再也不用把8266连接到电脑来写代码了，基本上就可以闲置了。

<!--more-->
# 使用官方的WebREPL(以ESP8266为例)
- NodeMCU ESP8266*1
- uPyCraft V1.1
- WIFI
关于ESP8266不能连接校园网的问题，可以选择打开手机热点，或者连接ESP8266发出的wifi信号，之后也会出一篇用树莓派当路由器的文章，喜欢折腾的，也可以试一试。
## 让8266连接网络
```python
def do_connect(essid, password):
    import network 
    wifi = network.WLAN(network.STA_IF)  
    if not wifi.isconnected(): 
        print('connecting to network...')
        wifi.active(True) 
        wifi.connect(essid, password) 
        while not wifi.isconnected():
            pass 
    print('network config:', wifi.ifconfig())
    

do_connect("name","password")
```

## 在repl中打开WebREPL
```bash
>>> import webrepl_setup
WebREPL daemon auto-start status: disabled

Would you like to (E)nable or (D)isable it running on boot?
(Empty line to quit)
> E
To enable WebREPL, you must set password for it
New password: 你的密码
Confirm password: 你的密码
Changes will be activated after reboot
Would you like to reboot now? (y/n) 
```
<b>还得再输入两条语句</b>
![](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/blog/webrepl.png)
这里有两个ip地址，如果你是直接连的ESP8266发出的WIFI信号的话，那么就是上面一个，如果你连的是自己的wifi，那么就是下面一个


## 通过网页访问ESP8266

<b>WebREPL客户端必须与开发板处在同一局域网下，否则无法正常连接</b>
[官方webrepl网址](http://micropython.org/webrepl/)
输入 ws://192.168.1.136:8266
再输入之前设置的密码，就ok了
![](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/blog/webrepl1.png)

## 一些常用指令
我们可以看到WebREPL网页右端有上传和下载文件的按钮，我们可以通过这些按钮来把我们本地写的代码上传到8266，但是上传之后怎么运行呢，怎么删除上传的错误文件？下面就是一些常用的指令
```bash
exec(open('main.py').read())  #执行main.py这个文件

import os
os.remove("test.py")          #删除test.py这个文件
os.listdir()                  #列出主目录下的所有文件
```

# 遗留问题
把连网和打开WebREPL的代码放到boot.py这个文件里，就可以上电自动运行，但是，这样就带来了一个问题，那就是ESP8266的IP会变，每次还得去查IP，也是挺烦的，查了一下官方手册，貌似是没有设置静态IP的方法。。。。。。

害

吐槽一下，官方的页面好丑，最近发现了一个国内的WebREPL，还挺好的，尝试了一下，由于ESP8266内存有限，用不了，准备用ESP32试一试

[EMP-IDE](http://www.1zlab.com/ide/#/)

![EMP-IDE](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/blog/EMP-IDE.png)


# 感谢阅读