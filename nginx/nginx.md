- [Linux安装nginx（没有编译环境）](#1-Linux安装nginx（没有编译环境）)
- [linux安装nginx   (有编译环境)](#2-linux安装nginx(有编译环境))

# 1. Linux安装nginx（没有编译环境）

> **在linux下安装nginx，首先需要安装 gcc-c++编译器。然后安装nginx依赖的pcre和zlib包。最后安装nginx即可。**

1.先安装gcc-c++编译器

```java
yum install gcc-c++
yum install -y openssl openssl-devel
```

2.再安装pcre包

```java
yum install -y pcre pcre-devel
```

3.再安装zlib包

```java
yum install -y zlib zlib-devel
```

> **下面进行nginx的安装**

1. 在/usr/local/下创建文件nginx文件

```java
mkdir /usr/local/nginx
```

2. 在网上下nginx包上传至Linux（https://nginx.org/download/），也可以直接下载

```java
wget https://nginx.org/download/nginx-1.19.9.tar.gz
```

3. 解压并进入nginx目录

```java
tar -zxvf nginx-1.19.9.tar.gz
cd nginx-1.19.9
```

4. 使用nginx默认配置

```java
./configure
```

5. 自定义编译生成目录（可以把所有资源文件放在指定的路径中，不会杂乱，方便卸载软件或移植软件）

```shell
./configure --prefix=/usr/local/test         #推荐使用这种方式
```

6. 编译安装

```java
make
make install
```

7. 查找安装路径

```java
whereis nginx
```

8. 进入sbin目录，可以看到有一个可执行文件nginx，直接**./nginx**执行就OK了。

```
./nginx
```

9. 查看是否启动成功

```java
ps -ef | grep nginx
```

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525122954107-155562798.png)

10. 然后在网页上访问自己的IP就可以了默认端口为80（出现如下欢迎界面就成功了！）

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525123013524-2133888199.png)

> **注意问题**

如以上步骤都完成且没有问题的话，就做如下操作

> 防火墙

```java
查看防火墙是否开启
systemctl status firewalld
```

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525123037019-946505205.png)

启动防火墙后，默认没有开启任何端口，需要手动开启端口。**nginx默认是80端口**

```Java
手动开启端口命令
firewall-cmd --zone=public --add-port=80/tcp --permanent
命令含义： --zone #作用域 --add-port=80/tcp #添加端口，格式为：端口/通讯协议 --permanent #永久生效，没有此参数重启后失效
```

开启后需要重启防火墙才生效

```java
systemctl restart firewalld.service
```

查看防火墙是否开启了80端口的访问

```java
 firewall-cmd --list-all
```

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525123123133-488824264.png)

**开启后再次访问！！**

> 端口占用

如果启动后出现了如下的问题就是**80端口**被占用

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525123156598-1768819004.png)

可以用下面这个命令进行查看80端口被谁占用

```java
netstat -tunlp | grep 80
```

![img](https://img2020.cnblogs.com/blog/2128586/202105/2128586-20210525123219035-1996362499.png)

这里因为我之前开启了的是被**nginx.master**或者**nginx.woeker**占用就不用管，如果不是这个的话那就把那个进程关闭掉

```
kill -9 进程号
```

**关闭之后重启nginx再次访问！！**

# 2. linux安装nginx(有编译环境)

## 参考

- https://www.cnblogs.com/pxstar/p/14808244.html

- https://blog.csdn.net/qq_38621978/article/details/103647225

- https://www.cnblogs.com/jingmoxukong/p/5945200.html

