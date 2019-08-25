---
title: 更换电脑前后对原hexo博客的处理  
date: 2019-05-23 21:52:57
tags: hexo 
---
有时我们需要更换电脑或重装系统，这时就需要我们对hexo博客源文件进行备份

如果您不是很会用git,我建议您可以先选择用U盘拷贝，然后有时间的时候再好好学习一下git
### 一、 备份
#### 方法一 U盘拷贝
并不是所有的文件都要拷贝，只需拷贝根目录下的
 ><br>_config.yml
 <br>package.json
 <br>scaffolds/
 <br>source/
 <br>themes/ 
 
#### 方法二 分支管理
<!-- more -->

在本地先初始化git仓库

```
git init
```

关联远程仓库
```
git remote add origin http://github.com/xxxxxx(你hexo博客的地址)
```


然后创建并切换到hexo分支
```
git checkout -b hexo
```

将文件放入暂存区，并推送到远程仓库

![](https://ly-object-1259106193.cos.ap-chengdu.myqcloud.com/hexo/hexo-1.png)

### 二、 安装hexo  
(别忘了安装Git和Node.js)
鼠标右键Git Bash  输入 (以下指令皆在Git bash中输入) 
```
npm install hexo-cli -g
```


### 三、 初始化文件夹

```
hexo init  
```

然后将源文件直接复制过来
方法二的朋友们就把指定分支上的文件clone下来就行
```
git clone -b 分支名 仓库地址
```





### 四、安装相应模块  

```
npm instal
npm install hexo-deployer-git --save //将文章部署到git的模块
```




### 五、进行测试  

```
hexo clean
hexo g
hexo s
```




在浏览器输入http://localhost:4000 就能看到博客了  




### 六、重新部署到GitHub上  
删除原先的SSH
重新生成SSH添加到github就好了

```
git config --global user.name "yourname"
git config --global user.email "youremail"
```

用下面两个指令确认你有没有输对

```
git config user.name
git config user.email
```

生成SSH(一路回车就OK)

```
ssh-keygen -t rsa -C "youremail"
```

# 感谢阅读