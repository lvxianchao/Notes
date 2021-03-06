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

Linux下直接将启动SVN服务器的命令：`svnserve -d -r 版本库位置`，放入到`/etc/rc.local`文件的`exit 0`命令前即可。

## 基本操作

### 常见术语及文件状态

* 常见术语

  版本库、检出、工作副本、更新、提交、版本、版本号

* 文件状态

  无版本控制、增加、修改、常规、冲突、删除、锁定



从版本库检出工作副本，工作副本为常规状态。

工作副本做出修改，工作副本为无版本控制状态。

对无版本控制状态文件进行增加操作，工作副本为增加状态。

对增加状态的工作副本进行提交操作（commit），版本库的版本号+1，并返回当前版本号给客户端，工作副本为常规状态。

### checkout和export的区别

|              | 字面意思 | 是否纳入版本控制 |     组成      |
| :----------: | :--: | :------: | :---------: |
| **checkout** |  检出  |    是     | .svn + 项目文件 |
|  **export**  |  导出  |    否     |    项目文件     |

* `.svn`记录着工作副本**最后一次更新**后的文件状态和工作副本的**一切变化**。
* 它们有很多共同的参数，如：`-r`指定检出/导出的项目版本。
* `checkout`可以简写成：`co`。

### add, ci, up, del, up

* `svn add` — 添加到版本控制。

  * `svn add file-name` 将指定文件纳入到版本控制。 

  * `svn add dir-name` 将文件夹纳入到版本控制。

    > 会递归遍历该文件夹下所有文件并纳入版本控制，如果只想添加文件夹而不添加其下的其它文件可以使用`--non-recursive`参数。

  * `svn add *` 将所有文件纳入到版本控制。

    > 如果有使用了`--non-recursive`的文件侠，其下的文件并不会纳入到版本控制，如果想纳入版本控制，可以使用`--force`参数强制纳入。

* `svn commit` — 提交修改到服务端

  * 可以简写成：`svn ci`
  * 必须要加上`- m`参数，后面跟上本次提交的概述，如：`svn commit -m '添加了某某功能'`。
  * 这个命令会使版本库的版本号`＋1`

* `svn update` — 更新工作副本

  * 可以简写成`svn up`。
  * `svn up -r 版本号 文件名`，如：`svn up -r 1 index.php`就会看到`index.php`这个文件在版本号为`1`的时候的内容。

* `svn delete` — 从版本库中删除文件或目录。

  * 别名：`svn del`、`svn remove`、`svn rm`。
  * 如果直接使用系统命删除要删除的文件，并不会在版本库里真正地删除。即使使用系统命令删除，再执行提交操作也不可以，删除文件这个操作不会被纳入版本控制，所以如果想从版本控制中删除文件就要使用这个命令（这一点和Git有所差别）。

### diff, mkdir, cat

* `svn diff` — 版本差异比较。

  * 简写：`svn di`

  * 比较某个文件的差异：`svn di file-name`。

    > 如：`svn di index.php`比较工作副本中`index.php`文件与**最后一次更新**的版本的差异。

  * 比较某个文件与某一版本的差异：`svn di -r 版本号 file-name`。

    > 如：`svn di -r 3 index.php`，比较工作副本中`index.php`文件与**版本号为2**时的内容差异。

  * 比较两个历史版本的差异：`svn di -r 旧版本号:旧版本号 文件名`。

    > 如：`svn di -r 1:3 index.php`，比较`index.php`这个文件的`1`版本和`3`版本的内容差异。

  * 如果什么参数都不加，会直接比较所有有差异的文件的内容差异。

* `svn mkdir` — 创建目录并增加到版本管制。

* `svn cat` — 不检出工作副本直接查看指定文件。

  > 这个命令可以在脱离工作副本的情况下远程查看版本库中文件的内容：`svn cat 版本库地址 文件名`。
  >
  > 如：`svn cat svn://127.0.0.1/test/index.php`会查看`test`版本库中`index.php`文件的内容。

### 工作副本还原：revert

* 使用`svn revert 文件名`命令可以将文件恢复到最近一次更新的状态。

* 批量还原：`svn revert *`。

  > 这个命令只会还原当前目录下有修改的文件，并不会递归还原。

* 递归批量还原：`svn revert --recursive *`。

### 二进制冲突与树冲突

#### 什么是冲突以及冲突的产生条件

##### 产生原因

更新到的数据与工作副本的修改正好在同一处。

##### 产生时机

冲突通常出现于工作副本长时间未更新的情况下。

##### 二进制冲突

如果像代码文件这种文件冲突，冲突会指定到某一行。也就是说，能够精确指明冲突所在位置的冲突，叫做**二进制冲突**。

