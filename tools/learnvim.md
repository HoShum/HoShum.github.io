# Vim教程

## 写在开头，为什么用Vim？

已经忘记是什么时候，网上看到视频里面的大神写代码完全可以脱离鼠标，只用键盘去输入，而且速度飞快，当时就非常的震惊，也很好奇是如何做到的

后来经过了解才知道，这叫`Vim`，而使用`Vim`去敲代码的群体还有个称号，叫`Vim党`，而`Vim`是啥？

我一想，这不是`Linux`中那个键位难受得飞起的编辑器吗，对于习惯使用Windows键位的人来说，这确实很难受，什么乱七八糟的，`hjkl`上下左右移动？想要输入还得分个插入模式、普通模式？这也...太脑残了吧，谁设计的？

当然，后来自然逃不了真香,,,

因为`Vim`的诞生始于还是命令行操作系统的时候，那时候没有图形化界面，也就没有鼠标这一样东西，那么这样想想能发明`Vim`的人还是很牛逼的，能想到多种输入模式的人更是天才的想法

那么，到了图形化界面已经很方便的今天，为什么`Vim`没有过时，依然有一堆粉丝呢？我觉得有如下几个原因：

1.  `Vim`非常轻量，它比`VSCode`还轻量，因为它就是一个纯粹的编辑器，也就意味着，它加载非常快
2.  `Vim`支持非常多的插件，能帮助你实现类似`IDE`的效果，但依然很快
3.  `Vim`几乎支持所有的语言，如果你既要写后端，又要写前端，时不时还得写一下脚本，那么就能感受到Vim的好处
4.  所有的`IDE`都有它自己的快捷键，如果你经常要切换，那么你就得记不同的快捷键，而`Vim`则是通用的，你可以使用同一套配置打遍天下
5.  `Vim`支持任何自定义的操作方式，比如我不喜欢`hjkl`来上下左右，那么你也可以自定义自己的映射，非常随心所欲
6.  主流的`IDE`，如`VSCode`、`IDEA`都支持以插件的方式配置`Vim`

说了这么多，也许你已经心动，但我并不推荐用纯`Vim`去写代码，因为`Idea`也确实有很多方便的地方，我更推荐的是使用`IdeaVim`这款插件，是的，几乎所有的IDE都有`Vim`的插件，如果你感兴趣的话，也可以照抄我的配置，开箱急用

以下教程是我在B站学习`Vim`时记录的笔记：[玩转Vim-从放弃到入门_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1NG4y1p74h/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=60b4d86edab68478132e73658e161cfd)

>   有些地方可能还不太完善，尤其插件篇章，后续会持续更新

如果你也感兴趣，不妨就开始吧~

------

## 一、基本操作

### 1、编辑插入模式

- i：insert 在当前光标前面插入
- a：append 在当前光标后面插入 
- o：open a line below 在当前光标的下一行插入
- A：append after line 在当前行的最后插入
- I：insert before line 在当前行的最前面插入
- O：open a line above 在当前行插入上一行

###   2、Vim的四种模式

- Vim默认是normal**（普通）模式**

- 使用insert、a等进入**编辑模式**
- 使用:cmd 进入**命令模式**和 v(visual)进入**可视化模式**

因为日常使用最多的是浏览文本，所以默认是普通模式

使用Esc可以退出插入模式进入normal模式

命令模式可以进行一些全局替换、退出等操作

可视化模式下可以进行批量操作，比如使用v可以批量选中字符，使用V可以批量选择行

### 3、Vim编辑小技巧

- 如何快速纠错？

  - ctrl + h 删除上一个字符
  - ctrl + w 删除上一个单词
  - ctrl + u 删除当前行

  > 上面这几个方法在很多终端也适用
  >
  > 以及：ctrl + B 回到上一个字符，ctrl + F 去下一个字符

- 快速切换insert和normal模式

  - 使用Ctrl + c（可能会中断某些插件）代替Esc或者`Ctrl + [ `
  - 在普通模式下可以使用`gi`快速跳转到最后一次编辑的地方并进入编辑模式

  > 之后的配置可以改变Esc的映射，比如CapsLock改成Ctrl

### 4、Vim快速移动技巧

- 基本上下左右：hjkl

- 单词之间跳转

  - w/W ：移动到下一个word/WORD开头
  - e/E：移动到下一个word/WORD结尾
  - b/B：移动到上一个word/WORD开头

  > word指的是以非空白符分割的单词，WORD以空白符分割的单词

