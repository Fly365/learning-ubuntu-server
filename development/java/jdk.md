# 安装JDK

## apt-get 自动安装

直接 apt-get 安装最新版本的oracle jdk8：

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

中间选择接收 oracle 的协议，然后继续安装。(吐糟一下: oracle 服务器的下载速度真慢，即使是我放在国外的VPS，号称千兆网络，也只有300-500k的速度......)

如果安装有多个版本的 JDK，可以用下面的命令来设置默认的JDK：

```bash
sudo apt-get install oracle-java8-set-default
```

安装完成之后运行 `java –version` 查看安装结果：

```bash
# java -version
java version "1.8.0_101"
Java(TM) SE Runtime Environment (build 1.8.0_101-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.101-b13, mixed mode)
```

此时java命令是通过将 /usr/lib/jvm/java-8-oracle/jre/bin/java 文件一路链接到 /etc/alternatives/java 和 /usr/bin/java 的：

```bash
root@sky2:~# which java
/usr/bin/java
root@sky2:~# ls -l /usr/bin/java
lrwxrwxrwx 1 root root 22 Oct  5 08:40 /usr/bin/java -> /etc/alternatives/java
root@sky2:~# ls -l /etc/alternatives/java
lrwxrwxrwx 1 root root 39 Oct  5 08:40 /etc/alternatives/java -> /usr/lib/jvm/java-8-oracle/jre/bin/java
```

并且没有设置 JAVA_HOME。推荐设置好 JAVA_HOME 并将 JAVA_HOME/bin 目录加入PATH，修改/etc/profile文：

```bash
# java
export JAVA_HOME=/usr/lib/jvm/java-8-oracle/
export PATH=$JAVA_HOME/bin:$PATH
```

再执行 `source /etc/profile` 载入即可。

## 手工安装JDK

如果手工安装，可以从oracle网站下载安装文件，如jdk-9u90-linux-x64.gz（对于ubuntu不要下载rpm版本）。

手工解压缩到安装路径:

```bash
gunzip jdk-9u90-linux-x64.gz
sudo mkdir /usr/lib/jvm
sudo mv jdk-9u90-linux-x64 /usr/lib/jvm/jdk8
```

JAVA_HOME 和 PATH 设置同上。

## 参考资料

- [https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04)