# 安装配置

## 安装
TBD

## 简单配置

安装之后，在使用之前简单配置。

#### 增加日常使用的用户

默认安装后只有root账户，肯定不能直接用root。

因此增加一个日常使用的用户，这个用户需要拥有 sudo 的权限，以便在必要时可以得到 root 权限：

```bash
adduser sky
adduser sky sudo
```

通过 passwd 命令设置密码：

```bash
passwd sky
```

## 安装常用命令

- add-apt-repository

	会经常用到 add-apt-repository 命令，如果系统没有默认安装，可以通过下面的命令来安装：

	```bash
    apt-get install software-properties-common
    ```
