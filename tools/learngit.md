# Git学习笔记及常见用法

## 一、Git简介

### ① Git是什么？

Git是分布式版本控制系统（即自动把历史版本记录下来）

### ② Git的诞生

Git由Linux系统的创始人Linus创建，起初目的是为了从各地来的Linux源代码方便合并，Git是由C语言开发的

### ③ 集中式与分布式

- 集中式的版本库是存放在中央服务器，干活的时候需要先从中央服务器中取得最新版本，肝完后再把自己的活推送给中央服务器，即需要联网进行工作，缺点是一旦中央服务器崩溃，所有人都无法干活
- 分布式则没有中央服务器，或者说每个人的计算机都是一个完整的版本库，当需要协作时只需要把各自的修改推送给对方即可，优点是安全性高，不需要联网

### ④ Git的安装

官网下载对应版本即可，安装完后需要设置用户名和邮箱

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

git的签名保存在家目录下的.gitconfig文件
```

### ⑤ 创建版本库

1. 打开git bash
2. 找到需要创建版本库的位置，使用git init命令初始化本地库
3. 使用git status查看当前git状态

```bash
natha@HoShumPC MINGW64 /d/dev_tools/Git/git_repository (master)
$ git status
On branch master 表示当前在master分支

No commits yet 表示还没有东西提交

nothing to commit (create/copy files and use "git add" to track)
```



1. 在版本库仓库地址下，把文件提交到Git

```bash
vim readme.txt
git add readme.txt 把文件提交到暂存区
git commit -m "wrote a readme file"  把文件提交到本地库
```



## 二、版本管理

### ① 查看Git的状态

1. 修改readme.txt上的内容
2. 查看git的状态

```bash
git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt **这里说明readme.txt被修改过了**

no changes added to commit (use "git add" and/or "git commit -a")
```

### ② 版本回退

当我们修改了文件并提交了，发现需要进行回退时，可以使用以下命令

- git log：可以查看历史记录（下面高亮的部分叫commit id，即版本号）

```bash
git log
commit 71dac18e6fe113cd55e229675d23d28bf0727007 (HEAD -> master)
Author: ake1525 <ake1525@ake.com>
Date:   Tue Mar 29 10:52:07 2022 +0800

    add distributed

