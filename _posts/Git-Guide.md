---
title: Git-Guide
date: 2021-11-01 19:05:48
tags: GIT
---

![](Git-Guide/Git&GitHub-1600161213521.png)

# Git&GitHub

## 零、日常使用

### 1.初始化

```bash
git init

git config --global user.name "yo***ian"

git config --global user.email "1078***992@qq.com"

//多人协作的话，就建立项目级的签名
//git config user.name "yo***ian"
//git config user.email "1078***992@qq.com"

git config --list

//如果之前设置过ssh了此处就直接调到下一模块，若是需要连接多个github,见下文
ssh-keygen -t rsa -C"1078***992@qq.com"

cd ~/.ssh

ls

cat id_rsa.pub

//然后在github操作添加ssh-key
cd ~/.ssh

//如果出现：bash: cd: /home/liuyongjian/.ssh: No such file or directory
//说明没有SSH Key,需要生成
//使用命令：
ssh-keygen -t rsa -C"107***2992@qq.com"
```

![](Git-Guide/2020031365463717.png)

```bash
//查看已经生成了 id_rsa 和 id_rsa.pub文件
cd ~/.ssh
ls
//获取SSH Key
cat id_rsa.pub  
//拷贝秘钥 ssh-rsa开头
复制ssh-rsa至1078***992@qq.com

//匹配github
setttings->SSHandGPGkeys->newsshkey
```



有时候会出现ssh不能用，如果赶得急就换HTTP:

```bash
git remote remove origin//移除ssh库
git remote add origin https://github.com/2997215859/ROI.git
git push origin mqtt
```



![](Git-Guide/F79446CC3731059E68734B68D4C8A4F4.png)

### 2.创建远程库

```bash
git remote -v 查看当前所拥有远程地址别名
git remote add [别名] [远程地址]

//创建分支: 
git branch [分支名]
```



本地创建远程的新分支并push上去：

```bash
git checkout -b my-test  //在当前分支下创建my-test的本地分支分支
git push origin my-test  //将my-test分支推送到远程
git branch --set-upstream-to=origin/my-test //将本地分支my-test关联到远程分支my-test上   
git branch -a //查看远程分支 
```

此时远程分支my-test已经创建好了，并且本地的分支已经关联到远程分支上
本地push代码以后会push到关联的远程分支上。

### 3.提交

```bash
git status
git add .
git checkout [要push的分支名]
git commit -m "本次提交注释"
git push [远程库] [远程分支]
```



提交可能会出现错误：

```bash
ssh: connect to host github.com port 22: Connection timed out fatal: Could not read from remote repository.
```

