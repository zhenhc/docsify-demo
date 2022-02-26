- 下载，官网：

```
http://nginx.org/en/download.html
```

![](../图片/微信截图_20211129135801.png)

- 启动nginx

有很多种方法启动nginx

(1)直接双击nginx.exe，双击后一个黑色的弹窗一闪而过

(2)打开cmd命令窗口，切换到nginx解压目录下，输入命令 nginx.exe 或者 start nginx ，回车即可

- 关闭nginx

如果使用cmd命令窗口启动nginx，关闭cmd窗口是不能结束nginx进程的，可使用两种方法关闭nginx

(1)输入nginx命令  nginx -s stop(快速停止nginx)  或  nginx -s quit(完整有序的停止nginx)

(2)使用taskkill  taskkill /f /t /im nginx.exe

### 参考

[windows下nginx的安装及使用](https://www.cnblogs.com/jiangwangxiang/p/8481661.html)

[windows 下 nginx 配置文件路径](www.zhipin.com/web/geek/resume)