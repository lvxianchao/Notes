# Laravel-Vue

## 使用Tinker和Factory构建管理中初始数据

1. 创建admins数据表，创建admins模型，生成迁移

    php artisan make:model Models/Admin -m

2. 编辑生成出来的迁移文件，添加相应字段
3. 编辑ModelFactory.php，复制一份代码到下面，改一下字段和模型类 
4. 进入tinker环境：php artisan tinker
5. 执行factory(App\Models\Admin::class, 50)->create();

## 子目录域名绑定与创建后台登录控制器

1. 宝塔面板里设置入口目录和伪静态
2. 创建路由组，声明命名空间
3. 生成控制器：php artisan make:controller Admin/EntryController

## 视图处理与hdjs框架下载安装 

1. cnpm install hdjs,去把后盾cms的模板弄过来，搞到image和css
2. 视图创建：public>view>admin>entry>login.blade.php
3. 把node_modules弄到public里

## 使用用户认证系统与独立设置guard进行登录处理

## 使用中间件middleware进行登录权限验证

php artisan make:middleware AdminMidlleware

中间件里引用Auth;

if(!Auth::guard('admin')->check()){
    return redirect(/admin/login);
}

把中间件挂到挂载点上

Kernel.php

$routeMiddleware

'admin.auth' => AdminMiddleware::class,

在要用的地方的构造函数里

$this->middleware('admin.auth')->except([
    'loginForm', 'login'
]);

## 阿里云OSS存储开发配置

阿里云OSS，选择新建Bucket，标准存储，公共读

---

找到新建的bucket，基础设置，下面的跨域设置，创建规则。

来源：*
allowed methods: 全允许

---

找到新建的bucket，访问控制，用户管理，新建用户，勾选：为该用户自动生成AccessKey

保存好新得到的key

获取bucket的外网域名

---

使用hdjs，oss，新建控制器把js里的需求填到里面，给控制器添加路由

在阿里云，给角色授权，搜索oss，AliyunOSSFullAccess

## vuejs结合hdjs实现多视频上传并有进度显示

## vue-cli 初始项目与解决phpstorm文件大时不响应

在node_modules文件夹上右击，选择Mark Directory as -> Excluded

---

上传文件的话需要给PHP安装fileinfo扩展，在宝塔面板里可以直接安装。

如果安装失败的话，在工具箱里把swap分区调整成1G

前端的话可以使用hdjs完成上传功能。

在后台需要建立component目录，在目录下创建UploaderController用来处理文件上传。

---

Laraver 处理多对多的时候，需要在模型里处理。

--- 

小技巧：

在写控制器的时候，可以新建一个CommonController，在里面声明常用的方法，比如：

success() 和 error() 用来返回响应。

laravel里直接return数组就是json格式。

---

可以使用vue-awesome-swiper插件来处理幻灯片。

使用 vue-axios 处理异步请求。

跨域问题可以在控制器里设置header解决。

---

打包成APP可以使用APICloud