可以参考以下两个链接进行增加config文件：[链接1](https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794) [链接2](https://blog.csdn.net/MBuger/article/details/70226712)

有时候会出现头指针分离，push不到远程去:

![](Git-Guide/image-20210513202300745.png)

其实可以直接像下面这样提交，就不会使用上面的步骤(**会丢失最新修改**):

```bash
git status
git add .
git checkout [要push的分支名]
git commit -m "本次提交注释"
git push [远程库] [远程分支]
```

### 4. 拉取:

```bash
git checkout [目标分支]
git checkout -f [当前分支]
git pull [远程库] [远程分支]
```

本地pull不了是使用git checkout -f [当前分支]



本地push不了可能是因为网页端修改过内容了，需要拉去远端数据到本地然后merge之后再push同步：

```bash
git fetch origin //获取远程更新
git merge origin/master //把更新的内容合并到本地分支
```



 

[有关git的一些常用命令](https://blog.csdn.net/qq_19674263/article/details/105277244)



### 5.同一台电脑多个密钥绑定多个guthub仓库:

[主](https://blog.csdn.net/weixin_42500714/article/details/101552802)

[次](https://www.cnblogs.com/zhengyan/p/10728527.html)

主要步骤:

```bash
//生成密钥对(指定密钥对名称)
//之前已经存在的就不用这句话了
//ssh-keygen -t rsa -C"1078302992@qq.com" -f ~/.ssh/id_rsa_one
ssh-keygen -t rsa -C"1254342191@qq.com" -f ~/.ssh/id_rsa_two

cd ~/.ssh
ls
id_rsa id_rsa.pub id_rsa.two id_rsa_two.pub known_hosts
cat id_rsa_two.pub

然后在github操作添加ssh-key

//配置密钥对配置文件
touch config
gedit config

# one
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
# two
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_two

```



### 6.windows的git bash

1.六、10的SSH配置

2.进入要上传的文件夹中

3.可以参照四

```bash
git init
git config user.name yongjian
git config user.email 1078302992@qq.com
git remote add [别名] [远程库]
git remote -v
```

4.提交

```bash
git status
git add .
git commit -m "本次提交注释"
git push [远程库] [远程分支]
```

//创建分支: git branch [分支名]

5.拉取:

```bash
git checkout [目标分支]
git checkout -f [当前分支]
git pull [远程库] [远程分支]
```










## 一、版本控制工具应该具备的功能

### 1.协同修改

多人并行不悖的修改服务器端的同一个文件。

### 2.数据备份

不仅保存目录和文件的当前状态，还能够保存每一个提交过的历史状态。

### 3.版本管理

在保存每一个版本的文件信息的时候要做到不保存重复数据，以节约存储空间，提高运行效率。这方便SVN采用的是增量式管理的方式，而Git采取了文件系统快照的方式。

### 4.权限控制

(1)对团队参与开发的人员进行权限控制

(2)对团队外开发者贡献的代码进行审核——Git独有

### 5.历史记录

(1)查看修改人、修改时间、修改内容、日志信息

(2)将本地文件恢复到某一个历史状态

### 6.分支管理

允许开发团队在工作过程中多条生产线同时推进任务，进一步提高效率

## 二、版本控制简介

### 1.版本控制

工程设计领域中使用版本控制管理工程蓝图的设计过程。在IT开发过程中也可以使用版本控制思想管理代码的版本迭代。

### 2.版本控制工具

思想:版本控制

实现:版本控制工具

#### (1)集中式版本控制工具

CVS、SVN、VSS.....

![](Git-Guide/image-20200911150019132.png)

单点故障:服务器一旦宕机，所有资料就没了，客户端上只会存储当时保存的状态

#### (2)分布式版本控制工具

Git、Mercurial、Bazaar、Darcs.........

![](Git-Guide/image-20200911150252557.png)

本地就有完整的历史进行版本控制

## 三、Git简介

### 1.Git简史

![](Git-Guide/image-20200911150436239.png)

### 2.Git官网和Logo

[官网地址](https://git-scm.com/)

Logo:

![](Git-Guide/u=924384885,4203827437&fm=26&gp=0.jpg)

### 3.Git的优势



(1)大部分操作在本地完成，不需要联网

(2)完整性保证

(3)尽可能添加数据而不是删除或修改数据

(4)分支操作非常快捷流畅

(5)与Linux命令全面兼容

### 4.Git的安装

见pdf版or百度

### 5.Git结构

![](Git-Guide/image-20200911151313551.png)

### 6.Git和代码托管中心

代码托管中心的任务:维护远程库

#### (1)局域网环境下

GitLab服务器

#### (2)外网环境下

GitHub

码云

### 7.本地库和远程库

#### (1)团队内部协作

![](Git-Guide/image-20200911151628313.png)

1->push

2->clone

2修改完->push

1再push

#### (2)跨团队协作

![](Git-Guide/image-20200911151815850.png)

## 四.Git命令行操作

### 1.本地库初始化

就是建立本地仓库

(1)命令

```git
git init
```

(2)效果

![](Git-Guide/image-20200911152156818.png)

![](Git-Guide/image-20200911152216074.png)

(3)注意

* .git目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改

### 2.设置签名

(1)形式

	* 用户名: tom
	* Email地址:goodMoring@atguigu.com

(2)作用

* 区分不同开发人员的身份

(3)辨析

* 这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系

(4) 命令

* 项目级别/仓库级别:仅在当前本地库范围内有效

> git config user.name tom_pro
>
> git config user.email goodMornig_pro@atguigu.com
>
> 信息保存的位置:  ./.git/config文件

- 查看git配置文件内容的两种方法:

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20102007.png)

- 系统用户级别:登录当前操作系统的用户范围

> git config --global user.name tom_glb
>
> git config --global goodMorning_pro@atguigu.com
>
> 信息保存位置:  ~/.gitconfig 文件

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20102814.png)

- 级别优先级

> 就近原则:项目级别优先于系统用户级别，二者都有时采用 项目级别的签名
>
> 如果只有系统用户级别的签名，就以系统用户的签名为准
>
> 二者都没有不允许

### 3.基本操作

```git
git status
```

查看工作区、暂存区状态

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20103235.png)

第一行:在主分支上
第二行:没有任何已经提交的内容=本地库里面没有东西
第三行:没有什么可提交的=暂存区没有东西

补充:vim的一些简单命令:

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20103846.png)

```vim
1.一般模式下::set nu 显示行号
2.i 进入插入模式 esc退出插入模式到一般模式
3.提交的话就是退到一般模式，然后 :wq+enter
```

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20104135.png)

