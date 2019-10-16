---
title: CentOS7安装jdk12
thumbnail: /gallery/CentOS.jpg
date: 2019-09-13 15:00:29
tags: CentOS
categories: 
- CentOS 
---
# 卸载openjdk
## 查找已安装的java包
```shell
rpm -qa | grep java
```

## 删除已安装的java
```shell
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
```
<!--more-->

# 下载JDK12压缩包
>点击[这里](https://www.oracle.com/technetwork/java/javase/downloads/jdk12-downloads-5295953.html)开始下载

![jdk下载](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/Centos7/jdk%E4%B8%8B%E8%BD%BD.png)

# 安装
## 创建安装目录
```shell
mkdir /usr/local/java/
```

## 上传文件至安装目录
```shell
rz  
```
然后选择要上传的文件
如果有xftp，那就更方便了

## 解压至安装目录

```shell
tar -xzvf jdk-12.0.2-linux-x64-bin.tar.gz -C /usr/local/java/
```

# 设置环境变量
## 打开文件
```shell
vim /etc/profile
```

## 修改文件
按i进入编辑模式
在末尾添加
```shell
export JAVA_HOME=/usr/local/java/jdk12.0.2
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

## 保存更改
>Linux相关保存命令
> 按ESC键 跳到命令模式，然后：
> - :w      保存文件但不退出vi
> - :w file 将修改另外保存到file中，不退出vi
> - :w!     强制保存，不推出vi
> - :wq     保存文件并退出vi
> - :wq!    强制保存文件，并退出vi
> - :q      不保存文件，退出vi
> - :q!     不保存文件，强制退出vi
> - :e!     放弃所有修改，从上次保存文件开始再编辑


```shell
:wq
```

## 使环境变量生效
```shell
source /etc/profile
```

# 添加软链接
```shell
ln -s /usr/local/java/jdk12.0.2/bin/java /usr/bin/java
```

# 检查
```shell
java -version
```
![检查](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/Centos7/java%E7%8E%AF%E5%A2%83.png)

# 删除压缩包
```shell
cd /root
rm -rf jdk-12.0.2-linux-x64-bin.tar.gz
```


