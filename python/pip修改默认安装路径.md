- 查看是否安装pip

```
pip --version
```

- 使用命令查看pip默认安装目录

```
python -m site
```
- 使用命令`python -m site -help`找到site.py配置文件路径

- 修改site.py配置文件中的`USER_SITE`和`USER_BASE`属性的值
> `USER_SITE`:pip install下载的第三方包的安装路径  
`USER_BASE`:pip自动下载的脚本路径

ep：
```
USER_SITE = "E:\Python\Python310\Lib\site-packages"
USER_BASE = "E:\Python"
```

- 使用pip安装和查看第三方包的安装路径

```
pip install redis
pip show redis
```

参考：

[PIP INSTALL 默认安装路径修改](https://www.cnblogs.com/yinliang/p/12931685.html)