commit d24c0a9a88f6c810ed51fb51486eab9ad6f219ab
Author: ake1525 <ake1525@ake.com>
Date:   Tue Mar 29 10:24:16 2022 +0800
```

- git reset：版本回退其中，后面加上 --hard 表示回退到哪个版本，如： git reset --hard HEAD^ ：表示回退到上一个版本；git reset -- hard 版本号（commit id） ：表示回退到具体的版本

```bash
git reset --hard 71dac **不用写完整**
HEAD is now at 71dac18 add distributed
```

但是当我们回退了之前的版本后后悔了想重新恢复到新版本怎么做呢？依然使用**git reset --hard commit_id**命令进行回退；而如果我们找不到版本号怎么办？可以使用**git reflog**查看历史命令找到版本号再回退

**git log和git reflog**的区别：git reflog是查看历史命令，显示的版本号是完整版本号的前7位（相当于查看精简版的版本信息）；而git log则是查看历史记录，可以显示完整的版本号以及记录是由谁提交（相当于查看详细版的版本信息）

### ③ 工作区和暂存区

- 工作区：就是我们文件所在目录
- 暂存区：工作区下的一个隐藏目录 **.git** Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指master的一个指针叫HEAD

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261035481.png)

因此，git add 命令实际上就是把提交的所有修改放到暂存区，而git commit 命令则是一次性把暂存区的所有修改提交到分支（关于分支和HEAD请看下文）

### ④ 管理修改

为什么说Git比其他版本控制系统设计更优秀，是因为Git跟踪并管理的是修改，而非文件。

比如说，当你第一次修改了一个文件并使用git add 命令进行添加，此时修改的文件被存放到暂存区，然后再第二次修改该文件但不添加，而是使用git commit 命令进行提交，这时第一次修改的文件就被提交到分支，而第二次修改的文件则因为没有放到暂存区而无法提交成功。

那针对这种情况我们如何提交第二次修改呢？

①可以继续使用git add 命令添加第二次修改，再使用git commit 命令提交

②也可以修改完第一次并添加后先不提交，而是等到第二次修改完并添加后再合并提交

### ⑤ 撤销修改

分三种情况

①修改后的内容未添加到暂存区：使用git checkout -- file 命令撤销回与版本库一模一样的状态

②内容已经添加到暂存区后再修改但未提交：使用git checkout -- file 命令撤销回已经添加到暂存区的状态

③修改后内容被添加到暂存区但未提交：使用git reset HEAD <file> 命令可以把暂存区的修改撤销掉，重新放回工作区，再使用git checkout -- file 命令丢弃工作区的修改

如果内容不仅被添加到暂存区还已经提交，此时只能通过版本回退的方法撤销提交，不过前提是没有被推送到远程库

### ⑥ 删除文件

分两种情况

①确实要删除：则可以先使用git rm <file> 命令再使用git commit -m “remove <file>” 命令

②删错了：使用git checkout -- test.txt 命令把版本库里的版本进行替代工作区的版本

## 三、分支管理

### ① 分支的作用

在实际开发环境中，往往总是多个任务同时进行，比如用户在使用一个线上分支，程序员在使用一个开发分支，而测试又使用一个测试分支。使用了分支就可以为每个任务创建单独的分支，意味着程序员可以把自己的工作从开发主线上分离开来，开发自己的分支而不会影响主线分支的运行。

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261035635.png)

### ② 分支的操作

| 命令名称            | 作用                         |
| ------------------- | ---------------------------- |
| git branch 分支名   | 创建分支                     |
| git branch -v       | 查看分支                     |
| git checkout 分支名 | 切换分支                     |
| git merge 分支名    | 把指定的分支合并到当前分支上 |

### ③ 分支的合并与冲突

### 1、正常合并

案例：在master分支上合并hot-fix分支

① master分支上的hello.txt

```bash
Hello World!
Hello World!
Hello World!
```

② hot-fix分支上的hello.txt

```bash
c
```

③ 合并分支

```bash
git merge hot-fix   #当前在master分支上
```

④ 合并后的master分支的hello.txt

```bash
Hello World!11111
Hello World!22222
Hello World!33333
```

因为hot-fix分支是在master分支的基础上分出去并进行了修改，当把hot-fix分支合并回去后，并不存在冲突，合并后的master分支就是hot-fix分支的内容

### 2、冲突合并

在上面的基础上，我们分别修改分支master的hello.txt文件和分支hot-fix的hello.txt文件，修改如下：

① master分支：

```bash
Hello World!
Hello World!
Hello World!test master
```

② hot-fix分支：

```bash
Hello World!11111
Hello World!22222
Hello World!test hot-fix
```

③ 在master分支上合并分支，发生冲突

```bash
git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt  #表示有冲突
Automatic merge failed; fix conflicts and then commit the result.
```

④ 使用vim进入hello.txt查看冲突原因

```bash
Hello World!11111
Hello World!22222
<<<<<<< HEAD
Hello World!test master
=======
Hello World!test hot-fix
>>>>>>> hot-fix
```

HEAD表示当前分支；<<<< 和 ====之间表示当前分支的代码；====和>>>>之间表示hot-fix分支的代码；**这里表示两段分支间的代码有冲突，因此git不敢给你自动合并，需要你手动进行合并**

⑤ 手动合并代码

手动删除冲突部分的代码，比如我想保留master分支的代码，就删除hot-fix部分的代码

```bash
Hello World!11111
Hello World!22222
Hello World!test master
```

使用 git add hello.txt 提交到暂存区

使用 git commit -m “test merge manually” **注意：此时不能带文件名，否则无法提交成功！**

**此时只会修改合并的分支代码，而不会修改不合并分支的代码！**

### 3、合并分支原理

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261211745.png)

master、hot-fix 其实都是指向具体版本记录的指针。当前所在的分支，其实是由 **HEAD**决定的。**所以创建分支的本质就是多创建一个指针**。HEAD 如果指向 master，那么我们现在就在 master 分支上。HEAD 如果执行 hotfix，那么我们现在就在 hotfix 分支上。所以**切换分支的本质就是移动HEAD指针（包括合并分支其实也是分支切换）**。

## 四、团队协作

### 1、团队内协作

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261035625.png)

说明：本地库A把代码**推送**到远程库，本地库B从远程库**克隆**代码到自己的本地库，然后修改后再将代码推送到远程库；本地库A再从远程库**拉取**代码到自己的本地库**更新**自己本地库的代码

clone与pull：clone是本来本地没有代码，因此需要从远程库拷贝一份到自己本地；而pull是本地本来有代码，现在代码更新了，因此从远程库拉取代码更新自己本地的代码

### 2、跨团队协作

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261202249.png)

说明：远程库A复制代码给远程库B，然后对方本地库从远程库B克隆一份该代码，更新后再推送给远程库B，然后远程B把代码请求拉取到远程库A，经过审核后合并代码，然后本地库再从远程库A中拉取代码，实现代码更新

## 五、远程仓库

GitHub网址：https://github.com/

### ① 创建远程仓库

登录GitHub选择右上角加号，点击New repository，填好远程库的名字后，可以选择https或ssh作为远程地址，这里先使用**https**进行演示：

这里在GitHub创建了一个远程库git-demo，其https地址为https://github.com/HoShum/git-demo.git

### ② 远程仓库操作命令

| 命令名称                           | 作用                                                   |
| ---------------------------------- | ------------------------------------------------------ |
| git remote -v                      | 查看当前所有远程地址别名                               |
| git remote add 别名 远程地址       | 起别名                                                 |
| git remote rm 别名                 | 删除远程仓库别名                                       |
| git push 别名 分支                 | 推送本地分支上的内容到远程库                           |
| git clone 远程地址                 | 将远程仓库的内容克隆到本地                             |
| git pull 远程库地址别名 远程分支名 | 将远程库对于分支最新内容拉下来后与当前本地分支直接合并 |

### ③ 给远程地址起别名

```bash
git remote add git-demo <https://github.com/HoShum/git-demo.git>
```

查看当前所有远程地址别名:git remote -v

```bash
git-demo   
     <https://github.com/HoShum/git-demo.git> (fetch)
