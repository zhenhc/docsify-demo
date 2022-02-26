- 查找并执行历史命令记录

```shell
history | grep 'config'

! + 你想重用的命令编号即可运行该命令
```

- 如果觉得 `history` 加管道加 `grep` 这样打的字还是太多，可以在 你的 shell 配置文件中（`.bashrc`，`.zshrc` 等） 中写这样一个函数：

```shell
his()
{
    history | grep "$@"
}
```