- 行间搜索移动：同一行快速移动的方式就是搜索一个字符并且移动到该字符

  - 使用f{char}移动到char字符上，t{char}移动到char的前一个字符
  - 如果第一次没有搜到`;`用`,`或者 , 继续搜索该行下一个/上一个
  - 大写F表示反过来搜前面的字符

- 水平移动

  - **0移动到行首的第一个字符**（也可以使用0w，就是先去到行首，再去到下一个字符）
  - ^移动到第一个非空白符
  - **$移动到行尾**
  - g移动到行尾非空白符

- 垂直移动

  - 使用()在句子间移动
  - 使用{}在段落之间移动

  > 使用插件easy_motion可以更方便

- Vim页面移动

  - `gg/G`移动到文件开头和结尾，使用`ctrl + o`快速返回
  - `H/M/L`跳转到屏幕开头，中间和结尾（Head，Middle，Lower）
  - `Ctrl + u`，`Ctrl + f`上下翻页（upword/forward）
  - `zz`把屏幕置为中间

### 5、Vim快速增删改查

- Vim增加字符：进入插入模式

- 快速删除一个字符或者单词

  - 在**插入模式**使用`ctrl + h`，`ctrl + w`，`ctrl + u`，分别删除一个字符，一个单词，一行
  - 在**normal模式**下使用`x`快速删除一个字符
  - 使用d配合文本对象快速删除一个单词
    - `daw`（d around word）删除一个单词包括空白字符
    - `dw` 默认就是daw
    - `diw` 删除一个单词，不包括空白字符
    - `dt(`( d until ’(‘ ) 从当前光标删除到下一个'('
    - `d$` 删除光标位置到行尾
    - `d0` 删除光标位置到行首
  - d和x可以搭配数字来执行多次
    - 比如`2dd`：删除两行
    - 比如`2x`：删除两个字符
  - 使用**显示模式**删除
    - 先进入显示模式，再选中要删除的字符或者行

- 快速修改

  - 常用有三个，`r`，`c`，`s`，进行replace，change，substitute

    **normal模式下**

    - r 可以替换一个字符，使用R可以不断进行替换

    - s 替换并进入插入模式，S是整行删除并进入插入模式；也可以搭配数字删除指定字符并进入插入模式

    - c 配合文本对象进行快速修改

      change也是删除字符并进入插入模式，下面进行一些举例👇🏻

      - `caw`：删除一个单词并进入插入模式
      - `C`：删除当前光标后的一整行并进入插入模式
      - `ct"`：删除光标位置到`"`并进入插入模式

- 查询

  - 使用`/`或者`？`进行向前或者反向搜索
  - 使用`n`或者`N`跳转到下一个或者上一个匹配
  - 使用`*`或者`#`进行当前单词的前向或者后向匹配
  - 如果要取消高亮，则使用`set nog`

### 6、Vim进行搜索替换

> 比如进行代码重构，经常需要进行替换代码

**在命令模式下：**

使用substitute可以查找并且替换掉文本，且支持正则表达式

- :[range] s[ubstitute]/{pattern}/{string}/[flags] 

> 注意这里的s和range间隔一个空格

- range表示范围，比如：10,20表示10到20行，%表示全部
- pattern表示要替换的模式，string表示是替换后的文本
- flags表示要替换的标记，有以下常用标记：
  - g（global）表示全局范围内执行
  - c（confirm）表示确认，可以确认或者拒绝修改
  - n（number）报告匹配的次数而不是替换，可以用来查询匹配次数

**举例**

- 统计`you`出现的次数：`:% s/you//n`
- 将1到6行的所有hello替换成world：`:% s/hello/world/g`
- 还可以结合正则表达式去进行更精确的替换
  - `:% s/\<hello\>/g`，这样就只会替换`hello`，而不会替换`_hello`或者`hello_`

### 7、多文件操作

Vim的多文件操作比较特别，先来认识几个概念

**① Buffer Window Tab**

- Buffer
  - 指打开的一个文件的内存缓冲区
  - Vim打开一个文件后就会加载文件内容为缓冲区，之后的修改都是针对这个内存中的缓冲区，==并不会直接保存到文件==
  - 直到使用`:w`命令才会保存到文件中
  - 如果我们打开多个文件，则会创建多个缓冲区
- Window是窗口，指Buffer可视化的分割区域
- Tab可以用来组织窗口为一个工作区

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932970.png" alt="image-20230123195755342" />

**② Buffer切换**

- 使用`:ls`会列举当前所有
- 使用`:bpre`，`:bnext`，`:bfirst`，`:nlast`分别跳转到上一个、下一个、最开始和最后一个缓冲区
- 也可以用`:b buffer_name`，配合Tab补全来跳转，buffer_name就是文件名

> 如何打开一个新的文件呢？使用:e ./文件名

**③ 窗口分割和切换**

有时候我们的屏幕需要展示多部分的代码，因此可以进行窗口的分割，每个窗口又可以继续创建不同的缓冲区

- <control + w> s：进行水平分割；或者使用`:sp`
- <control + w> v：进行垂直分割；或者使用`:vs`

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932971.png" alt="image-20230123201159198" />

- <control + w>w：在窗口间循环切换
- <control + w>h：切换到左边的窗口；H：移动到左边
- <control + w>j：切换到下边的窗口；J：移动到下边
- <control + w>k：切换到上边的窗口；K：移动到上边
- <control + w>l：切换到右边的窗口；L：移动到右边

**④ Tab（标签页）将窗口分组**

Tab是容纳一系列窗口的容器，使用较少，这里先知道吧~

### 8、Text Object文本对象

Vim里面也有对象的概念，如一个单词，一个句子，一个段落

通过操作文本对象比操作一个字符要方便很多

**① 文本对象**

一个文本对象由两部分组成

- 修饰符：`a`表示该对象及后面的空字符，`i`表示对象本身
- 对象名称：`w`为单词，`s`为句子，`p`为段落

> 有些插件可能会扩展文本对象，比如使用f表示一个函数

**② 基本格式**

[number]\<command>[text object]

- number表示次数（使用较少）
- command表示命令
  - 如d(delete)
  - 如c(change)
  - 如y(yank)复制
- text object表示文本对象

**③ 示例**

以下使用一些示例来作说明

- 操作单词句子段落

  - `daw`：删除光标所在的单词（包含后面的空格)，不进入插入模式
  - `ciw`：删除光标所在的单词(不包含后面的空格)，并进入插入模式
  - `vaw`：选择光标所在的单词（包含后面的空格)，并进入视图模式

  > 其余s，p也同理

