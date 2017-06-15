# ubuntu-1604-redis-install-and-configure（在Ubuntu 16.04 LTS(长期支持版上安装和配置一个单例的redis)）


#### 介绍(废话)
* Redis是内存中的key-value(键名-键值)存储，以其灵活性、高性能以及广泛的语言支持而闻名。下面是在Ubuntu 16.04 LTS 版本上的安装和配置Redis。


#### 先决条件
* 能够连接网络的Ubuntu 16.04的主机(linux 都差不多 不一定是Ubuntu这个发行版，之前我就在树莓派(Raspberry pi 3 b)(基于ARM而非X86架构的嵌入式电脑(用他的主要原因是省电(当前我有4个RP)))) ，在RP上安装Centos 7 (Centos可以认为是非商业版的RedHat)，并在Centos 7 上通过源码安装过Redis，因为redis是用C语言开发的所以具有非常高的可移植性，在windows平台上也有一个微软维护的vc版的redis(我只是听过还没有用过，毕竟现在再用Ubuntu这个Linux发行版来做开发平台，毕竟感觉Ubuntu比Centos和Fedora更适合做桌面操作系统，之前也尝试过很多个linux的发行版，之后我会把相关使用的截图发送到网络上(github上))。

#### 安装构建和配置依赖
1. 获取最新redis源码，和编译时用到的依赖
  * 在安装依赖之前请将软件源(顾名思义就是软件在网络上获取的源头)替换成国内比较快的，选取快的软件源可以通过ping命令来看延迟时间来判断，软件源也可以通过搜索引擎来找的
2. 更新、升级、添加依赖
``` shell
# 替换过软件源后更新和升级
sudo apt update && sudo apt upgrade -y
# 添加编译所需依赖
sudo apt install  build-essential tcl -y
```

#### 下载、编译、安装
1. 进入暂时存储目录
2. 下载最新稳定版redis的源码
3. 解压
4. 进入解压后的目录
5. 构建
6. 测试
7. 安装
```shell
cd /tmp && curl -O http://download.redis.io/redis-stable.tar.gz && tar zxf redis-stable.tar.gz && cd redis-stable/ && make && make test && sudo make install
```

#### 配置
1. 创建redis配置文件所存放的目录
```shell
mkdir /etc/redis
```

2. 将redis提供redis配置文件的样板拷贝一份到刚刚建好的目录里
```shell
cp /tmp/redis-stable/redis.conf /etc/redis/
```

3. 编辑配置文件(单例)
```shell
sudo vim/atom/code/subl /etc/redis/redis.conf
```
* 修改supervised这个键名对应的键值为systemd
* 修改dir这个键名对应的键值为/var/lib/redis

#### 创建Redis的systemd单元文件
1.打开/etc/systemd/system/redis.service
```shell
sudo atom\vim\nano\gedit\subl\code /etc/systemd/system/redis.service

```

2. redis.service中的内容
