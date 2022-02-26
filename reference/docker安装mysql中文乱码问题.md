### docker安装mysql中文乱码问题

> 说明：如果已经创建好mysql容器，首先需要删除容器，然后重新根据镜像创建容器

1. 首先，进入安装mysql查看字符集：

```shell
docker exec -it [启动镜像时给镜像起的名字] /bin/bash

例如：docker exec -it mysql /bin/bash

mysql -uroot -p

show variables like 'char%';
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200813173321113.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY0NTYwMw==,size_16,color_FFFFFF,t_70#pic_center)

2. 在某个路径下创建一个my.conf文件，配置文件内容如下：

```shell
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

3. 启动mysql

```shell
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf/my.conf:/etc/mysql/my.cnf \     #这一行就是刚才在外部创建的配置文件映射到docker里面mysql的配置文件
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7
```

> **注意：在docker中的mysql的配置文件所在路径在 [/etc/mysql] 下，但是在该目录下，有 [conf.d my.cnf my.cnf.fallback mysql.cnf mysql.conf.d] 这么多文件，映射时候只有映射 [my.cnf] 这个配置文件才能生效，映射其他配置文件无效，请注意！！！**

4. 再次查看字符集，发现已经变为utf8

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200813174316806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY0NTYwMw==,size_16,color_FFFFFF,t_70#pic_center)

5. 如果还不行，需要把之前创建的数据库删除，重新导入数据

### 参考链接

- [docker安装mysql后插入中文乱码问题如何解决_米兰小码匠的博客-程序员信息网](https://www.i4k.xyz/article/weixin_43645603/107981930)