第三行说的是一个未追踪的文件，建议使用git add命令提交到暂存区
第四行说的是暂存区没有东西但是有未追踪的文件可以提交
(暂存区的内容是可以commit到本地库的)

#### (2)添加

```git
git add [filename]
```

将工作区的"新建/修改"添加到暂存区

将当前文件夹中的所有文件都添加到暂存区:

```git
git add .
```

从暂存区撤回到工作区:

```git
git rm --cached 文件名
```

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20104806.png)

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20105031.png)

#### (3)提交

```git
git commit -m "commit message" [filename]
```

将暂存区的内容提交到本地库

上面这句话就比git commit [filename],然后自动进入vim编辑器编写备注要方便

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20110411.png)

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20110546.png)

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20110749.png)

![](Git-Guide/%E6%89%B9%E6%B3%A8%202020-09-11%20111109.png)

#### (4)查看历史记录

```git
git log
```

多屏显示控制方式:

	空格向下翻页
	
	b向上翻页
	
	q退出

```git
git log --pretty=oneline
```

![](Git-Guide/image-20200912160727229.png)

```git
git log --oneline
```

![](Git-Guide/image-20200912160807150.png)

只显示一部分hash值

```git
git reflog
```

![](Git-Guide/image-20200912160914878.png)

HEAD@{移动到当前版本需要多少步}

#### (5)前进后退

- 本质

![](Git-Guide/image-20200912161105905.png)

- 基于索引值操作(推荐)

```bash
git reset --hard [局部索引值]
例:git reset --hard 42e7e84
```

- 使用^符号:只能后退

```git
git reset --hard HEAD^
```

注意:一个^表示后退一步，n个表示后退n步

- 使用~符号:只能后退

```git
git reset --hard HEAD~n
```

注意:表示后退n步

#### (6)reset命令的三个参数对比

文件状态:红色是在工作区(untracked)，绿色是在暂存区(staged)

![](Git-Guide/image-20201223144218820.png)

##### a. --soft参数

仅仅在本地库移动HEAD指针

![](Git-Guide/image-20200912173243073.png)

本地库是指向9a9ebe0这个版本，但是本地的文件(工作区)还是6325c55时的内容

![](Git-Guide/image-20200912173406255.png)

暂存区状态:

![](Git-Guide/image-20200912173452805.png)

此时是绿色，也就是有文件有变化,其实是这样的:
![](Git-Guide/image-20200912173552496.png)

soft命令后，本地库指针后退了，显得暂存区工作区往前走了一个，其实是本地库后退了一个,所以回显示绿色，需要从暂存区提交到本地库的状态

此时文件处于staged的状态，也就是暂存区的内容还没有commit到本地库，也就是本地库的指针已经调到指定的版本指针(已经commit过的状态)，但是已经存在的变更在暂存区(staged)和工作区(modified)依旧存在。

##### b. --mixed参数

>在本地库移动HEAD指针
>
>重置暂存区

![](Git-Guide/image-20200912173757209.png)

本地还是没有变。而且暂存区状态变成红色了:

![](Git-Guide/image-20200912173921452.png)

!(Git-Guide/image-20200912172729376.png)

mixed命令后，本地库和暂存区指针都后退了，显得工作区往前走了一个，其实是本地库和暂存区后退了一个:



此时文件处于modified的状态，也就是工作区的内容还没有add到暂存区,也就是本地库的指针已经调到指定的版本指针(已经commit过的状态)，暂存区也已经跟随指针回退到相应的版本，但是已经存在的变更在工作区(modified)依旧存在

