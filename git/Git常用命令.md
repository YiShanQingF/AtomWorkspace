# Git 常用命令

![](http://p1.pstatp.com/large/pgc-image/153761776465798a2409144)


```
配置姓名和邮箱
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

### 常用命令


[参考文档](https://www.toutiao.com/i6604018327293526532/)

```
将当前的目录初始化为git仓库
git init
新建一个目录，将其初始化为Git代码库
git init [project-name]
```

```
git add fileName
git add .
将文件提交至暂存区
```

```
git commit -m '提交注释'
将文件提交本地仓库
```

```
git status
查看状态
```

```
git diff fileName
对比文件修改情况
```

```
查看日志
git log
git log –pretty=oneline
git reflog
```


```
回退到上一版本
git reset --hard HEAD^
回退到指定版本
git reset --hard 版本号

撤销工作区修改内容
git checkout -- fileName
```


## 分支
### 分支创建
```
创建并切换 dev 分支
git checkout -b dev
创建 dev 分支
git branch dev
切换 dev 分支
git checkout dev
删除 dev 分支
git branch -d dev
当本地分支有内容没有推送到, 远程绑定的分支时
git branch -D dev
查看本地所有分支
git branch
查看所有分支(包括远程)
git branch -a
```


### 分支合并
```
git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式
把 dev 分支内容合并到当前分支
git merge dev
git merge –no-ff -m “注释” dev
```

### 分支策略
 1. 首先 master 主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的 dev 分支上干活，干完后，比如上要发布，或者说 dev 分支代码稳定后可以合并到主分支 master 上来。
 2. 当 dev 开发分支工作区的开发工作还未完成,修改代码还不能提交, 但是有 bug 需要马上解决.
    	直接从 dev 分支切换 master 分支, 再创建 fix-bug 分支, 会将当前未开发完成的代码,带到新的 fix-bug 分支上.
    	正确做法: 把 dev 分支上未提交的代码 add 再隐藏, 再切换 master 分支, 再创建 fix-bug 分支, 修复 bug 提交代码, 然后将 fix-bug 分支合并 master 分支.
```
	隐藏工作修改内容
	git stash
	查看隐藏区内容
	git stash list
	1.git stash apply恢复，恢复后，stash内容并不删除，你需要使用命令git stash drop来删除。
	2.另一种方式是使用git stash pop,恢复的同时把stash内容也删除了。
	git stash pop
	删除隐藏区内容
	git stash drop
```


### 多人协作


> 从远程库克隆时候，Git自动把本地的master分支和远程的master分支对应起来了，并且远程库的默认名称是origin。

```
查看远程库的信息
git remote
查看远程库的详细信息
git remote –v
```

```
克隆远程仓库到本机
git clone

远程与本地仓库关联
git remote add origin https://github.com/tugenhua0707/testgit.git
第一次推送master分支时，加上了 –u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令(git push origin master)
git push -u origin master
```


#### 拉取分支
```
从远程 dev 分支拉取,并创建本地 dev 分支
git checkout -b dev origin/dev
拉取与本地分支关联的远程分支
git pull
将远程的 dev 分支代码拉取到本地 dev 分支上
git pull origin/dev dev
将本地的 dev 分支与远程的 dev 分支建立连接, 方便拉取代码(git pull) 和推送(git push)
git branch --set-upstream-to=origin/dev dev
```


#### 推送分支
```
推送分支就是把该分支上所有本地提交到远程库中，推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支
git push origin master
git push origin dev
git push
```

#### 删除远程分支
```
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```



#### 冲突问题







