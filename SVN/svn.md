# SVN 使用

标签： SVN 版本控制 开发工具

---

## SVN是什么

### 简介

### 简单比较

CVS，SVN，GIT

|  特性  |  CVS  | SVN  | GIT  |
| :--: | :---: | :--: | :--: |
| 并发修改 |  支持   |  支持  |  支持  |
| 并发提交 |  不支持  |  支持  |  支持  |
| 历史轨迹 | 不支持更名 | 支持更名 | 支持更名 |
| 分布式  |  不支持  | 不支持  |  支持  |



## SVN服务端及客户端环境搭建

#### SVN安装

* Ubuntu 16.04

  `sudo apt install subversion`

* Mac OS

  `brew install subversion`

Subversion软件包已经包含服务端和客户端

#### 服务端命令与客户端命令

##### 服务端

* svnserver - 控制svn系统服务的启动等。
* svnadmin - 版本库的创建、导出、删除等。
* svnlook - 查看版本库的信息等。

##### 客户端

* svn - 版本库的检出、更新、提交、重定向等。

#### 版本库的创建与删除

##### 创建

`svnadmin create [arg] /path/repos`

###### 参数

* `--fs-type`
  * `dbd`
  * `fsfs`

##### 删除

`rm -rf ` 都懂的。

#### 版本库配置及权限分组

配置文件位于版本**conf**文件夹下。

* **authz** — 配置用户组以及用户组权限。
* **passwd** — 配置用户名和密码。
* **svnsever.conf** — 配置默认权限、权限配置文件及密码配置文件。

##### svnserver.conf

`read`、`write`、`none`

* `anon access = read` 未经授权的用户权限
* `auth access = write` 已授权的用户权限
* `password-db = passwd` 指定**用户名和密码配置文件**的路径，支持绝对和相对路径。
* `authz-db = authz` 指定**权限分组配置文件**的路径，支持绝对和相对路径。

##### passwd

有样本，直接复制修改就好。

##### authz

按分组指定权限

![分组](https://ws4.sinaimg.cn/large/006tKfTcly1fo1e4rog2ij30bk056wey.jpg)

按目录指定权限

![按分组指定](https://ws2.sinaimg.cn/large/006tKfTcly1fo1e5rdi5xj3086056jrk.jpg)

按项目目录指定权限

![按项目目录指定权限](https://ws1.sinaimg.cn/large/006tKfTcly1fo1e6wzr7lj308209274w.jpg)

#### 版本库访问

启动SVN服务：`svnserve -d -r [path/repos]`

检出仓库：`svn co svn://ip-adress --username username --password password`.

`co`是`checkout`的简写。

#### SVN服务自启动

## 基本操作

## 进阶

## 高级









