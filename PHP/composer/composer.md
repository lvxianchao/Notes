# Composer

# 工作原理

composer是一个依赖管理，类似于Mac下的应用商店。

开发者把其封装好的组件上传，其它的开发人员就可以通过composer直接下载这个插件，然后使用，及其方便。

但实际上，这些插件的真实存储的地方一般都是在代码版本控制库里，比如：github。

当我们需要一个插件然后通过composer安装的时候，首先会连接到composer的应用商店。

这个商店的地址：https://packagist.org

应用商店里检索到这个插件，获取其代码存储地址，然后再发送给我们。

# 创建composer.json文件及在packagist.org中提交项目

使用`composer init`命令来创建一个composer项目。

接下来的环节就按需求走就行了。

然后代码提交到版本库里，去packagist.org创建账号并登录，然后上传。

## composer整合github实现自动推送

[官方说明](https://packagist.org/about#how-to-update-packages)

github在仓库里选择settings，然后services，创建，填写信息。