git-demo        <https://github.com/HoShum/git-demo.git> (push)
```

表示既可以用这个别名拉取，也可以用这个别名推送

### ④ 拉取远程库到本地

```bash
git pull git-demo master
```

拉取远程库到本地后会自动提交代码到本地库

在Windows下搜索凭证管理器，可以看到注册在Windows的凭证

### ⑤ 克隆远程库到本地

```bash
cd ..
mkdir git-clone
cd git-clone
git clone <https://github.com/HoShum/git-demo.git>
cd ./git-demo
git status
```

说明：

- git clone不需要凭证
- git clone会做以下操作：1、拉取代码；2、初始化本地库；3、创建别名（默认为origin）

### ⑥ 团队内协作

先在GitHub邀请别人，当加入到团队后，便可以将本地代码推送到远程库，这样别人也可以从远程库中拉取代码到自己的本地库

### ⑦ 跨团队协作

### ⑧ SSH免登陆

如果使用GitHub的话，由于众所周知的网络原因，比起使用`https`会更推荐使用`ssh`来进行推送和拉取

具体操作的话，GitHub提供了两种方式来配置ssh公钥，分别是针对仓库级别的和针对全局的，但是要注意的是，GitHub的公钥只能配置在一个地方，比如你配置在了仓库A，就不能再配置在仓库B，非常蛋疼，所以如果是自己的仓库，建议配置在全局~

如何生成公钥？

非常简单，最无脑的办法就是，在Terminal / CMD控制台输入命令`ssh-keygen`，然后去到`~/.ssh/`目录下， `id_rsa_pub`就是公钥文件，接下来复制到Github即可

## 六、Idea集成Git

### ① 配置Git忽略文件

之所以要配置忽略文件，是因为它们与实际项目功能无关，不参与服务器上部署运行。把它们忽略掉既可以屏蔽掉IDE工具之间的差异，也可以节省Git的空间

为了便于让~./git.ignore文件引用，建议放在用户家目录下

① git.ignore文件模板内容如下：

```bash
# Compiled class file
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (J2ME)
.mtj.tmp/
# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see 
<http://www.java.com/en/download/help/error_hotspot.xml>
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
```

② 在.gitconfig文件中引用忽略配置文件（在Windows家目录下）

```bash
[user]
	name = hoshum
	email = hoshum@example.com
