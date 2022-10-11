## 一、Git概述

> Git是一个免费、开源的分布式版本控制系统，可以快速高效地处理从小型到大型的各种项目。并且Git易于学习、内存小、性能极快，它具有廉价的本地库。方便的暂存区域和多个工作流分支等特性。其性能优于SVN、CVS等版本控制工具。

### 1.1 什么是版本控制

> 版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。版本控制其实最重要的是可以记录文件修改历史记录，从而让用户能够查看历史版本，方便版本切换。



### 1.2 为什么需要版本控制

> 版本控制出现的核心原因是开发逐渐由个人开发过度到团队协作



### 1.3 版本控制工具

> 版本控制工具可以根据使用分为集中式版本控制工具和分布式版本控制工具

#### 集中式版本控制工具

> 常见的如CVS、SVN、VSS

集中化的版本控制工具都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而管理员也可以轻松掌控每个开发者的权限，并管理一个集中化的版本控制系统，要远比在各个客户端上维护本地数据库来得轻松容易。

事分两面，有好有坏，这么做显而易见的缺点是中央服务器的单点故障，如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。



#### 分布式版本控制工具

> 常见的如Git、Bazaar

像Git这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（到本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其它客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。

分布式的版本控制系统出现以后，解决了集中式版本控制系统的缺陷：

1. 服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）
2. 每个客户端保存的也都是整个完整的项目（包含历史记录，更加安全）



### 1.4 Git工作机制

