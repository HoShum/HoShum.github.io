### Linux开机的三种方式

#### 背景

我们平时部署项目一般是在服务器上面安装各种中间件，以及把jar包拉到特定的文件夹里面写脚本启动。但是当遇到宕机的情况下，我们需要等服务器启动之后，慢慢上去开启各种脚本去启动各个项目，如果项目都比较急，这种方法往往比较费时间且比较麻烦。。。

那有没有什么方法可以服务器启动之后，项目也跟着启动呢，不用再慢慢上去服务器敲命令敲到手残呢

由该问题引起的好奇，一顿百度之后，发现有几个方法可以在Linux添加开机启动

#### 把需要启动的jar包做成服务

> systemd是Liunx的启动守护进程，已被大多数Liunx发行版所采用。相较于之前被采用的init进程串行启动，systemd进程采用并行启动且为系统启动管理提供了成套的方案。 

1. 去到` /etc/systemd/system`文件夹，编写一个service文件， 例如hz.service

   ![](https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/111.png)

2. 运行命令，重新加载Linux服务列表

   ```sh
   systemctl daemon-reload
   ```

3. 启动该服务

   ```sh
   systemctl start hz.service
   ```

4. 设置该服务开机启动

   ```shell
   systemctl enable hz.service
   ```

#### 在/etc/rc.d/rc.local 写启动脚本

第二种方式是直接修改配置文件`/etc/rc.d/rc.local`中添加开机启动需要执行的命令

![](https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/1687705299104.jpg))

最后修改rc.local的权限

```shell
chmod +x rc.local
```

#### 在corntab中写开机启动脚本

crontab是Linux的定时任务模块，但同样也可以编写开机启动的命令

输入命令`crontab -e`打开当前用户的定时任务，添加如下内容

![](https://raw.githubusercontent.com/HoShum/PictureRepo/main/imgs/1687705561466.jpg)

保存后下次重启服务器便可以自动执行上面的命令

