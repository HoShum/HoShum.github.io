
## 1、查看对应的java进程  

使用命令：**top**

<img src="https://raw.githubusercontent.com/HoShum/HoShum.github.io/master/operations/imgs/image-20230625154431116.png" alt="image-20230625154431116" />



## 2、列出该进程下面的所有线程

使用命令：**top -Hp pid** (top -Hp 14369) 

<img src="https://raw.githubusercontent.com/HoShum/HoShum.github.io/master/operations/imgs/image-20230625154629139.png" alt="image-20230625154629139" />



## 3、输出线程对应的十六进制码  

使用命令：**printf “%x\n” pid **(printf “%x\n” 14380)

`备注：pid指的是线程的pid，此处选了一个cpu和内存占用较高的pid做为演示`
<img src="https://raw.githubusercontent.com/HoShum/HoShum.github.io/master/operations/imgs/image-20230625163118802.png" alt="image-20230625163118802" />







## 4、根据对应的十六进制码获取对应代码位置

使用命令： **jstack pid|grep -A 10 ××**, （jstack 14369|grep -A 10 382c）

`备注：其中pid是进程的pid，××是这个进程下的一个占用内存较高的线程的pid十六进制码，同时也可以jstack pid > thread_stack.log 输出到文件查看所有的线程信息，代码如果有死循环很容易定位到代码位置；  -A 10该关键字后10行， -B 10该关键字前10行， -C 10该关键字前后10行`

<img src="https://raw.githubusercontent.com/HoShum/HoShum.github.io/master/operations/imgs/image-20230625170424593.png" alt="image-20230625170424593"/>

