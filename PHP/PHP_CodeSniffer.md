# （2017-11-19）PHP_CodeSniffer

> [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) 是一个根据给定的标准来检查代码质量的工具。

这个工具包括两个脚本：`phpcs`，`phpcbf`。

- **phpcs** 检测代码质量。
- **phpcbf** 自动修正有问题的代码。

## 安装

官方给出的安装方式有三种：**下载安装**，**使用`pear`安装**，**使用`composer`安装**。

这里只记录下载安装和Composer安装

#### 下载安装

```
curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar
php phpcs.phar -h

curl -OL https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar
php phpcbf.phar -h
```

然后将你下载到的这两个二进制包放到`/usr/local/bin/`路径下。

重命名为：`phpcs`和`phpcbf`。

再给予它们可执行权限，就可以正常使用了。

#### 通过Composer安装

> 前提需要你的环境上安装了**Composer**。

> 如果你还没有安装，可以去[Laravel - China社区](https://laravel-china.org/composer)查看如何安装并使用国内镜像。

执行`composer global require "squizlabs/php_codesniffer=*"`会使用Composer进行全局安装。


安装完成后，这两个命令并不能直接执行。

在Mac上，他们在`~/.composer/.vendor/bin/`路径下。

你需要将这个路径添加进环境变量里，然后就可以正常使用了。

[Mac - 设置环境变量](../Mac/环境变量.md)