[core]
	excludesfile = C:/Users/natha/git.ignore
```

### ② Idea中定位Git

打开Idea，进入设置页面，找到Version Control下的Git，选择Git安装目录下的bin目录下的git.exe文件，点击测试，提示Git版本后表示成功定位到Git

### ③ 初始化本地库

新版Idea中的VCS看不到Import into Version Control，需要先到设置界面的Version Control下的Commit，取消勾选Use non-model commit interface，然后再到VCS中便可以看到Create Git Repository

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306261203291.png)

① 新建Git仓库后，会发现pom.xml文件**变红**（意味着未被追踪），同时去到项目所在目录可以看到一个.git目录（默认隐藏），同时顶端菜单栏会多出一个Git菜单

② 选中pom.xml右键选择Git，然后点击add，此时pom.xml**变绿**，表示被提交到暂存区

③ 新建一个类com.example.Git，此时Idea会提示是否要自动进行追踪

④ 选中整个项目，右键选中Git，点击add（可能旧版还会弹出是否要把忽略掉的文件添加进去，新版默认不会把忽略文件添加进去），之后再点击commit，填写提交信息，便顺利提交到本地库中，此时项目需要提交的文件都变为**黑色**！

### ④ 切换版本

当我们修改了代码后，会发现左边的文件会**变蓝，**表示文件被修改但还未提交，此时可以点击左下角的Git，然后点击Log，可以找到不同的版本，其中黄色是HEAD指针，绿色是分支指针，右键可以切换版本

### ⑤ 创建与切换分支

选中项目后右键，点击Git，然后点击Branch，就可以创建新的分支；又或者可以点击Idea右下角的当前分支名称的按钮，同样可以创建新的分支；切换分支同理

### ⑥ 合并分支（正常合并）

点击Idea右下角的当前分支按钮，选中要合并的分支，然后点击Merge Selected into Current

### ⑦ 合并分支（有冲突）

当合并分支时有冲突，Idea会非常智能地把冲突显示出来，并提供给我们去修改

## 七、Idea集成GitHub、Gitee码云

① 先使用token在Idea中配置GitHub

② 在Idea中可以直接把代码分享到GitHub，会自动帮我们创建远程库并推送过去

③ 代码推送：建议使用SSH方式进行推送。代码推送要保证本地代码的版本比远程库的代码版本要高，否则无法推送

④ 代码拉取：如果远程库代码和本地库代码不一致，会自动合并，如果合并失败，还需要手动解决冲突

⑤ 代码克隆：如果代码已经托管到远程库，那么我们可以从Idea首页使用克隆拷贝代码到本地

------

Idea对码云的操作与GitHub的操作一样

## 补充内容

## 1、Git常用命令

| 命令名称                                       | 作用               |
| ---------------------------------------------- | ------------------ |
| git config --global http://user.name 用户名    | 设置用户签名       |
| git config --global http://user.email 用户邮箱 | 设置用户签名       |
| git init                                       | 初始化本地库       |
| git status                                     | 查看本地库状态     |
| git add 文件名                                 | 添加到暂存区       |
| git commit -m “日志信息” 文件名                | 提交到本地库       |
| git reflog                                     | 查看历史记录       |
| git reset --hard 版本号                        | 版本穿梭           |
| git log                                        | 查看历史记录       |
| git fetch                                      | 拉取代码，不合并   |
| git pull                                       | 拉取代码，自动合并 |

## 2、Idea中快速查看版本修改信息

① 直接新建一个Project From Version Control 项目，使用Git地址克隆下来到本地

② 左下角点击Git，新建一个ChangeList，命名为忽略提交，把原本ChangeList的所有内容拉到里面，然后这个List的内容不需要提交，只有原本的ChangeList里面的内容才需要提交

## 3、Git Reset的三种模式

### ① mixed（默认）

```bash
git reset --mixed commitId
#等同于
git reset commitId
```

作用：回退到指定的版本，并将回退的内容全部放入**工作区**中

### ② soft

```bash
git reset --soft commitId
```

作用：回退到指定的版本，并将回退的内容全部放大**暂存区**中，此时使用`git status`可以看到暂存区中未提交的内容

### ③ hard

```bash
git reset --hard commitId
```

作用：回退到指定的版本，并**清空所有的修改**，此时再用`git status`会看到本地修改过的代码都消失了

## 4、本地版本回退后，如何推送给远程？

场景是这样的，开发中发现目前的版本有问题，因此想回退到上一个版本，那么可以使用`revert`和`reset`命令

### ① revert

revert相当于放弃提交的修改，然后生成一个新的提交，因此会保留以前提交过的记录

```bash
git revert HEAD
git push origin master
```

### ② reset

reset则是相当于将`HEAD`指针回退到指定版本，然后指定版本到当前版本的修改都会被清空，需要注意的是，因为reset是完全回退版本，因此提交远程仓库的时候需要**强制提交**，否则会报版本落后的错误

```bash
git reset --hard HEAD^ #回退到上一版本
git push origin master -f # -f 表示强制的意思
```

## 5、Git Stash如何用

假设这么个场景，当前在master分支上写代码，突然你的项目经理跟你说有个紧急的线上Bug需要修复，但是你当前的代码只写了一半，如果此时强行提交到开发环境会导致别的小伙伴报错，而如果你不提交，那么你可能在写的这一大堆代码到时要逐个去`add`和`commit`也是麻烦，而且万一要进行回滚就真的头大了

> 实际上更多的应用场景是在多分支开发下，此处使用单分支举例

这时候`stash`命令就能派上用场了

### ① 保存当前修改

```bash
git stash
git stash save "hot save"`
```

