# Mac 下安装 MySQL5.7

[官方下载](https://dev.mysql.com/downloads/mysql/)

安装以后会在**系统偏好设置**的面板里出现一个**MySQL**。

点击以后是个很简洁的窗口，服务会自动启动。

![MySQL](https://ws4.sinaimg.cn/large/006tNc79ly1fpjrajqkmaj31140x8jta.jpg)

## 配置一下环境变量

安装完成以后命令是在`/usr/local/mysql/bin`目录下，如果想在命令行里访问的话多费劲。

下面来添加一个环境变量。

首先分清你用的什么Shell。

终端里输入：`echo $SHELLL`。

如果结果是：`/bin/zsh`，那恭喜，这是个好东西。

但如果结果是：`/bin/bash`，兄弟，要不要试试`zsh`？

要是其它的自行搜索，我也不知道。

如果是`zsh`:

* `vim ~/.zshrc`
* 找个你觉得顺眼的地方把`export PATH=$PATH:/usr/local/mysql/bin`加进去。
* 退出编辑，`source ~/.zshrc`
* 重启终端。

如果是`bash`，把文件换成`.bash_profile`，其它都一样，顺便向你推荐一波`zsh`，不谢。

---

安装完成后，会弹出一个窗口并告诉你账号和密码，那密码那个费劲，当然要改。

## 第一步

先把服务给关掉。

## 第二步

终端里输入：`sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables`。

再开一个窗口，输入：`sudo /usr/local/mysql/bin/mysql -u root`。

这时候应该已经进入MySQL了。

输入：`update mysql.user set authentication_string=PASSWORD('你的密码’) where user='root';`

完。

