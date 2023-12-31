# 导学

## 什么是汇编语言
我想大家应该都知道，像C语言、Java语言、JavaScript语言等，都是高级语言，是计算机科学发展的产物，而对于计算机来说，它其实是听不懂你在什么的，如果我们想让我们的程序能运行，其实是需要一个翻译官把我们的高级语言翻译成机器语言，它才能运行，至于这个翻译官，一般来说就是我们的编译器、链接器。<p> 
而汇编语言（Assembly language），是一门低级语言，它非常底层，可以说能够直接操纵硬件，在它的下面就是机器语言了。因为汇编语言的诞生，就是由于机器语言都是二进制，程序员想要记住这些指令来编程实在是太麻烦了，那么干脆就使用一些助记符来表示这些指令，这样汇编语言就诞生了。

## 学习汇编语言有什么意义
这是个好问题，大家的时间都很宝贵，把宝贵的时间花在有意义的事情上，才是正道。我们搞Java或者前端的，不写C，更不写底层，那么汇编语言有什么吸引力吗？🫥 <p>
其实关于汇编语言，我想如果大家是计算机专业的话，大概在大二就学过这门课了，但不管你学过或者没学过，我想大概也都忘记了，下面我将以一个全新的视角来说明一下学习汇编有什么用 <p>
之所以催生出想要重学汇编的想法，是因为我在学习并发、Java虚拟机的时候，总感觉有些概念非常难懂，很多东西虽然看着不是很难，但总有种似懂非懂的感觉，另外出于对计算机原理的兴趣，最终让我我萌生了要去强化自己计算机基础的想法。
因此，如果你也像我一样：
- 不想只懂CRUD，想知其然更知其所以然，做一个理解计算机的人 😎
- 想更好地学习JVM的知识（你以为Java就跟汇编无关？其实字节码文件就是Java版的汇编语言，比如AOP就用到了字节码技术）🤨
- 想要拜读《CSAPP》等计算机著作 😏
那么就行动起来，跟我一起学习汇编语言吧~😘

## 如何学习汇编语言
其实跟学习所有语言一样，汇编语言首先是一门语言，是语言自然就有它的语法，而且汇编语言其实它的定位是一门基础课，所以并不用太过害怕😁
那么有什么好的资料可以供我们学习呢？自然首推的是王爽老师的《汇编语言》了，要知道在这本书的作者简介里，王爽老师不仅是一个计算机教育家，还是一个哲学家，因此非常建议大家看一下这本书，真的会有一种相见恨晚的感觉🤩
《汇编语言》这本书是讲8086CPU的，而8086CPU毕竟比较老了，大家学完后还可以接着看另一本书《X86汇编语言，从实模式到保护模式》，这个作者可以说跟王爽老师也是不相伯仲，他的另一本计算机科普书《穿越计算机的迷雾》是倪光南院士做序的🙏
> 8086CPU可以说是一款经典的英特尔的16位CPU，具体更多介绍请看书本

另外，我们在学习一门知识的时候，一定要清楚我们学习的目的是什么，捉住主要矛盾，与我个人而言，并不打算搞底层开发，学习汇编语言更多的意义是为之后学习其他课程打基础，因此我更多是以理解看懂为主，追求速度和效率，并不会做过多的实践

👇这里放《汇编语言》的电子书资源（除了Markdown笔记外，我也会在书上做笔记），而另一本书的电子书我暂未找到，找到的小伙伴麻烦分享一下~😘
[《汇编语言》(第4版)](https://hoshumsdomain.top:9000/csbooks/%E6%B1%87%E7%BC%96%E8%AF%AD%E8%A8%80.pdf)

## 笔记
[《汇编语言》(第4版)笔记](./asm.md)
[实验篇笔记](./lab.m$$d)