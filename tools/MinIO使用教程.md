# MinIO使用教程

## 什么是MinIO

MinIO是一个开源的分布式高性能对象存储系统，使用GO语言编写

[官方文档地址](https://www.minio.org.cn/docs/minio/container/index.html)

它具有以下特点：

-   高性能 在标准硬件上，读/写速度上高达183GB/秒和171GB/秒
-   可扩展性 利用了web缩放器知识，简单的通过添加机器就可扩展集群
-   云原生   支持k8s,微服务和多租户的容器技术
-   S3兼容  采用了S3兼容协议(阿里云oss等也支持该协议)
-   简单   安装使用都非常简单，一个命令启动

MinIO的优点如下：

-   **部署简单**

    一个二进制文件（minio）即是一切，还可以支持各种平台

-   **支持海量存储**

    可以按zone扩展，支持单个对象最大5TB

-   **低冗余且磁盘损坏高容忍**

    标准且最高的数据冗余系数为2(即存储一个1M的数据对象，实际占用磁盘空间为2M)。但在任意n/2块disk损坏的情况下依然可以读出数据(n为一个纠删码集合中的disk数量)。并且这种损坏恢复是基于单个对象的，而不是基于整个存储卷的

-   **读写性能优异**

## 基本概念

MinIO没有文件的概念，只有桶和对象的概念，或者也可以把桶理解为目录，对象理解为文件

-   `S3`

    Simple Storage Service，简单存储服务，这个概念是Amazon在2006年推出的，对象存储就是从那个时候诞生的。S3提供了一个简单Web服务接口，可用于随时在Web上的任何位置存储和检索任何数量的数据。

-   `Object`

    存储到 Minio 的基本对象，如文件、字节流，Anything...

-   `Bucket`

    用来存储 Object 的逻辑空间。每个 Bucket 之间的数据是相互隔离的。

-   `Drive`

    部署 Minio 时设置的磁盘，Minio 中所有的对象数据都会存储在 Drive 里。

-   `Set`

    一组 Drive 的集合，分布式部署根据集群规模自动划分一个或多个 Set ，每个 Set 中的 Drive 分布在不同位置。

    -   一个对象存储在一个Set上
    -   一个集群划分为多个Set
    -   一个Set包含的Drive数量是固定的，默认由系统根据集群规模自动计算得出
    -   一个SET中的Drive尽可能分布在不同的节点上

## 安装

### 服务端（单机版）

不管性能如何，先装起来用再说~😜

这里以Docker为例：

```bash
#创建文件夹
mkdir -p ~/minio/data
#拉取镜像
docker pull minio/minio:latest
#运行Docker命令
docker run \
   -p 9000:9000 \
   -p 9090:9090 \
   --name minio \
   -v ~/minio/data:/data \
   -e "MINIO_ROOT_USER=ROOTNAME" \
   -e "MINIO_ROOT_PASSWORD=CHANGEME123" \
	 server /data --console-address ":9090"
```

解释一下参数

-   `-p 9000:9000`这个是minIO的API端口，必须指定
-   `-e "MINIO_ROOT_USER=ROOTNAME"`用来指定ROOT用户名，至少3个字符
-   `-e "MINIO_ROOT_PASSWORD=CHANGEME123"`用来指定ROOT用户密码，至少8个字符
-   `server /data --console-address ":9090"`用来指定Web的端口，注意不能和9000端口重复

​	如果不想使用9000端口，想换一个端口，则可以使用该命令：`server --address ":{PORT}"`

### 客户端

-   网页端

    刚才安装服务端指定`--console-address`就是网页端的端口，打开`http://localhost:9000`即可进入网页端进行管理

-   命令行客户端

    除了网页端之外，Minio官方也提供了一个MinIO Client客户端命令供我们使用，具体可到官方文档进行下载

## 快速使用

### Web端

Web端提供了一个很全面的图形化界面用来进行管理，下面就来快速上手一下👇🏻

1、使用我们刚才设置的管理员账号密码进行登录，进入如下界面

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021114873.png" alt="image-20230702111409816" style="zoom: 33%;" />

2、先创建一个账号

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021116702.png" alt="image-20230702111630676" style="zoom:33%;" />

同时我们还可以给该用户设置`access key`和`secret key`,相当于账号和密码，可以用来集成到代码中进行文件的上传和下载

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021133296.png" alt="image-20230702113326247" style="zoom:33%;" />

3、创建一个Bucket桶🪣

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021144385.png" alt="image-20230702114419356" style="zoom:33%;" />

4、上传文件📜

点击界面右上角角`Upload`即可上传文件或文件夹

5、下载文件

如果想要开放文件的下载权限，首先需要把桶的权限设置为`PUBLIC`或者`CUSTOM`

>   当然CUSTOM需要自己去手动设置

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021156351.png" alt="image-20230702115647299" style="zoom:33%;" />

复制文件路径

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021158627.png" alt="image-20230702115846583" style="zoom:33%;" />

​		加上MinIO的前缀即可下载成功

<img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202307021159145.png" alt="image-20230702115928102" style="zoom:33%;" />