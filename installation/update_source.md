# 设置更新源

如果服务器在国内，则可以考虑设置apt源为国内代理，这样速度要好很多。

首先备份源列表:

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list_original
```

然后修改/etc/apt/sources.list文件:

```bash
TBD
```

最后执行 update 命令：

```bash
sudo apt-get update
```