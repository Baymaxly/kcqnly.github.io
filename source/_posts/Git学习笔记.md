---
title: Git学习笔记 
date: 2019-07-06 10:15:47
tags: git 
---
1.在现有目录中初始化Git仓库

```
git init
```

2.对文件的跟踪

```
git add *.c
```


3.提交文件

```
git commit -m "message"
message为对本次提交的说明
```



<!-- more -->

4.掌握仓库当前的状态

```
git status
```


5.查看不同

```
git diff file-name
```



6.查看提交日志

```
git log
```



7.版本回溯

```
git reset --hard HEAD^
HEAD表示当前版本
上一个版本就是HEAD^，上上一个版本就是HEAD^^
当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
```



8.查看命令历史

```
git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
然后再使用git reset --hard 1094a
就可以返回到append GPL的那个版本
```



9.丢弃工作区的修改

```
git checkout -- file
git reset HEAD <file>
```


场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

10.删除文件

```
git rm <file>
如果误删了
使用git checkout -- text.txt 可恢复
```



11.分支

```
git branch hexo 创建分支hexo
git checkout hexo 切换到hexo分支
git checkout -b hexo 创建并切换到hexo分支
git merge hexo 将hexo分支合并到master分支
git branch -d hexo 删除hexo分支
git branch 查看分支
```




