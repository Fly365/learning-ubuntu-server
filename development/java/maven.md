# Maven

## 安装 Maven

[参考地址](http://www.sysads.co.uk/2014/05/install-apache-maven-3-2-1-ubuntu-14-04/)，步骤如下：

1. Remove older version
2. Add following lines to sources.list
3. Update Repository and Install
4. Add Symbolic Link

具体执行命令依次如下：

```bash
sudo apt-get remove maven2
sudo add-apt-repository "deb http://ppa.launchpad.net/natecarlson/maven3/ubuntu precise main"
sudo apt-get update
sudo apt-get install maven3
sudo ln -s /usr/share/maven3/bin/mvn /usr/bin/mvn
```

安装完成之后，执行 `mvn --version` 查看：

```bash
# mvn --version
Apache Maven 3.2.1 (ea8b2b07643dbb1b84b6d16e1f08391b666bc1e9; 2014-02-14T17:37:52+00:00)
Maven home: /usr/share/maven3
Java version: 1.8.0_101, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-8-oracle/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "2.6.32-042stab113.11", arch: "amd64", family: "unix"
```

## 安装 Artifactory

从[https://www.jfrog.com/open-source/](https://www.jfrog.com/open-source/)下载到最新的artifactory，将zip包解压。

> 注：万一jfrog的网站无法打开，可以使用地址 https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip 来下载。

安装前需要确保 JAVA_HOME 有正确设置。最新版本的artifactory会要求至少jdk1.8版本, 否则无法启动.

将目录复制到/usr/lib，执行安装:

```bash
sudo mv artifactory*** /usr/lib/
cd /usr/lib/
sudo mv artifactory*** artifactory
cd artifactory/bin
sudo ./installService.sh
service artifactory check
sudo service artifactory start
```

安装成功后就可以通过[http://localhost:8081](http://localhost:8081)访问artifactory的页面了，默认管理员账号和密码为 admin/password 。

[参考地址](http://www.softwarepassion.com/install-artifactory-on-ubuntu-box/)。

安全起见，登录后先修改admin密码，点击Admin -> Security -> Users -> User List -> admin，修改密码即可。

另外，Admin -> Security -> General, 取消"Allow Anonymous Access"。

### 配置maven

为了让mvn能连接到本地的artifactory，需要配置maven。

修改maven的配置文件，全局配置文件在maven3安装路径 /usr/share/maven3/conf/ 下，需要更新server配置信息和profile 配置信息。

server配置段：

	  <servers>
	   <server>
	      <username>****</username>
	      <password>****</password>
	      <id>releases</id>
	    </server>
	    <server>
	      <username>****</username>
	      <password>****</password>
	      <id>snapshots</id>
	    </server>
	  </servers>

profile配置段：

	  <profiles>
	    <profile>
	      <repositories>
	        <repository>
	          <snapshots>
	            <enabled>false</enabled>
	          </snapshots>
	          <id>releases</id>
	          <name>libs-release</name>
	          <url>http://localhost:8081/artifactory/libs-release</url>
	        </repository>
	        <repository>
	          <snapshots />
	          <id>snapshots</id>
	          <name>libs-snapshot</name>
	          <url>http://localhost:8081/artifactory/libs-snapshot</url>
	        </repository>
	      </repositories>
	      <pluginRepositories>
	        <pluginRepository>
	          <snapshots>
	            <enabled>false</enabled>
	          </snapshots>
	          <id>releases</id>
	          <name>plugins-release</name>
	          <url>http://localhost:8081/artifactory/plugins-release</url>
	        </pluginRepository>
	        <pluginRepository>
	          <snapshots />
	          <id>snapshots</id>
	          <name>plugins-snapshot</name>
	          <url>http://localhost:8081/artifactory/plugins-snapshot</url>
	        </pluginRepository>
	      </pluginRepositories>
	      <id>artifactory</id>
	    </profile>
	  </profiles>

	  <activeProfiles>
	    <activeProfile>artifactory</activeProfile>
	  </activeProfiles>