---
title: Git学习笔记
date: 2018-03-06 17:06:06
tags: [Git,github]
---
# 前言

Git的学习笔记

<!--more-->

* 学习教程 ：[Git教程--廖雪峰](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

* ## 安装完git后先登录git

* ### 执行下面命令登录
```javascript
$ git config --global user.name "UserName"
$ git config --global user.email "email@example.com"
```

* ## 创建本地版本库
* 在本地任意位置创建空文件夹，然后打开文件，在文件夹内右键打开`Git Bash Here`执行
```javascript
& git init 
```
* 使该文件夹变成`git`可以管理的仓库（执行命令完后可发现在文件夹中出现一个隐藏的`.git`文件夹）
* 隐藏文件夹查看：打开文件夹-工具-文件夹选项-查看-隐藏文件和文件夹-显示隐藏的文件）
* 将需要管理的文件放到刚才创建的文件夹内

* ## 提交的所有修改放到暂存 git add
```javascript
$ git add gitnote.txt
```


* ## 将所有修改文件文件加到仓库缓存区以执行git add .命令
```javascript
$ git add .
```

* ## 提交修改到分支 git commit
* 一次性把暂存区的所有修改提交到分支，创建git版本库时Git为我们创建了唯一一个master分支
```javascript
$ git commit -m "这里写的是提交的说明，可以随便写"
```


* ## git status 随时查看仓库的当前状态
```javascript
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   gitnote.txt

```


* ## git log  命令显示最近的提交日志 
　　　
* 它会列出所有历史记录，最近的排在最上方，显示提交对象的哈希值，作者、提交日期、和提交说明。
* 按键盘`Q`键退出历史记录列表。
```javascript
$ git log 
commit 78bc094efb61461182a033f88d348a529fa1aa37 (HEAD -> master)
Author: zjun1728 <503280155@qq.com>
Date:   Wed Sep 5 15:56:41 2018 +0800

    123

commit 65b32e2967a92fcaa3e4be7427b730552f864574
Author: zjun1728 <503280155@qq.com>
Date:   Wed Sep 5 15:37:57 2018 +0800

    提交一个文件

```


* commit后面的一串字符是每次提交的commit-id。
* 简化日志输出信息
输出完整的commit-id
```javascript
$ git log --pretty=oneline 
```

* 输出前几位commit-id
```javascript
$ git log --pretty=oneline --abbrev-commit
```
    
* ## 版本回退 
* 已提交时（commit后，但未推送到远程库）
* git reset --hard HEAD^
```javascript
$ git reset --hard HEAD^
HEAD is now at 65b32e2 提交一个文件
```


* 现在显示当前的版本变为`commit`为 `65b32e2967a92fcaa3e4be7427b730552f864574`的版本即上一个版本这时打开文件可
发现文件内的内容变为上一次修改之前的内容

* 上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，往后的某一个版本则写成`HEAD~100`
若撤销回退即回到未回退之前的状态

* #### 回退到指定的版本号
```javascript
$ git reset --hard commit_id 
```


* 找到未回退之前的`commit-id`。即`78bc094efb614`  （版本号不用写全，前5-8位就行，太少不能确定）
```javascript   
$ git reset --hard 78bc0
	HEAD is now at 78bc094 123
```


* 撤销成功

* 查看commit id(版本号)的方法是执行git reflog命令（（历史命令查看），git reflog记录你的每一次命令）
```javascript
$ git reflog
0bc4ae8 (HEAD -> master) HEAD@{0}: commit: 修改文本内容
78bc094 HEAD@{1}: reset: moving to 78bc0
65b32e2 HEAD@{2}: reset: moving to 65b32
65b32e2 HEAD@{3}: reset: moving to HEAD^
78bc094 HEAD@{4}: commit: 123
65b32e2 HEAD@{5}: commit (initial): 提交一个文件
```

* commit: 修改文本内容，是执行git commit -m ""命令的说明，为回退提供参照，

* 命令行窗口未关闭时可以执行`git log` 命令查看

* __未提交时（commit前）当未提交时可以使用git checkout -- file 撤销修改,如:__ 
```javascript
$ git checkout -- gitnote.txt
```


* ##### 包括两种情况

* * 一种是gitnote.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态（也可以手动将文件改回原来状态）；

* * 一种是gitnote.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。


* 把gitnote.txt文件在工作区的修改全部撤销，让这个文件回到最近一次git commit或git add时的状态。


* ## 删除文件 git rm (或者手动在文件夹中删除)
```javascript
	$ rm gitnote.txt
```


* * 若确定删除，则执行`git commit`命令提交删除

* * 若是误删，则执行 git checkout --file 撤销刚才的删除行为（只能恢复文件到最新版本）
```javascript
	$ git checkout -- gitnote_1.txt
```


* ##远程仓库

* 本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

* __创建SSH Key__

* 在用户（Users）目录下（或users子目录下）有.ssh目录 ,且存在id_rsa和id_rsa.pub文件，则无需创建，没有则执行
```javascript
$ ssh-keygen -t rsa -C "youremail@example.com"
```


* youremail@example.com 输入自己的邮箱地址 一直回车使用默认值

* 然后登陆GitHub，打开“Account settings”，“SSH Keys”页面：

* 然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：点“Add Key”，添加Key成功

* #### 添加远程库

* 在github上创建新的仓库 仓库名与本地库保持一致

* 我们可以将这个仓库克隆到本地，也可将已有的本地仓库与之关联

* 克隆到本地时执行：（任意位置）

```javascript
$ git clone git@github.com:zjun728/GitNote.git
```

* 我们本地已有了一个仓库，下面需要将本地库与远程库关联执行：
```javascript
$ git remote add origin git@github.com:zjun728/GitNote.git
```

* 若提示fatal: remote origin already exists 则执行
```javascript
$ git remote rm origin 
```

* 然后再执行
```javascript
git remote add origin git@github.com:zjun728/GitNote.git
```

* 再执行
```javascript
 $ git push -u origin master 
```

* 第一次在master分支推送master分支时，加上了-u参数 Git将本地master分支与远程分支关联起来，
 并将本地分支内容推送到远程master分支上去 以后可简化为  git push 
 
* 如果我们在当前分支推送其他本地分支，需要指定本地分支：
```
& git push origin dev
```

* （初次提交本地分支（master 或者dev等），例如git push origin master操作，并不会定义当前本地分支的远程分支（upstream），
 我们可以通过git push --set-upstream origin master，关联本地master分支的远程分支（upstream），
* 另一个更为简洁的方式是初次push时，加入-u参数，
* 例如git push -u origin master，这个操作在push的同时会指定当前分支的远程分支（upstream）。
* [本段参考](https://segmentfault.com/a/1190000002783245)）
 
* ## 分支
* （在dev分支上执行 add commit push 时只是将当前分支dev的修改内容推送到远程库，其他分支保持原状（不推送到远程库）
 同理，在master分支上执行add commit push操作时也只是执行当前分支的更改其他分支不变）
 
* __ 创建dev 分支__
```javascript
 $ git branch dev 
```
 
* #### 切换到分支
```javascript
$ git checkout dev
```


* 切换到`dev` 分支，在`dev` 分支有一些操作后切换到`master`分支时如果在切换`master`之前在dev分支上有了一些修改没有
 
 `add` 或者 `add` 了，没有 `commit` 这时候切换到`master`分支时在`master`分支上是可以看到在`dev`分支上修改的内容的只
 
 有在dev分支上修改完后，`add` 并且`commit` 后才会在`master`分支上隐藏修改在master分支上是看不到的， 只有将dev分支
 
 合并到master分支后才会看到在dev分支上的修改.

* #### 创建+切换分支（组合命令）
```
$ git checkout -b dev
```

* #### 查看本地当前分支（当前分支前面会标一个*号。）
```
$ git branch
```

* #### 查看远程分支（红色标记）
```
$ git branch -ｒ
```

* __`dev`分支的工作成果合并到`master`分支上 （合并之前先`add` 、`commit`）__
```
$ git merge dev
```

* 用于合并指定分支到当前分支 所以当我们要合并dev分支到master上时要将分支切换到master
 `git log`可以看到分支的合并情况
 
* __合并完成后，就可以放心地删除本地 `dev` 分支了__：
```
$ git branch -d dev
```


* #### 未合并时该命令是不能执行的，如果要强制删除未合并的分支，需要执行大写的`-D`参数命令：
```
$ git branch -D dev
```

* 如果我们删除了本地的分支`dev`而没有删除远程分支`dev`，这时我们再创建一个和删除分支名称一样的本地分支`dev`
 然后 `add`、`commit`、`push`（这时提示我们是第一次提交，需要与远程分支关联）如果我们默认推送到远程分支`dev`,
 这时推送到的是原来的那个没有删除的远程分支`dev`上去的.
  
  
* __删除远程服务器（github）库的分支__
```
$ git push origin -d dev
```

* __dev 分支要完全合并到master分支上（即在dev 分支上commit后再合并到master）才能执行该操作__
```javascript
$  git branch -d dev
warning: not deleting branch 'dev' that is not yet merged to
         'refs/remotes/origin/dev', even though it is merged to HEAD.
error: The branch 'dev' is not fully merged.
If you are sure you want to delete it, run 'git branch -D dev'.
```


* 当在`dev`分支上删除文件`gitnote_3.txt` 而没有`add` ，这时切换到`master`分支上会发现刚才的操作依然存在，在
`master`分支上`add`、`commit` 此时转到`dev`分支上发现刚才删除而没有`add`的操作没有了（`gitnote_3.txt`文件在`dev`分支上又出现了）__这时查看状态发现，没有需要添加到缓存区或提交的东西.__
如下显示：
```javascript
$ git status
On branch dev
nothing to commit, working tree clean
```


* 如果在`dev`分支上修改文件`gitnote.txt` 并且`add`了，这时再转到`master`分支上这时查看状态发现
```javascript
$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   gitnote.txt
```

* 文件依然存在缓存区，查看刚才修改的`gitnote.txt`文件发现，修改的东西保持不变，这时`commit` 然后切换到`dev`会发现
刚才修改的东西没有了，并且查看状态发现，没有任何需要缓存或提交的东西
```javascript
$ git status
On branch dev
nothing to commit, working tree clean
```

* __推送分支dev 到远程库 _git push_ 
第一次推送到远程库时需执行
```javascript
$ git push -u origin dev 
```

* #### 分支冲突解决
当两个分支修改同一个文件的同一处内容（已`commit`），修改的内容不一样时，如当我们在分支`dev`修改`gitnote.txt`文件第一行内容,
`add`、`commit`后再切换到`master`分支，同样修改 `gitnote.txt`文件第一行内容（两次修改的结果不一样）`add`、 `commit` 后 将`dev` 分支合并到`master`
分支时会发生冲突，这个时候我们需要手动解决冲突后再合并
冲突时我们可以 `git status` 查看冲突内容

* __如果我们其他分支master有临时任务，且当前分支dev任务尚未完成时，我们可以执行  __git stash__ 将当前工作存储起来__
```javascript
$ git stash
```

* 这时用__`git status`__查看工作区显示是干净的
__`git stash list`__ 可以查看存储状态

```javascript
$ git stash list
```
如果临时任务完成继续当前分支dev工作时
需要将未完成工作恢复一下，有两个办法：

* * __ 一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除__；

* * __另一种方式是用`git stash pop`，恢复的同时把stash内容也删了__。

* #### 查看远程库信息
```javascript
$ git remote
```

* ####查看更详细的远程库信息：
```javascript
$ git remote -v
```

* ## 标签
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
打标签： 切换到需要打标签的分支执行git tag <name>命令：
```javascript
$ git tag v1.0
```

* __git tag查看所有标签__：
```javascript
$ git tag
```

* __默认标签是打在最新提交的commit上的，如果我们需要在特定的commit上打标签找到历史提交的commit id，然后打上就可以了：__

* 历史commit-id查看方法
```javascript
$ git log --pretty=oneline --abbrev-commit
```


* __如果我们需要在某一次 commit 处打标签__
```javascript
$ git tag v0.9 f52c633
```

* 标签不是按时间顺序列出，而是按字母排序的。

* __可以用git show tagname查看标签信息__。
```javascript
$ git show v0.9
```

* __还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：__
```javascript
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

 * __删除标签
```javascript
$ git tag -d v0.1
```

* __如果要推送某个标签到远程，使用命令git push origin tagname：__
```javascript
$ git push origin v1.0
```

* __一次性推送全部尚未推送到远程的本地标签__：
```javascript
$ git push origin --tags
```

* __删除远程标签，先从本地删除：git tag -d tagname__
```javascript
$ git tag -d v0.9
```

* __然后，从远程删除：git push origin :refs/tags/tagname。__
```javascript
$ git push origin :refs/tags/v0.9
```
