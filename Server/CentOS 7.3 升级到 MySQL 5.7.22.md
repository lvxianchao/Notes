# CentOS 7.3 MySQL 升级到 5.7.22

今天需要在公司的一台 CentOS 7.3 服务器上搭建测试环境。上去一看 MySQL 的版本是 `5.6.20`，强迫症就犯了，果断升级。

## 1. 处理 MariaDB

```bash
# 检查 MariaDB 是否存在
sudo yum list installed | grep mariadb

# 如果有，卸载
sudo yum -y remove mariadb*
```

## 2. 处理 MySQL 的 yum 源

```bash
# 切换到目录到相应位置（一般这个位置用来存放用来自定义安装的软件包）
cd /usr/local/src

# 下载 MySQL 5.7.22 的源
sudo rpm -ivh mysql57-community-release-el7-11.noarch.rpm

# 检测是否安装成功
sudo yum repolist enabled | grep "mysql.*-community.*"
```
正常的话应该显示成这样：

![file](https://lccdn.phphub.org/uploads/images/201806/20/18848/a6VHQAROak.png?imageView2/2/w/1240/h/0)

## 3. 处理其它版本的源

```bash
# 查看本地镜像源
yum repolist all | grep mysql
```

可以看到有一堆，这是经过关闭掉 5.6 和 5.5 版本的源以后的最终效果，可以看到 5.7 的开着。

![file](https://lccdn.phphub.org/uploads/images/201806/20/18848/KfmUwtVbbu.png?imageView2/2/w/1240/h/0)

```bash
# 禁用 MySQL 5.5和5.6的源
sudo yum-config-manager --disable mysql55-community
sudo yum-config-manager --disable mysql56-community

# 启用 MySQL 5.7的源
sudo yum-config-manager --enable mysql57-community-dmr
```

> 执行 `yum-config-manager` 这个命令，需要已经安装了 `yum-utils`，如果因为这个命令报错，`yum -y install yum-utils` 安装一下就好。

## 4. 安装 MySQL

```bash
sudo yum install mysql-community-server
```

安装过程看网速，感觉很慢，然后就该干嘛干嘛了。

> 参考
>
> * https://my.oschina.net/CraneHe/blog/823684
> * https://blog.csdn.net/u011886447/article/details/79796802