- 操作括号内的内容
  - `ci"`：删除""内的内容，并进入插入模式
  - `da{`：删除{}内的内容，但不进入插入模式

### 9、复制粘贴及寄存器的使用

**① 普通模式下**

- 复制：`y`
- 粘贴：`p`
- 剪切：先`d`，后`p`

| 命令                                    | 撤销上一次编辑         |
| :-------------------------------------- | ---------------------- |
| `ctrl` + `R`（mac上是 `control` + `R`） | 重做，即撤销上一次撤销 |
| `V`（大写）                             | 选中当前行             |
| `d`                                     | 剪切文本               |
| `y`                                     | 复制文本               |
| `p`                                     | 在光标后粘贴文本       |
| `P`（大写）                             | 在光标前粘贴文本       |
| `dd`                                    | 剪切光标所在行         |
| `yy`                                    | 复制光标所在行         |
| `D`                                     | 剪切光标到行尾的文本   |
| `x`                                     | 剪切光标所在位置的字符 |

> 需要学会配合文本对象去使用，这样才可以高效！！！比如先复制一个单词yiw，再$到行尾，最后p进行粘贴

**再来举几个例子**

- `y4k`：复制包括当前行在内的上面4行
- `y3l`：复制包括当前字符在内的右边3个字符
- `yfr`：复制到`r`为止
- `yi{`：复制{}内的所有文本
- `di[`：删除[]内的所有文本
- `dto`：删除当前光标直到`o`
- `ci"`：删除""内的所有文本，并进入插入模式

**② 插入模式下**

- 一样使用`control + c`和`control + v`或者`cmd + c`和`cmd + v`进行复制粘贴

- 当使用了`:set autoindent`命令后，复制粘贴后的代码的缩进全都错乱了
- 这个时候可以使用`:set paste`和`:set nopaste`解决

**③ 寄存器**

Vim复制或者删除后的内容并不是存放在系统剪贴板，而是“无名寄存器”

利用这个特点可以对某个字符先按`x`再按`p`进行两个字符的对调

> 由于感觉这个功能是比较高级的功能，这里先略过，以后再回来看

**④ 使用系统剪切板**

`:set clipboard=unnamed`这样设置以后就可以在普通模式下按`p`直接粘贴