##### 树冲突

像图片这种文件，产生冲突后，无法指定到产生冲突的精确位置的冲突，叫做**树冲突**。

也就是说：

* 发生树冲突的文件都**不是**二进制文本文件。
* 树冲突无法精确到行，并且处理树冲突必须处理整个文件。

#### 如何尽可能的避免冲突

经常更新代码。

#### 如何解决冲突

* 处理时机有两种：`立即处理`和`推迟处理`。

* 文件冲突时，svn会给出几种处理冲突的选项包括：

  * `mc` — 保留我自己的代码。

  * `tc` — 保留对方的代码。

  * `p`　— 推迟处理。

    > **推迟处理**会将冲突代码保留在冲突的文件内。
    >
    > 之后如果想去解决冲突，可以使用命令：`svn resolve 冲突文件名`，之后svn会继续提供之前的处理冲突的选项。

* 如果冲突已经解决了，需要告诉版本库已解决冲突。命令：`svn resolved 文件名`。

* 提交代码。

### 锁定与解锁

* `svn lock` — 锁定文件，防止其他成员对文件进行提交。
* `svn unlock` — 解锁文件。

锁定与解锁可用于防止发生冲突，但不够灵活，而且缺点也很明显。

当我使用了`svn lock index.php`命令对`index.php`文件进行锁定，别人可以对这个文件进行编辑，但却不能提交。

别人在提交的时候svn会提示，这个文件被我给锁定了。

所以，如果我们两个修改的是这个文件里的不同地方时，就会影响开发效率。

这时，触发解锁有两种机制：

* **锁定方（我）**使用解锁命令进行解决操作：`svn unlock index.php`。
* **锁定方**对这个文件进行了编辑操作，并提交。提交操作会直接触发解锁这个文件。

如果我在对这个文件进行了编辑操作，并提交的时候，不想解锁这个文件，可以在提交的时候添加：`--no-unlock`参数。

> 对文件进行锁定这种操作还是谨慎为好，并不推荐这种操作，**珍爱生命，远离锁定**。

## 进阶

* `svn list` — 列出当前目录下**处于版本控制的**所有文件。

  * 简写：`svn ls`，不会递归列出文件夹下的文件。
  * 递归：`svn ls --recursive`。
  * 显示详细信息：`svn ls -v`。

* `svn status` — 列出工作副本的状态。

  * 简写：`svn st`。
  * 这个命令会给出有改动的文件及其状态标记。
    * `A` — 被纳入版本库的文件。
    * `D` — **纳入**版本库的已删除的文件（用`svn rm 文件`命令删除的文件会显示成这个标记）。
    * `!` — 文件缺失（用系统的删除命令的文件会显示成这个标记）。
    * `M` — 修改过的文件。
    * `?` — **未纳入**版本库的文件。
    * `R` — 被替换的文件。
    * `C` — 文件存在冲突。

* `svn log` — 查看提交日志。

  后接文件名可以指定查看的文件。

* `svn info` — 工作副本及文件/文件夹的详细信息。

  后接`--xml`可以显示成XML格式。

### 多版本库解决方案

当我们只有一台SVN服务器，但想要运行多个版本库的时候，就需要采取手段，使得SVN服务器同时监听多个版本库。

有两种解决方案：

* **多端口监听**

  SVN服务默认监听`3690`端口，所以当我们如果运行了一个SVN服务，再想去运行另一个SVN服务的时候，就会提示端口已被占用。

  ​

  所以，当我们想再运行一个SVN服务的时候，可以明确指定服务所运行的端口来避开这个问题，命令：`svnserve -d -r 版本库位置 --listen-port 端口号`。

  ​

  这样运行的服务，当别人想检出这个版本库的时候，也需要加上端口号。

  命令：`svn co svn://SVN服务器地址:端口号/版本库位置`。

* **只使用一个端口号**

  如果想只占用一个端口号来运行多个SVN服务该怎么做？

  我们可以把多个版本库的**父级目录**用来SVN服务的根目录，在运行SVN服务的时候指定这个目录为监听目录。

  这样，在检出的时候，只要把版本库名称路径做相应修改即可。

  |       |      优点       |   缺点    |
  | :---: | :-----------: | :-----: |
  | 多个端口号 | 版本库可以创建在任意位置  | 端口号容易混淆 |
  | 一个端口号 | 版本库必须创建在同一目录下 | 无需分配端口号 |


### Copy

* 简写：`svn cp 源文件名 目标文件名`
* 复制指定版本：`svn cp -r 版本号 源文件名 目标文件名`
* ​

### 高级













