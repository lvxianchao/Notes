# 简化 Homestead 操作

[Homestead](https://laravel.com/docs/5.6/homestead) 是 [Laravel](https://laravel.com/) 官方推荐的本地开发环境，但每次想要操作虚拟机的时候都需要切换到相应的目录下才能执行 [Vagrant](https://www.vagrantup.com/) 命令，比较麻烦，所以我们来简化一下，让我们可以任意目录下执行相关操作命令。

修改 **~/.bash_profile**，添加面下代码，如果安装了 [zsh](https://wiki.archlinux.org/index.php/Zsh_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))，则需要修改 `~/.zshrc` 文件。

```bash
function vm () {
    ( cd ~/Homestead && vagrant $* )
}
```

修改完以后，终端里执行 `source .zshrc` 使其立即生效，然后就可以在任意目录下使用 `vm up`，`vm halt` 之类的命令直接操作 Homestead 了。

`vm` 可以随意替换成你喜欢的命令，`~/Homestead` 一定要是你的 Homestead 脚本所在的位置，一般都放在主目录下。