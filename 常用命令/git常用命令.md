- 添加本地仓库

```shell
git add .
```

- 提交代码

```shell
git commit -m "提交信息"
```

- 关联一个远程仓库

```shell
git remote add origin git@server-name:path/repo-name.git
```

- 第一次推送到远程仓库

```shell
git push -u origin master
```

- 之后推送到远程仓库

```shell
git push origin master
```

- 查看远程仓库地址

```shell
git remote -v
```

- 修改远程仓库地址

```shell
git remote set-url origin git@github.com:zhenhc/mybatis-plus-generate.git
```