这两个命令的作用是一样的，只不过后一个可以记录版本

使用命令后，Git会把你当前所有未提交（包括暂存和非暂存）的内容**以栈的形式**都保存起来

> 默认情况下，只会缓存下列文件：
>
> - 添加到暂存区的修改（staged changes）
> - Git跟踪但未添加到暂存区（unstaged changes）
>
> 而不会缓存以下文件：
>
> - 工作区中新的文件
> - 被忽略的文件
>
> 如果要缓存的话，可以使用以下命令：
>
> - `git stash --include-untracked`
> - `git stash --all`

### ② 取出缓存中的修改

当我们修复完Bug后，终于又可以开开心心继续写我们的代码，这时候我们就可以从缓存中取出我们的代码了

刚才说过保存的时候是以栈的形式进行保存，因此默认下取出缓存也是从第一个取出

```bash
git stash pop
```

使用后会恢复之前缓存的工作目录，并**删除第一个stash缓存**

也可以指定取出哪个缓存

```bash
git stash apply stash@[0] #这个是缓存的名字，使用stash list即可看到
```

要注意，**该命令不会删除stash缓存**

### ③ 查看已有的缓存

```bash
git stash list
```

### ④ 移除缓存

移除第一个或指定名字的缓存

```bash
git stash drop stash@[0] #可选
```

清除所有缓存

```bash
git stash clear
```

### ⑤ 查看指定stash的diff

```bash
git stash show stash@[0] #可选
```