##### c.  --hard参数

> 在本地库移动HEAD指针
>
> 重置暂存区
>
> 重置工作区

也就是说，
--hard是不保留所有更改，三个区的文件都切到上一条commit(本地库)的状态

例如你在上次 **commit** 之后又对文件做了一些改动：把修改后的**ganmes.txt**文件**add**到**暂存区**，修改后的**shopping list.txt**保留在工作区

```shell
git status
```



![](Git-Guide/4428238-1c22b16e14586320.png)

最初状态


 然后，执行了**reset**并附上了**--hard**参数：

```shell
git reset --hard HEAD^
```

你的 **HEAD \**和当前\** branch** 切到上一条**commit** 的同时，你工作目录里的新改动和已经add到stage区的新改动也一起全都消失了：

```shell
git status
```

![](Git-Guide/20211111111827.png)


 可以看到，在 **reset --hard** 后，所有的改动都被擦掉了。

#### (7) 删除文件并找回

- 前提: 删除前，文件存在时的状态提交到了本地库
- 操作:

```git
git reset --hard [指针位置]
```

> 删除操作已经提交到本地库:指针位置指向历史记录
>
> 删除操作尚未提交到本地库:指针位置使用HEAD

![](Git-Guide/image-20200912185538491.png)

![](Git-Guide/image-20200912185601032.png)

or

![](Git-Guide/image-20200912185844115.png)

#### (8)比较文件差异

##### a. git diff [文件名]

将工作区中的文件和暂存区进行比较

![](Git-Guide/image-20200912190312828.png)

##### b.  git diff [本地库中历史版本] [文件名]

将工作区中的文件和本地库历史记录比较

![](Git-Guide/image-20200912190349374.png)

##### c.不带文件名比较多个文件

![](Git-Guide/image-20200912190542552.png)

### 4.分支管理

#### (1)什么是分支？

在版本控制过程中，使用多条线同时推进多个任务

![](Git-Guide/image-20200912190629652.png)

#### (2)分支的好处?

- 同时并行推进多个功能开发，提高开发效率
- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可

#### (3)分支操作

要将本地代码上传到远程新的分支上去:
1.远程新建分支

2.本地新建分支

git branch [分支名]

3.本地切换分支

git checkout [分支名]

4.push

git push [远程库] [分支名]

##### a.  创建分支

```git
git branch [分支名]
```

##### b.  查看分支

```git
git branch -v

git branch -a //查看本地和远程所有分支
```

##### c.  切换分支

```git
git checkout [分支名]
```

![](Git-Guide/image-20200912191110968.png)

##### d.  合并分支

- 第一步:切换到接受参数的分支(被合并，增加新内容)上

```git
git checkout [被合并分支名](master)
```

- 第二步:执行merge命令

```git
git merge [有新内容分支名](hot-fix)
```

##### e.  解决冲突

- 冲突的表现

![](Git-Guide/image-20200912191744984.png)

- 冲突的解决

> 第一步:编辑文件，删除特殊符号
>
> 第二步:把文件修改到满意的程度，保存退出
>
> 第三步:git add [文件名]
>
> 第四步;git commit -m "日志信息"
>
> - 注意:此时的commit一定不能带具体文件名



### 5.文件的状态变化



![](Git-Guide/image-20201223143525420.png)

新建的文件处于untracked状态， git add之后变成staged状态，再git commit之后变成unmodified状态。此时修改之后就变成Modified状态

文件状态:红色是在工作区(untracked)，绿色是在暂存区(staged)

## 五.Git的基本原理

### 1.哈希

![](Git-Guide/image-20200912192023278.png)

哈希是一个系列的加密算法，各个不同的哈希算法虽然加密强度不同，但是有以下几个共同点:

- 不管输入数据的数据量有多大，输入同一个哈希算法，得到的加密结果长度固定
- 哈希算法确定，输入数据确定，输出数据能够保证不变
- 哈希算法确定，输入数据有变化，输出数据一定有变化，而且通常变化很大
- 哈希算法不可逆

Git底层采用的是SHA-1算法

哈希算法可以被用来验证文件。原理如下图所示:

