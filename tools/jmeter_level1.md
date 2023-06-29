# jmeter 入门

## jmeter是什么

jmter是用java开发的测试工具，免费开源，轻量级，支持多协议，常用于压力测试和系统的性能测试

## jmeter能做什么

- 接口测试
- 性能测试
- 压力测试（优势）
- 数据库测试

## 安装和配置

1. 前提时已经安装了jdk环境，进入[官方下载地址](https://jmeter.apache.org/download_jmeter.cgi)下载

   官网下载较慢。。建议复制下载链接到迅雷，速度杠杠啦

   

2. 设置界面中文

   默认jmeter的界面是英文，但可以通过设置配置文件把界面变成中文

   下载后，解压压缩包，打开`bin/jmeter.properties`，找到`language`的参数，修改该参数的值为`zh_CN`

   <div align="center"><img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281114.png"/></div>



3. 设置编码

   如果不设置编码，jmter查看返回结果时如果有中文的话会出现乱码的现象

   打开`bin/jmeter.properties`，找到`sampleresult.default.encoding`的参数，修改该参数的值为`utf-8`

   <div align="center"><img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281142698.png"/></div>

   

4. 启动jmeter

   双击`/bin/jmeter.bat`，就可以打开jmter界面

   ![image-20230628114845640](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281148675.png)

   

## jemter 的使用

### 线程组

> 线程组元件是任何一个测试计划的开始点，作用是设定模拟用户量和循环数量等信息

右键`测试计划` ->  添加  --> 线程

![image-20230628115454674](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281154711.png)

#### 线程组分类

- 线程组

  这个就是我们通常添加运行的线程。通俗来讲一个线程组，可以看做一个虚拟用户组，线程组中的每个线程都可以理解为一个虚拟用户。

- setup 线程组

  主要是测试开始前进行初始化的工作

  实际场景：

  - 测试数据库操作功能时，用于执行打开数据库连接的操作。
  - 测试用户购物功能时，用于执行用户的注册、登录等操作。
  
- teardown 线程组
  
  用于测试过后的执行动作
  
  实际场景：
  
  - 测试数据库操作功能时，用于执行关闭数据库连接的操作
  
  - 测试用户购物功能时，用于执行用户的退出等操作。
  
    
  
> 一个测试计划的执行顺序：setup线程组  -->  多个线程组并行运行  -->  teardown线程组

  

#### 线程组设置

![image-20230628134710747](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281347787.png)

线程数：虚拟用户数。一个虚拟用户占用一个进程或线程。

准备时长：设置的虚拟用户数需要多长时间全部启动。如果线程数为20 ，准备时长为10 ，那么需要10秒钟启动20个线程。也就是每秒钟启动2个线程。

循环次数：每个线程发送请求的次数。如果线程数为20 ，循环次数为100 ，那么每个线程发送100次请求。总请求数为20*100=2000 。如果勾选了“永远”，那么所有线程会一直发送请求，一到选择停止运行脚本。

**注意：**

多个相同类型的线程组启动的时候`默认是并行执行`啦，实际测试中如果需要一个线程组执行完之后再执行另一个线程组，比如，我要先调获取商品列表的接口，返回结果之后才能调点击进去查看详情的接口

那需要 点击`测试计划`，勾选`独立运行每个线程组`

相同类型的线程组之间的执行顺序由界面中的顺序来决定，如果需要修改执行顺序，需要拖动界面左侧的线程组来进行修改

![image-20230628135346231](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281353290.png)



### 添加请求

添加线程组之后，就可以添加需要测试的请求

`线程组右键 --> 采样器 --> Http请求`

![image-20230628142031682](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281420735.png)

填写响应的接口信息，服务器ip，端口号，请求的url，参数，消息体等

消息体中的数据为json格式

> 注意：同一个线程组里面的请求是一个接着一个来访问的

**技巧：**

1. 如果多个线程组访问的服务器ip，端口，协议等信息是一样，则可以把信息放到`Http默认请求值`

   具体操作：`测试计划右键 --> 添加  --> 配置元件 -->  HTTP请求默认值`

   ![image-20230628142958372](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281429415.png)

   打开后把信息统一填到里面

   ![image-20230628143227890](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281432991.png)

   

   这样，每一次新建一个请求的时候就不用重复填写相同的信息了，知填写不同的路径和对应需要测试的参数就可以啦

   

   2. 同理可得，如果只是同一个线程组里面的信息相同，则可以在对应的线程组下级添加对应的Http请求默认值

      ![image-20230628143708686](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281437769.png)

   

   

#### 添加请求头

   `Http请求右键 --> 配置元件 --> Http消息头管理器`

   ![image-20230628145041468](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281450501.png)

   同理，如果同一个线程组或者同一个测试计划用到的一些消息头是一样的，则可以在对应的下级里面创建，如下图所示

   ![image-20230628145312663](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281453711.png)

   

   #### 添加cookie

   `Http请求右键 --> 配置元件 --> HTTP Cookie管理器`

   ![image-20230628145733587](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281457629.png)

   同理，如果同一个线程组或者同一个测试计划用到的一些cookie是一样的，则可以在对应的下级里面创建

   

### 启动测试计划

设置完线程组和请求之后，就可以启动测试计划进行请求测试

![image-20230628150431583](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281504621.png)

### 查看结果

那启动了之后可以在哪里查看结果呢，jmeter提供了多种结果监听器

#### 查看结果数

`右键-->添加-->监听器-->查看结果树`

![image-20230628150918331](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281509371.png)

在这里可以查看对应接口的请求情况，请求头以及返回结果等

#### 聚合报告

`右键 --> 添加 --> 监听器 --> 聚合报告`

表格时间已毫秒显示

![image-20230628151748229](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281517274.png)

- 样本：总的发出请求数
- 平均值：请求的平均响应时间。单位：ms
- 中位数：50%的样本都没有超过这个时间。这个值是指把所有数据按由小到大将其排列，就是排列在第50%的值。单位：ms
- 90% 百分位： 90%的样本都没有超过这个时间。这个值是指把所有数据按由小到大将其排列，就是排列在第90%的值。单位：ms
- 99% 百分位： 99%的样本都没有超过这个时间。这个值是指把所有数据按由小到大将其排列，就是排列在第99%的值。单位：ms
- 最小值：最小响应时间
- 最大值：最大响应时间
- 异常%：本次测试中，有错误请求的百分比。
- 吞吐量：表示每秒完成的请求数。
- 接收：每秒从服务器端接收到的数据量。
- 发送：每秒发送到服务器端的数据量



至此一个简单的压力测试就可以在通过上面的方法进行设置~~~

### 设置变量

但是测试的时候又会有一种情况，比如，我现在想查询出书本的列表，然后取第一条记录的id再来查询该书本的详情，这种情况下就会涉及两个请求之间的数据传递

在这种情况下，变量的作用就产生

依然用回上面的例子，我的测试计划有两部：

- 查询所有的书本
- 查询第一本书的详细情况

第一步，先新建好第一个查询所有书本的请求

1. 再 `请求右键 --> 添加 --> 后置处理器 --> json提取器`

   ![image-20230628154452344](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281544433.png)

   > Jmeter后置处理器之Json提取器：https://blog.csdn.net/weixin_44169484/article/details/104979985

   2. 查看提取结果

      `右键 --> 添加 --> 采样器 --> Debug Sampler`

      ![image-20230628154643570](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281546631.png)

      点击运行，可以查看对应的提取结果是否正确

      获取书本这个请求的返回值：

      ![image-20230628155025732](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281550773.png)

      点击调试取样器，查看对应的结果，可以看到已经提取到对应的结果，获取到第二条记录的id，并赋值给book_id这个参数

      ![image-20230628155406583](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281554632.png)

​	

​		如果想提取全部id如下：

![image-20230628155535667](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281555750.png)

​	再运行一下执行计划，查看结果树

![image-20230628155646123](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281556202.png)

#### 同一个线程组下的变量

第二部，新建查询书本详细情况的Http请求

书本id要拼接上url后面，同一个线程组引用变量：`${引用名}`

![image-20230628160450241](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281604290.png)

至此，同一个线程组下就可以实现不同Http请求之间的变量引用

#### 跨线程组

如果两个请求不在同一个线程组，那就不能用上面的方法引用变量的

![image-20230628160832026](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281608096.png)

1. 设置成全局变量

   在`第一个请求右键 --> 添加 --> 后置处理器 --> BeanShell PostProcessor`

   ![image-20230628161026343](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281610382.png)

   表达式：`${__setProperty(nbook_id,${book_id},)};`

   ![image-20230628161405183](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281614234.png)

2. 在第二个请求中引用这个全局变量

   语法：`${__property(nbook_id)}`

   ![](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306291134657.png)

   至此，就可以实现跨线程组引用变量啦~~~

   

### 断言

和java的断言作用类似，判断是否符合预想结果

点击运行查看结果数即可查看断言结果

#### 响应断言

适用于：用于对请求的信息进行断言分析

`右键 --> 添加 --> 断言 --> 响应断言`

![image-20230628162908885](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281629938.png)

模式匹配规则：

- 包括：文本包含指定的正则表达式
- 匹配：整个文本匹配指定的正则表达式
- 相等：整个返回结果的文本等于指定的字符串（区分大小写）
- 字符串：返回结果文本包含指定字符串（区分大小写）
- 否：取反
- 或者：如果存在多个测试模板，勾选代表逻辑或



#### Json断言

适用于：HTTP响应结果是 json 格式时，可以使用 json断言

`右键 --> 添加 --> 断言 --> JSON断言`

![image-20230628164228647](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306281642703.png)

#### 更多

> 参考网址：https://zhuanlan.zhihu.com/p/410756743

   

### 逻辑处理器

控制逻辑处理器下的Http的执行规律等

比如：

- 随机控制器 ：该控制器下的HTTP请求之中随机选择一个HTTP请求进行运行

- 仅一次控制器：在多线程循环的时候，将使其子节点下的取样器请求只运行一次

  无论线程组设置了多少个循环，该控制下的请求只执行一次

- 循环控制器：该控制器下的取样器请求可以循环运行

- ......

> 参考网址：https://zhuanlan.zhihu.com/p/538436000

   



   

   