> 但是注意如果是在服务器上，则无法使用剪切板，只能先`:set paste`然后在插入模式下使用`control + v`进行粘贴

### 10、使用宏完成批量操作



### 11、Vim补全大法

- `control n`：显示补全
  - `control n`：下一个
  - `control p`：上一个

### 12、修改配色

- 使用`:colorscheme <C-d>`来查看配色
- 使用`:colorscheme <color>`来修改配色
- 网上下载的配色方法放到`.vim/colors`中

### 其他

**命令模式下：**

- `set nu`：显示行号
- `syntx on `：语法高亮
- `set hls`：高亮搜索结果 
- `set incsearch`：增量搜索，边搜索边高亮
- `:qa`：退出所有的窗口
- 在出现智能提示下，`control + N`和`control + P`分别代表下一个和上一个

**跳转快捷操作**

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932972.png" alt="image-20230206104405207" style="zoom:60%;" />

- 跳转：`<C-]>`
- 回退：`<C-o>`
- 前进：`<C-i>`

**Visual模式快速跳转**

在vim的Visual模式下，您可以使用以下快捷键来快速选择到指定的位置：

1.  使用h、j、k和l键，或者使用箭头键来移动光标，选择要选择的文本区域。
2.  您可以使用w键和e键来在单词之间移动并选择文本，或者使用b键向后移动并选择单词。
3.  您还可以使用f和t命令来跳转到特定字符并选择文本。例如，要选择句子中的所有字符直到“;”字符，您可以输入命令“v f;”。
4.  如果您想要选择一整行，您可以使用快捷键Shift+v来进入行选择模式，然后使用j和k键选择要选择的行数。
5.  如果您想要选择一整个文档，您可以使用快捷键Shift+g来跳转到文档的最后一行，然后再按下Shift+gg选择整个文档。
6.  可以使用<C-v>进入块状模式（目前还不太会使用）

**普通模式进行缩进**

在 Vim 中，在普通模式下进行缩进，您可以使用以下命令：

1.  按下 `Ctrl + V`，进入可视块模式。
2.  使用上下箭头或鼠标选择要缩进的文本块。
3.  按下大写字母 `I` 进入插入模式，然后输入缩进符号，例如四个空格或一个制表符。
4.  按下 `Esc` 退出插入模式，此时所有选择的文本块都被缩进了。

请注意，缩进符号的类型和数量取决于您的个人偏好和项目的规范。此外，您还可以使用 `>>` 命令将当前行向右缩进一个缩进符号，或使用 `<<` 命令将当前行向左移动一个缩进符号。

**批量替换单词**

在 Vim 编辑器中，可以使用全局替换命令（:s）批量替换单词。下面是具体的步骤：

1.  打开需要进行替换的文件：在终端中输入 `vim filename` 命令打开文件，或者在 Vim 中使用 `:e filename` 命令打开文件。
2.  进入命令行模式：在 Vim 中按下冒号键（:）即可进入命令行模式。
3.  输入替换命令：使用 `:s/old_word/new_word/g` 命令进行替换。其中，old_word 表示需要被替换的单词，new_word 表示替换后的单词，g 表示全局替换。

*例如，要将文件中所有的“apple”替换为“orange”，则可以使用命令 `:%s/apple/orange/g`。其中，% 表示对整个文件进行操作。*

1.  *按下回车键执行替换命令。*
2.  *查看替换结果：如果需要查看替换结果，可以使用 Vim 的搜索命令（/）进行查找。*
3.  *保存文件并退出：使用 Vim 的保存命令（:w）保存文件，使用退出命令（:q）退出 Vim 编辑器。如果需要同时保存并退出，可以使用组合命令（:wq）。*

*更多例子：*

*当需要批量替换文件中的某个单词时，可以使用 Vim 的全局替换命令 `:s`。下面是一些示例：*

-   将文件中所有的 "dog" 替换为 "cat"：

```
rubyCopy code
:%s/dog/cat/g
```

-   将当前行中的 "apple" 替换为 "orange"：

```
rubyCopy code
:s/apple/orange/g
```

-   将当前行中的所有 "apple" 替换为 "orange"，但不区分大小写：

```
rubyCopy code
:s/apple/orange/gi
```

-   将当前行中第一个 "apple" 替换为 "orange"：

```
rubyCopy code
:s/apple/orange/
```

-   将当前行中第二个 "apple" 替换为 "orange"：

```
rubyCopy code
:s/apple/orange/2
```

