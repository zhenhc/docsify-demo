## linux安装jdk

> 如果你使用的是CentOS的话，默认没有安装完整版的JDK，需要自行安装！

- 下载JDK 8，下载地址：https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/

![img](https://img-blog.csdnimg.cn/img_convert/4f5cab5d82bc329f2014cb79cb6f56ea.png)

- 下载完成后将JDK解压到指定目录；

```shell
cd /mydata/java
tar -zxvf OpenJDK8U-jdk_x64_linux_hotspot_8u312b07.tar.gz
mv OpenJDK8U-jdk_x64_linux_xxx.tar.gz jdk1.8
```

- 在`/etc/profile`文件中添加环境变量`JAVA_HOME`。

```shell
vi /etc/profile
# 在profile文件中添加
export JAVA_HOME=/mydata/java/jdk1.8
export PATH=$PATH:$JAVA_HOME/bin
# 使修改后的profile文件生效
. /etc/profile

# 查看jdk安装是否成功
java -version
```

### 参考链接

[吊炸天的 Kafka 图形化工具 Eagle，必须推荐给你！](https://blog.csdn.net/zhenghongcs/article/details/118539579)