![](Git-Guide/image-20200912141806018.png)

Git就是靠这种机制来从根本上保证数据完整性的

### 2.Git保存版本的机制

#### (1)集中式版本控制工具的文件管理机制

以文件变更列表的方式存储信息。这类系统将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。

![](Git-Guide/image-20200912142112021.png)

#### (2)Git的文件管理机制

Git把数据看作是小型文件系统的一组快照。每次提交更新时Git都会对当前的全部文件制作一个快照并保存这个快照的索引。为了高效，如果文件没有被修改，Git不再重新存储该文件，而是只保留一个链接指向之前存储的文件。所以Git的工作方式可以称之为快照流。

![](Git-Guide/image-20200912142347996.png)

#### (3)Git文件管理机制细节

##### a.Git的"提交对象"

![](Git-Guide/image-20200912142533177.png)

##### b.提交对象及其父对象形成的链条

![](Git-Guide/image-20200912142518562.png)

### 3.Git分支管理机制

#### (1)分支的创建

![](Git-Guide/image-20200912142714184.png)

#### (2)分支的切换

![](Git-Guide/image-20200912142729357.png)

![](Git-Guide/image-20200912142752282.png)

![](Git-Guide/image-20200912142816815.png)

核心就是不动文件，动指针

## 六.Github

### 1.账号信息

GitHub首页就是注册页面:https://github.com/

![](Git-Guide/image-20200912143014675.png)

### 2.创建远程库

![](Git-Guide/image-20200912143059370.png)

![](Git-Guide/image-20200912143119382.png)

### 3.创建远程库地址别名

```git
git remote -v 查看当前所拥有远程地址别名
git remote add [别名] [远程地址]
```

![](Git-Guide/image-20200912200836708.png)

### 4.推送

```git
git push [别名] [分支名]
```

### 5.克隆

#### (1)命令

```git
git clone [远程地址]
```

#### (2)效果

- 完整的把远程库下载到本地
- 创建origin远程地址别名
- 初始化本地库

### 6.团队成员邀请

![](Git-Guide/image-20200912143559118.png)

"岳不群"其他方式把邀请链接发送给"令狐冲"，"令狐冲"登录自己的GitHub账号，访问邀请链接

![](Git-Guide/image-20200912143710981.png)

### 7.拉取

- pull = fetch+merge
- git fetch [远程库地址别名] [远程分支名]
- git merge [远程库地址别名/远程分支名]
- git pull [远程库地址别名] [远程分支名]

### 8.解决冲突

#### (1)要点

- 如果不是基于GitHub远程库的最新版所做的修改，不能推送，必须先拉取。
- 拉取下来后如果进入冲突状态，则按照"分支冲突解决"操作解决即可。

#### (2)类比

- 债权人:老王
- 债务人:小刘



- 老王说:10天后归还。小刘接受，双方达成一致。
- 老王媳妇说:5天后归还。小刘不能接受。老王媳妇需要找老王确认后再执行

### 9.跨团队协作

#### (1)Fork

![](Git-Guide/image-20200912144741225.png)

![](Git-Guide/image-20200912144804136.png)

#### (2)本地修改，然后推送到远程

####  (3)Pull Request

![](Git-Guide/image-20200912144826493.png)

![](Git-Guide/image-20200912144846976.png)

![](Git-Guide/image-20200912144903364.png)

#### (4)对话

![](Git-Guide/image-20200912144922694.png)

![](Git-Guide/image-20200912144940980.png)

#### (5)审核代码

![](Git-Guide/image-20200912144956936.png)

#### (6)合并代码

![](Git-Guide/image-20200912145027361.png)

![](Git-Guide/image-20200912145042178.png)

#### (7)将远程库修改拉取到本地

### 10.SSH登录

#### (1)进入当前用户的家目录

```git
cd ~
```

#### (2)删除.ssh目录

```git
rm -rvf .ssh
```

#### (3)运行命令生成.ssh密钥目录

```git
ssh-keygen -t rsa -C atguigu2018ybuq@aliyun.com
```

#### (4)进入.ssh目录查看文件列表给i他

```git
cd .ssh
ls -lF
```

#### (5)查看id_rsa.pub文件内容

```git
cat id_rsa.pub
```