-   将文件中的 "hello" 替换为 "world"，但仅替换第 3 行到第 7 行：

```
bashCopy code
:3,7s/hello/world/g
```

**代码折叠	**

1.  `zo`展开折叠
2.  `zO`对所在范围内所有嵌套的折叠点进行展开
3.  `zc`折叠
4.  `zC`对所在范围内所有嵌套的折叠点进行折叠

## 二、Vim的配置

### 1、vimrc

- Vim的配置可以写在 ~/.vimrc

- 要使配置生效，除了关闭文件后打开，还可以使用`:source ~/.vimrc`

下面看一张图片了解配置文件

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932973.png" alt="image-20230125184916930" style="zoom: 33%;" />

### 2、Vim映射

#### ① 基本映射

> 基本映射就是指Normal模式下的映射

下面使用一些例子进行演示

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932974.png" alt="image-20230204120514417" />

如果要进行持久化只需要写进~/.vimrc文件，**注意**，不需要加`:`

#### ② 常用模式下定义映射

#### <img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932976.png" alt="image-20230204121716704" />

- `:imap <c-d> <Esc>ddi` 意思就是先进入普通模式再按dd再回到插入模式

> 但是这种方式下有个问题，比如定义了这两个映射 1、:namp - dd；2、:nmap \ -
>
> 这样如果按下\ 就会删除一行，**相当于递归映射**
>
> 如果装了很多插件，而这些插件的映射有冲突，就会出现问题

#### ③ 非递归映射

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932977.png" alt="image-20230204123007719" />

**示例**

![image-20230204124621874](https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932978.png)

- `<leader>`表示前缀，用来方便定义映射
- ``^`表示回到原本光标的位置
- `<cr>`表示回车

## 三、Vim插件

### 1、Vim-Plug 插件管理器

首先需要安装一个插件管理器，用来管理Vim的插件

> GitHub地址：https://github.com/junegunn/vim-plug

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

打开`.vimrc`，只保留这两行，其他删掉即可

```bash
call plug#begin()

"这里加需要加的插件

