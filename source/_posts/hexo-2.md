---
title: 在一台电脑上管理两个hexo博客 
thumbnail: /gallery/library.jpg
date: 2019-06-17 10:57:52
tags: hexo 
categories:
- hexo
---
出于个人原因，要在电脑上管理两个博客，这期间也是遇到了不少问题，也顺便把解决方法分享给大家  

## 主要问题
当你生成SSH添加到Github上时
由于之前部署第一个博客时
你可能输入了

```
git config --global user.name "yourname"
git config --global user.email "youremail"
ssh-keygen -t rsa -C "youremail"
```
<!--more-->
这里的global是全局设置，所以再次输入这两条指令，然后生成SSH，会覆盖之前一个博客的SSH，当你再对之前的博客进行hexo d时，就会报错

## 解决方法
### 一、生成新的SSH
先别输入git config...那两个指令
先输入

```
ssh-keygen -t rsa -C "your_new_email" 
//然后gitbash里会这样
Generating public/private rsa key pair.  
 Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): 
 //这里输入新的id_rsa的名字，以便于和之前的id_rsa区分
```


这个新的SSH貌似没有出现在C盘的.ssh文件夹，而是出现在git bash的文件夹，所以你得把那两个id_rsa复制到.ssh文件夹  

然后设置新Github的SSH key（将新的SSH添加到GitHub上）
### 三、添加新的 SSH 密钥到 SSH agent中

```
ssh-add -D 
//如果出现Could not open a connection to your authentication agent
//那就输入ssh-agent bash
ssh-add xxxxxx #旧密钥名称，一般是id_rsa
ssh-add xxxxxx #新创建的密钥名称
```
### 四、取消全局配置

```
git config --global --unset user.name
git config --global --unset user.email
```

然后分别到两个博客文件里的.deploy_git文件夹下面右键git bash
设置用户名和邮箱  
（其实当时处理第一个博客的时候，直接在.deploy_git文件夹下设置user.name和user.email,而不用全局设置，就OK）
```
git config user.name "yourname"
git config user.email "youremail"
```
然后
hexo clean hexo g hexo d 试一试  

>如果出现  
>FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html  
>只需将_config.yml里的deploy部分仓库的地址改为
>https://yourname:yourpassword@github.com/yourname/yourname.github.io.git
(你的GitHub用户名和密码)  

至此就处理好了

# 感谢阅读

