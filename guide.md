# 操作指南

> 这是本知识库的操作指南

## 关于知识库
本知识库基于`docsify`来搭建，该项目为GitHub开源项目，非常轻量，支持多种部署方式，且支持多种插件\
具体可看官方文档：[官方中文文档](https://docsify.js.org/#/zh-cn/) \
如果各位发现有更好用的插件，请务必添加到知识库中！:kissing_heart:

## 关于文档
文档主要由`markdown`文档组成，`markdown`是一种轻量级的标记语言，支持实时渲染，可以说非常适合程序员，也希望各位可以通过编写文档热爱上写作~ :blush:

## 什么是Markdown
`markdown`语法非常简单，详细可看官方教程：[Markdown官方教程](https://markdown.com.cn/)，亦或者看本知识库中的相关内容[Markdown语法](/tools/markdown) \
另外，`markdown`是转成`html`的格式来渲染的，因此也可以使用`html`的语法来进行编写，如果大家发现更多用法，务必分享出来！！！:exclamation:

## 如何写Markdown
支持写`markdown`的软件非常多，这里主要的区别是： 
* 如果你更喜欢用纯语法的方式写，那么我推荐`VSCode`，当然你用文本编辑器也没问题:satisfied:
* 如果你像我一样更喜欢**所见即所得**，那么我会更推荐`Typora`这款软件，它可以使用快捷键来代替`markdown`语法
> 实际上绝大部分的IDE、笔记软件（如语雀、Notion、Wolai等）都支持`markdown`写法，各位可以多去尝试~

## 关于Typora
Typora是一款非常简洁的专门用来写Markdown的软件[Typora地址](https://typora.io/)，它无需你记忆Markdown的语法，即可使用各种快捷键来快速编写文档，而且它有多种主题可以配色，具体可到官网查阅。\
目前Typora已经结束了`dev`期，发布了正式版，但依然保留`dev`版本的下载，可以说非常良心，而且这款软件也不贵，三个设备89元，是买断制的，喜欢的可以支持一波（我自己已经支持了😂）

## 关于文档的图片上传
不知道是不是`docsify`本身图片链接的原因，目前编写的`markdown`文档原生格式的图片无法加载，因此改为使用GitHub作为图床，搭配`PicGo`[PicGo地址](https://picgo.github.io/PicGo-Doc/zh/)使用，请大家编写`markdown`文档的时候注意图片的格式，如果使用`Typora`的话可以直接设置图片为上传

## 关于插件
> Docsify有非常多的插件可以用来扩展，这里介绍部分比较特别的插件如何使用
> 这里放一个全插件中文说明网址，[插件说明](https://xhhdd.cc/index.php/archives/80/)

### PDF嵌入插件
该插件支持嵌入pdf文件，非常适合用来存放电子书 <br>
具体用法看就跟嵌入代码块差不多，用```来包裹，里面放pdf文件的路径

### 文本高亮插件
先看效果
<div align="center">
    <img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306290956991.png"/>
</div>
具体使用：

```markdown
> [!tip] #当然这里可以换成note、warning、attention
> 填写你要的内容
```
### GIF插件
显而易见，用来显示GIF，用法跟图片一样

```markdown
![](docs/charlie.gif
```

### Emoji插件
就是用来给我们的文档增添点活力的，用法非常简单

```markdown
:具体的Emoji:
```
但是如何方便地去找想要的Emoji呢？
- 目前发现VSCode有个插件叫`Emoji`可以用来支持搜索，如果有更方便的方法记得分享~🙋
- 另外，使用Windows的话，可以按下`Windows + .`来获取Emoji 😂

> 更新：除了常规的Emoji外，目前还增加了Twitter Emoji

### 日志更新插件
[GitHub链接](https://github.com/anikethsaha/docsify-plugin/tree/master/packages/docsify-changelog-plugin)
该插件主要方便用来编写更新日志，具体位置在[更新日志](/CHANGELOG.md) <br>
由于GitHub上说需要把`loadNavbar`置为false，这就导致了导航栏不能显示了，但它有说到，可以在`<body>`内使用<nav>标签解决😂