#### (6)复制id_rsa.pub文件内容，登录GitHub，点击用户头像→Settings→SSH and GPG keys

#### (7)New SSH Key

#### (8)输入复制的密钥信息

#### (9)回到Git bash创建远程地址别名

```git
git remote add origin_ssh git@github.com:atguigu2018ybuq/huashan.git
```

#### (10)推送文件进行测试

## 七.Eclipse操作

## 八.Git工作流

### 1.概念

在项目开发过程中使用Git的方式

### 2.分类

#### (1)集中式工作流

像SVN一样，集中式工作流以中央仓库作为项目所有修改的单点实体。所有修改都提交到Master这个分支上。

这种方式与SVN的主要区别就是开发人员有本地库。Git很多特性并没有用到。

![](Git-Guide/image-20200912150638369.png)

#### (2)GitFlow工作流

GitFlow工作流通过为功能开发、发布准备和维护设立了独立的分支，让发布迭代过程更流畅。严格的分支模型也为大型项目提供了一些非常必要的结构。

![](Git-Guide/image-20200912150830534.png)

#### (3)Forking工作流

Forking工作流是在GitFlow基础上，充分利用了Git的Fork和pull request的功能以达到代码审核的目的。更适合安全可靠地管理大团队地开发者，而且能接受不信任贡献者地提交。

![](Git-Guide/image-20200912151340497.png)

### 3.GitFlow工作流详解

#### (1)分支种类

##### a.主干分支 master

主要负责管理正在运行地生产环境代码。永远保持与正在运行的生产环境完全一致

##### b.开发分支 develop

主要负责管理正在开发过程中的代码。一般情况下应该是最新的代码。

##### c.bug修理分支 hotfix

主要负责管理生产环境下出现的紧急修复的代码。从主干分支分出，修理完毕并测试上线后，并回主干分支。并回后，视情况可以删除该分支。

##### d.准生产分支(预发布分支) release

较大的版本上线前，会从开发分支中分出准生产分支，进行最后阶段的集成测试。该版本上线后，会合并到主干分支。生产环境运行一段阶段较稳定后可以视情况删除。

##### e.功能分支 feature

为了不影响较短周期的开发工作，一般把中长期开发模块，会从开发分支中独立出来。开发完成后会合并到开发分支。

#### (2)GitFlow工作流举例

![](Git-Guide/image-20200912152200388.png)

#### (3)分支实战

![](Git-Guide/image-20200912152148221.png)

#### (4)具体操作

①创建分支

![](Git-Guide/image-20200912152311010.png)

![](Git-Guide/image-20200912152329824.png)

②切换分支审查代码

![](Git-Guide/image-20200912152404719.png)

![](Git-Guide/image-20200912152421098.png)

![](Git-Guide/image-20200912152434413.png)

③检出远程新分支

![](Git-Guide/image-20200912152536747.png)

④切换回master

![](Git-Guide/image-20200912152623645.png)

⑤合并分支

![](Git-Guide/image-20200912152638597.png)

⑥合并结果

![](Git-Guide/image-20200912152712014.png)

合并成功后，把master推送到远程。

## 九.GitLab服务器搭建过程

### 1.官网地址

[首页](https://about.gitlab.com)

[安装说明](https://about.gitlab.com/installation/)

### 2.安装命令摘录

![](Git-Guide/image-20200912153010367.png)

### 3.调整后的安装过程

![](Git-Guide/image-20200912153050448.png)

当前步骤完成后重启

### 4.GitLab服务操作

①初始化配置gitlab

```gitlab
gitlab-ctl reconfigure
```

②启动gitlab服务

```gitlab
gitlab-ctl start
```

③停止gitlab服务

```gitlab
gitlab-ctl stop
```

### 5.浏览器访问

![](Git-Guide/image-20200912153348920.png)

### 参考链接

https://www.runoob.com/git/git-remote-repo.html

https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B re0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93

https://www.toutiao.com/i6675555153799545355/?tt_from=weixin&utm_campaign=client_share&wxshare_count=1&timestamp=1600127841&app=news_article&utm_source=weixin&utm_medium=toutiao_android&use_new_style=1&req_id=202009150757210100160410540620006D&group_id=6675555153799545355