call plug#end()
```

当安装插件后，修改vimrc后，需要

- 执行`:source ~/.vimrc`
- 执行`:PlugInstall`

### 2、vim-startify 好用的vim开屏插件

> GitHub地址：https://github.com/mhinz/vim-startify

把插件加到vim-plug中

```bash
Plug 'mhinz/vim-startify'
```

### 3、如何找到需要的插件？

大部分的插件都托管在GitHub

- 直接在Google搜索想要的插件，比如我想要Java相关的插件，则`vim java plugin`
- 通过这个网站搜索：https://vimawesome.com

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932979.png" alt="image-20230206010754981" />

### 4、Vim美化插件

修改Vim外观

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932980.png" alt="image-20230206011052301" />

https://github.com/vim-airline/vim-airline

https://github.com/yggdroot/indentline

- 安装状态栏美化

```bash
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
```

> 注意要删掉Plugin的in

- 增加代码缩进线条

```bash
Plug 'Yggdroot/indentLine'
```

- 找好看的配色

  - vim-hybird配色：`github.com/w0ng/vim-hybird`
  - solarized配色：`github.com/altercation/vim-colors-solarized`
  - gruvbox配色：`github.com/morhetz/gruvbox`

  <img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932981.png" alt="image-20230206012435054" style="zoom: 50%;" />

  安装完插件后，可以执行以下命令更换主题：`:colorscheme hybird`

  如果需要持久化，则需要在vimrc下：

  ```bash
  set background=dark
  colorscheme hybird
  ```


### 5、NerdTree

>   [GitHub - preservim/nerdtree: A tree explorer plugin for vim.](https://github.com/preservim/nerdtree)

官方说明：

```bash
nnoremap <leader>n :NERDTreeFocus<CR>
nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
nnoremap <C-f> :NERDTreeFind<CR>
```

-   `o`展开目录，`O`递归展开所有目录
-   `x`合拢当前节点所有目录，`X`递归合拢当前节点下的所有目录
-   `p`跳到当前节点的父节点，`P`跳到根节点
-   `J`跳到同级下的最后一个节点，`K`跳到同级下的第一个节点
-   `i`split一个新窗口打开选中文件并跳转到该窗口（垂直）
-   `s`split一个新窗口打开选中文件并跳转到该窗口（水平）
-   `I`切换是否显示隐藏文件
-   `F`切换是否显示文件
-   `q`关闭NerdTree窗口

### 6、EasyMotion

> https://github.com/easymotion/vim-easymotion

```bash
Plug 'easymotion/vim-easymotion'
```

这里主要使用这个命令即可

```bash
nmap s <Plug>(easymotion-s2)
nmap t <Plug>(easymotion-t2)
```

### 7、Vim-surround

> https://github.com/tpope/vim-surround

```bash
Plug 'tpope/vim-surround'
```

- ds（delete a surrounding)
  - 比如删除由`{}`包裹的文本：`ds{`
- cs（change a surrounding)
  - 比如修改由`{}`包裹变为`()`包裹的文本：`cs{(`
- ys（you add a surrounding)
  - 比如给一个单词加上双引号：`ysiw"`，注意使用左*会有空格，使用右*不会有空格

### 8、模糊搜索和批量替换

#### ① 模糊搜索

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932982.png" alt="image-20230207215046860" />

> https://github.com/junegunn/fzf

安装

```bash
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
```

- 使用Ag[Pattern]模糊搜索字符串
  - 搜索到的多个结果使用`C-j`和`C-k`上下移动
- 使用Files[Path]模糊搜索目录

#### ② 批量替换

> https://github.com/brooth/far.vim

安装

```bash
Plug 'brooth/far.vim'
```

### 9、Vim tagbar

> https://github.com/preservim/tagbar

安装

```bash
Plug ’preservim/tagbar‘
```

### 10、代码补全插件

> https://github.com/Shougo/deoplete.nvim

- 支持多种语言，支持模糊匹配
- 需要安装对应语言的扩展





## 四、其他 

### 1、IdeaVim

> [GitHub - JetBrains/ideavim: IdeaVim – A Vim engine for JetBrains IDEs](https://github.com/JetBrains/ideavim)

注意，在IdeaVim中设置action映射需要这样做：

-   使用递归映射：`nmap <leader>tt <action>(ParameterInfo)`
-   使用非递归映射：`nnoremap <leader>tt :action ParameterInfo`



**踩坑点**

-   如果使用组合健，切记不要使用`a i b o`等本身有特殊命令作为开头，不然会比较麻烦

### 2、VSCode Vim

VSCode Vim跟Idea不同，是需要在`setting.json`里面使用JSON的形式进行配置（个人认为这种配置方式比原生的要麻烦）

同时VSCode官方已经提供了一部分插件使用，只需要手动选择开启或者关闭即可

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932983.png" alt="image-20230623125356899" style="zoom: 33%;" />

<center>VSCode Vim配置页面</center>

## 五、我的配置

先贴上我的Idea配置

### ① 安装插件

<img src="https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/202306251932984.png" alt="image-20230625103329375" style="zoom:50%;" />

### ② 打开`~./ideavimrc`文件

在Idea界面右下角能直接打开

贴上我的配置：

```txt
"" Source your .vimrc

"source ~/.vimrc

" ================================================================================================

" = Extensions =====================================

" ================================================================================================

set easymotion

set nerdtree

set surround

" ================================================================================================

" = Basic settings =====================================

" ================================================================================================

set clipboard+=unnamed

set ignorecase " Do incremental searching.

set scrolloff=30

set history=200

set incsearch

")set hlsearch

"set keep-english-in-normal-and-restore-in-insert

map Q gq

"缩进

"" -- Map IDE actions to IdeaVim -- https://jb.gg/abva4t

"" Map \r to the Reformat Code action

"map \r <Action>(ReformatCode)

"" Map <leader>d to start debug

"map <leader>d <Action>(Debug)

"" Map \b to toggle the breakpoint on the current line

"map \b <Action>(ToggleLineBreakpoint)

" Find more examples here: https://jb.gg/share-ideavimrc

" ================================================================================================

" = No Leader Keymaps =====================================

" ================================================================================================

"nnoremap ji :action GotoImplementation<CR>

"nnoremap jd :action GotoDeclaration<CR>

"nnoremap je :action GotoNextError<CR>

"nnoremap jm :action MethodDown<CR>

"nnoremap gmn :action MethodDown<CR>

"paste 粘贴"

nnoremap <C-v> p

inoremap <C-v> <Esc>pa

"easymotion 快速检索插件"

nmap ss <Plug>(easymotion-s2)

"returnToNormal 回到普通模式"

inoremap jk <Esc>

vnoremap jk <Esc>

"MoveQuickly 快速移动光标"

nnoremap J 5j

nnoremap K 5k

nnoremap W 5w

nnoremap B 5b

"vnoremap <C-j> 5j

"vnoremap <C-k> 5k

vnoremap <C-n> 5j

vnoremap <C-p> 5k

"SwitchTab"

nnoremap <S-l> :action NextTab<cr>

nnoremap <S-h> :action PreviousTab<CR>

"快速移动光标"

nnoremap vv viw

nnoremap vk v$

nnoremap vj v^

"Change Inner Word"

nnoremap cc ciw

"MoveCaretToLineStartOrEndThenInsert 移动到行首行尾"

nnoremap <C-h> ^

nnoremap <C-l> $

inoremap <C-h> <ESC>^i

inoremap <C-l> <ESC>$a

"Jump to Next Bracket"

nnoremap <C-k> %

inoremap <C-k> <Esc>a

"showParameterInfo"

"inoremap pp :action ParameterInfo<CR>

"Delete A Line"

inoremap <C-d> <Esc>ddi

"-----------------------显示模式----------------------"

vnoremap < <gv

vnoremap > >gv

vnoremap <C-c> y

vnoremap J :action MoveLineDown<CR>

vnoremap K :action MoveLineUp<CR>

"--------------------自定义Idea配置--------------------"

"ReformatCode"

noremap rr :action ReformatCode<CR>

"ToggleLineBreakpoint"

nnoremap <C-g> :action ToggleLineBreakpoint<CR>

"GotoImplementation"

nnoremap gi :action GotoImplementation<CR>

nnoremap gs :action GotoSuperClass<CR>

nnoremap gb :action Back<CR>

nnoremap gf :action Forward<CR>

nnoremap gd :action GotoDeclaration<CR>

nnoremap gn :action MethodDown<CR>

nnoremap gp :action MethodUp<CR>

nnoremap gj :action GotoNextError<CR>

nnoremap gk :action GotoPreviousError<CR>

nnoremap gu :action ShowUsages<CR>

"Method Up / Down"

nnoremap <C-[> :action MethodUp<CR>

nnoremap <C-]> :action MethodDown<CR>

"Undo"

"inoremap uu :action $Undo<CR>

" ================================================================================================

" = Leader Keymaps =====================================

" ================================================================================================

" leaderkey

let mapleader=" "

" ================================================================================================

" 👻👻👻 Which-Key 👻👻👻

" ================================================================================================

set which-key

set notimeout

" ========================

" From https://gist.github.com/LintaoAmons/18a8e3d5d45a22280ca54f1c69f43717

" ========================

" d: diff

nmap <leader>dd <action>(Vcs.ShowTabbedFileHistory)

" f: Find/Format ⭐️

let g:WhichKeyDesc_FindOrFormat = "<leader>f FindOrFormat"

let g:WhichKeyDesc_FindOrFormat_FindFile = "<leader>ff FindFile"

nmap <leader>ff <action>(GotoFile)

let g:WhichKeyDesc_FindOrFormat_FindFileLocation = "<leader>fl FindFileLocation"

nmap <leader>fl <action>(SelectInProjectView)

let g:WhichKeyDesc_FindOrFormat_FindText = "<leader>ft FindText"

nmap <leader>ft <action>(FindInPath)

let g:WhichKeyDesc_FindOrFormat_Commands = "<leader>fc Commands"

nmap <leader>fc <action>(GotoAction)

let g:WhichKeyDesc_FindOrFormat_OpenedProject = "<leader>fp OpenedProject"

nmap <leader>fp <action>(OpenProjectWindows)

let g:WhichKeyDesc_FindOrFormat_Format = "<leader>fm Format"

nmap <leader>fm <action>(ReformatCode) \| <action>(OptimizeImports)

" g: Git ⭐️

let g:WhichKeyDesc_Git = "<leader>g Git"

let g:WhichKeyDesc_Git_RollbackHunk = "<leader>gr RollbackHunk"

nmap <leader>gr :action Vcs.RollbackChangedLines<CR>

" i: InWhert ⭐️

let g:WhichKeyDesc_InsertAfterBrackets = "<leader>i InsertAfterBrackets"

nmap <leader>i f(a

" j: add Semicolon and goto nextline⭐️

let g:WhichKeyDesc_InsertSemicolon = "<leader>j InsertSemicolon"

nmap <leader>j A;<ESC>o

" l: lsp: Language server protocol (align with neovim)⭐️

let g:WhichKeyDesc_LSP = "<leader>l LSP"

let g:WhichKeyDesc_LSP_Rename = "<leader>lr Rename"

nmap <leader>lr <action>(RenameElement)

" n: No ⭐️

let g:WhichKeyDesc_No_Highlight = "<leader>nl NoHighlight"

nmap <leader>nl :nohlsearch<CR>

" s: Show ⭐️

let g:WhichKeyDesc_Show = "<leader>s Show"

let g:WhichKeyDesc_Show_FileStructure = "<leader>ss ShowFileStructure"

nmap <leader>ss <action>(FileStructurePopup)

let g:WhichKeyDesc_Show_Bookmarks = "<leader>sb ShowBookmarks"

nmap <leader>sb <action>(ShowBookmarks)

let g:WhichKeyDesc_Show_ParameterInfo = "<leader>sb ShowParameterInfo"

nmap <leader>sp <action>(ParameterInfo)

" r: Run/Re ⭐️

let g:WhichKeyDesc_RunOrRe = "<leader>r RunOrRe"

let g:WhichKeyDesc_RunOrRe_ReRun = "<leader>rr ReRun"

nmap <leader>rr <action>(Rerun)

let g:WhichKeyDesc_RunOrRe_ReRunTests = "<leader>rt ReRunTests"

nmap <leader>rt <action>(RerunTests)

let g:WhichKeyDesc_RunOrRe_Rename = "<leader>rn Rename"

map <leader>rn <action>(RenameElement)

" w: Window ⭐️

let g:WhichKeyDesc_Windows = "<leader>w Windows"

let g:WhichKeyDesc_Windows_maximize = "<leader>wo maximize"

nmap <leader>wo <action>(UnsplitAll) \| <action>(HideAllWindows)

let g:WhichKeyDesc_Windows_splitWindowVertically = "<leader>wl splitWindowVertically"

nmap <leader>wl <action>(Macro.SplitVertically)

let g:WhichKeyDesc_Windows_closeActiveWindow = "<leader>wc closeActiveWindow"

nmap <leader>wc <c-w>c

" z: zip(fold) ⭐️

let g:WhichKeyDesc_Zip = "<leader>z Zip"

let g:WhichKeyDesc_Zip_unZipAll = "<leader>zo unZipAll"

nmap <leader>zo <action>(ExpandAllRegions)

let g:WhichKeyDesc_Zip_ZipAll = "<leader>zc ZipAll"

nmap <leader>zc <action>(CollapseAllRegions)

" c: Close ⭐️;

let g:WhichKeyDesc_CloseBuffer = "<leader>c CloseBuffer"

nmap <leader>c :q!<CR>

" e: Toggle Explorer ⭐️

let g:WhichKeyDesc_ToggleExplorerOrExtract = "<leader>e ToggleExplorer"

nmap <leader>e <action>(ActivateProjectToolWindow)

" e: Extract

" extract method/function

vmap <leader>em <action>(ExtractMethod)

" extract constant

vmap <leader>ec <action>(IntroduceConstant)

" extract field

vmap <leader>ef <action>(IntroduceField)

" extract variable

vmap <leader>ev <action>(IntroduceVariable)

" ========================

" Custom

" ========================

"MoveWindow"

nnoremap <leader>ww <C-w>w
nnoremap <A-h> <C-w>h
nnoremap <A-l> <C-w>l
nnoremap <A-j> <C-w>j
nnoremap <A-k> <C-k>k

nnoremap <leader>wl :action MoveTabRight<CR>

nnoremap <leader>wj :action MoveTabDown<CR>

nnoremap <leader>wk :action MoveTabTop<CR>

nnoremap <leader>wv :action SplitVertically<CR>

nnoremap <leader>ws :action SplitHorizontally<CR>

nnoremap <leader>wq :qa<CR>

"StretchWindow"

nnoremap <leader>wm :action StretchSplitToRight<CR>

nnoremap <leader>wn :action StretchSplitToLeft<CR>

"NerdTree"

nnoremap <leader>tt :NERDTree<CR>

nnoremap <leader>tf :NERDTree<CR>

"ActivateToolWindow"

nnoremap <leader>at :action ActivateTerminalToolWindow<CR>

nnoremap <leader>ar :action ActivateRunToolWindow<CR>

nnoremap <leader>ad :action ActivateDebugToolWindow<CR>


"启动项目相关"
nnoremap <leader>ff :action Debug<CR>
nnoremap <leader>rr :action Run<CR>

```