![查看源图像](https://pic2.zhimg.com/v2-01368ac7859e73cf8b882d644fdb1589_b.jpg)

### 1.5 Git和代码托管中心

> 代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为远程库。

- 局域网
  - GitLab
- 互联网
  - GitHub（外网）
  - Gitee码云（国内网站）



## 二、Git安装

> 一路点击下一步即可，注意Git安装目录中不要出现中文或者空格。



## 三、Git常用命令

|              命令               |      作用      |
| :-----------------------------: | :------------: |
|            git init             |  初始化本地库  |
|           git status            | 查看本地库状态 |
|         git add 文件名          |  添加到暂存区  |
| git commit -m “日志信息” 文件名 |  提交到本地库  |
|           git reflog            |  查看历史记录  |
|     git reset –hard 版本号      |    版本穿梭    |

### 3.1 设置用户签名

> 安装好git后需要进行的一次操作，只需要进行一次，作用是提交代码时标记自己是谁和联系方式，若不设置提交时会报错。

|                命令                 |     作用     |
| :---------------------------------: | :----------: |
| git config –global user.name 用户名 | 设置用户签名 |
| git config –global user.email 邮箱  | 设置用户邮箱 |

**如何查看是否配置成功了？**

```
#在C盘用户目录下查看是否存在文件.gitconfig，若存在，打开即可查看设置的用户签名
如: C:\Users\Lenovo\.gitconfig
[user]
	name = KittySmile
	email = 15770838310@163.com
```

[^注]: 这里设置的用户前面和将来登录 GitHub（或其它代码托管中心）的账号没有任何关系。这只是设置本地 Git 客户端签名。



### 3.2 初始化本地库

> 初始化本地库即让 Git 获取文件的管理权，让文件变为工作区。

|   命令   |     作用     |
| :------: | :----------: |
| git init | 初始化本地库 |



### 3.2 查看本地库状态

> 查看工作区状态

|    命令    |      作用      |
| :--------: | :------------: |
| git status | 查看本地库状态 |



### 3.3 添加暂存区

> 将文件保存到暂存区，还未提交到本地库，可以通过 git rm –cached 文件名 进行删除。

|         命令          |                            作用                            |
| :-------------------: | :--------------------------------------------------------: |
|    git add 文件名     |      将文件添加到暂存区（文件名为.表示添加所有文件）       |
| git rm –cached 文件名 | 将暂存区中的文件删除（不删除工作区文件，路径有中文会出错） |



### 3.4 提交本地库

> 将暂存区的文件提交到本地库，形成库中文件的历史版本。

|              命令               |              作用              |
| :-----------------------------: | :----------------------------: |
| git commit -m “日志信息” 文件名 | 将暂存区中的文件添加到本地库中 |



### 3.5 版本穿梭

> 提交之后的文件都有自己的版本号，Git保存了所有提交的版本，并且可以通过版本号切换到不同版本，这叫做版本穿梭。

|        命令名称        |              作用              |
| :--------------------: | :----------------------------: |
|       git reflog       |        查看版本提交日志        |
| git reset –hard 版本号 | 将当前仓库的版本切换为目标版本 |

[^注]: 切换版本即让 HEAD 指向的 master 分支进行切换，可以通过 .git/HEAD 文件查看当前分支名称，而在 .git\refs\heads\ 目录下选择分支名可以查看当前指向的版本号。



## 四、Git分支操作

> Git分支操作是指在版本控制过程中，同时推进多个任务，为每个任务创建单独的分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开，在开发自己的分支时，不会影响主线分支的运行。分支底层其实是指针的引用。使用分支可以同时并行推进多个功能开发，提高开发效率。并且在各个分支开发过程中。如果某一个分支开发失败，不会影响其它分支，失败的分支可以删除重新开始。

|        命令         |             作用             |
| :-----------------: | :--------------------------: |
|  git branch 分支名  |           创建分支           |
|    git branch -v    |           查看分支           |
| git checkout 分支名 |           切换分支           |
|  git merge 分支名   | 把指定的分支合并到当前分支上 |



### 4.1 查看分支

> 查看当前存在的所有分支

|     命令      |          作用          |
| :-----------: | :--------------------: |
| git branch -v | 查看当前存在的所有分支 |



### 4.2 创建分支

> 根据当前分支版本创建出一个新的分支

|       命令        |                作用                |
| :---------------: | :--------------------------------: |
| git branch 分支名 | 根据当前分支版本创建出一个新的分支 |



### 4.3 切换分支

> 切换到不同分支

|        命令         |   作用   |
| :-----------------: | :------: |
| git checkout 分支名 | 切换分支 |



### 4.4 合并分支

> 将目标分支与当前分支合并（不影响目标分支）。

|       命令       |   作用   |
| :--------------: | :------: |
| git merge 分支名 | 合并分支 |

[^注]: 当出现冲突时，需要手动找到冲突点，并解决冲突，再重新添加到暂存区和仓库。



## 五、GitHub操作

> GitHub是当今最常用的远程仓库。在GitHub上创建好一个仓库后，可以通过本地仓库设置远程仓库。

|                  命令                  |                        作用                        |
| :------------------------------------: | :------------------------------------------------: |
|             git remove -v              |               查看当前所有远程库别名               |
| git remote add 远程库别名 远程仓库地址 |               设置远程仓库并设置别名               |
|           git push 别名 分支           |         推送本地分支仓库内容到远程仓库分支         |
|           git clone 远程地址           |              将远程仓库内容克隆到本地              |
|     git pull 远程库别名 远程分支名     | 将远程仓库分支的最新内容拉取下来和当前本地分支合并 |

[^注]: 推送到远程仓库前需要在自己windows上登录Github账号。方式为进入控制面板 -> 凭据管理器 -> 普通凭据 中添加Github账号信息。



### 5.1 创建远程仓库

> 在本地库中设置远程仓库。设置好后以后本地库推送和拉取就根据远程库操作。

|                命令                |          作用          |
| :--------------------------------: | :--------------------: |
|           git remote -v            | 查看当前所有远程库别名 |
| git remote add 远程库别名 远程地址 |   设置本地库的远程库   |

[^注]: 设置远程仓库的前提是远程仓库存在。



### 5.2 推送到远程仓库

> 将本地仓库分支内容推送到远程仓库分支（默认是master分支）

|           命令           |                作用                |
| :----------------------: | :--------------------------------: |
| git push 远程库别名 分支 | 推送本地分支仓库内容到远程仓库分支 |



### 5.3 拉取远程到本地库

> 将远端库中内容拉取下来并自动提交本地库。

|              命令              |                        作用                        |
| :----------------------------: | :------------------------------------------------: |
| git pull 远程库别名 远程分支名 | 将远程仓库分支的最新内容拉取下来和当前本地分支合并 |



### 5.4 克隆远程库到本地

> 将远程库内容克隆到本地库，克隆过程中会拉取代码、初始化本地仓库、创建origin远程仓库别名。

|        命令        |           作用           |
| :----------------: | :----------------------: |
| git clone 远程地址 | 将远程仓库内容克隆到本地 |

[^注]: 克隆操作不需要登录。



### 5.5 团队协作

> 多名成员协作开发时，推送内容到远程仓库的前提是需要成为团队内一员。



## 六、IDEA集成Git

> 工作中最长使用的方式。

### 6.1 配置Git忽略文件

> 即配置通过Git推送时忽略掉的文件，一般忽略掉与项目实际功能无关的文件。创建忽略规则文件为xxx.ignore（前缀名随便起，建议是git.ignore），这个文件存在位置原则上哪里都可以，但为了便于让~/.gitconfig文件引用，建议也放在用户家目录下。

git.ignore文件模板内容如下：

```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

hs_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

在用户家目录C:\Users\Lenovo下创建git.ignore文件，将模板内容复制进去，保存。

然后找到 **.gitconfig** 文件，新增配置引用该ignore文件：

```
[core]
	excludesfile = C:/Users/Lenovo/git.ignore
	#注意，这里/符号不能误写为\符号
```



### 6.2 IDEA配置Git安装目录

> IDEA初次使用Git需要进行的操作，告诉IDEA使用哪个Git。

依次点击File -> Settings -> Version Controller -> Git -> 找到git启动文件绑定 -> 测试成功

![文件无法预览。](https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011200909388.png?Expires=1665491039&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=N1rA0r5Z3rqLz5FFCW7GTYLt%2FMs%3D)



### 6.3 IDEA项目Git的初始化&添加&提交

> IDEA设置好Git安装目录后，需要将项目交由Git管理，交给Git管理就无非初始化，添加到临时区，提交本地等等。



#### 初始化

> 使用IDEA将项目交给Git管理的操作（创建Git仓库，即初始化）：

![文件无法预览。](https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011204945824.png?Expires=1665493140&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=zZertsuZEeWZI0W9h6mAR9BuYUw%3D)

选中目标文件（点击OK进行目标文件Git的初始化）：

<img src="https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011205532437.png?Expires=1665493342&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=VwIy5z6igQ0XmXI53fj%2BO5eiTVs%3D" alt="文件无法预览。" style="zoom:50%;" />

#### 添加

> 初始化成功后，右键项目会出现git选项，选择add加入暂存区。

<img src="https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011210550381.png?Expires=1665493908&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=zVsn1dlZapmZ0s%2Fshb1HupZ4yLk%3D" alt="文件无法预览。" style="zoom: 33%;" />



#### 提交

> add到暂存区后，就是推送到本地库。

![文件无法预览。](https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011211109366.png?Expires=1665494195&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=SUq459eEBfVrFh0Mb4E%2BPOCBkM0%3D)



### 6.4 IDEA切换版本

> IDEA中切换版本较为简单，点开IDEA下方的Git视图，点击Log，就可以查看所有的版本，想要切换版本，在目标版本上右键check即可。

![文件无法预览。](https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011212237550.png?Expires=1665494889&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=tM71oSQzOfygFBeEUCCLKiJZMc0%3D)



### 6.5 IDEA创建分支&切换分支

> IDEA中创建和切换分支也很方便，其中创建分支有两种方式，一种是右键项目 -> Git -> New Branch，另一种则直接在IDEA右下方找到版本号点击进行操作。

#### 创建分支

第一种，右键项目创建：

<img src="https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011212757051.png?Expires=1665495223&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=pX9RFmopNSZjQyTkv5IJcmt%2Fan0%3D" alt="文件无法预览。" style="zoom:50%;" />

<img src="https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011213129298.png?Expires=1665495426&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=ihf3GyHvWZXT0YxA9cmqFrLB98s%3D" alt="文件无法预览。" style="zoom:50%;" />



第二种，则直接右下角找到该版本创建，过程与第一种一致。



#### 切换分支

> 切换分支操作也简单，直接点击IDEA右下角分支，选择切换到的目标分支，点击切换即可

![文件无法预览。](https://edu-20220510.oss-cn-beijing.aliyuncs.com/notes_pictures/image-20221011213357650.png?Expires=1665495606&OSSAccessKeyId=TMP.3KhjjMHDsT7SVqRUTwwXrUMWEgvym86h1usQdfhBLqqVg19DYjWKQ3HcDH3aFV7QbKU78rRvEXrRLxxYyvVNU7zuRUPk6y&Signature=D%2BY02XXNevUmUIcJBhDoZJCjXl8%3D)



