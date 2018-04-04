# 记录来自codecasts.com

## 第一节

* 安装Laravel安装器

    `composer global require "laravel/installer"`

* 配置环境变量

    `PATH=$PATH:$HOME/.composer/vendor/bin`

* 使用Laravel安装器初始化一个项目

    `laravel new spa-vue`

* 安装依赖

    `npm install`

* 安装vue-router

    `npm install vue-router --save`

* 在`/resource/assets/sass/app.scss`里去掉google的文件引用。

* 在`app.js`里引用vue-router

    `import VueRouter from './vue-router'`

    `Vue.use(VueRouter)`

* 创建路由文件routers.js

    ```js
    let routes = [
        {
            path: '/',
            component: require('./components/Home');
        }
    ];

    export default new VueRouter({
        routes
    });
    ```

* app.js

    先删除组件导入。

    ```js
    // 导入路由
    import router from './routes.js'

    // 在Vue根组件里引用路由
    const app = new Vue({
        el: '#app',
        router
    });
    ```

* 创建视图
  * 在视图文件夹下创建`layouts`文件夹，在其下创建`master.blade.php`。
  * 引入`/app/app.js`和`/css/app.css`。
    * 创建挂载节点
        ```html
        <div id="app">
            <router-view></router-view>
        </div>
        ```
* 更改laravel的根路由，返回master视图。

* 去掉#
  * `router.js`里
        ```js
        export default new VueRouter({
            // 添加这一行
            mode: 'history',
            routes
        });
        ```
  * `web.php`路由文件里
        ```php
        Route::any('{all}', function () {
            return view('layouts.master');
        });
        ```

## 第二节：与后端交互

生成数据

// 创建文章模型并生成迁移

`php artisan make:model Post -m`

// 创建模型的工厂

`php artisan make:factory PostFactory --model=Post`

// 改写迁移文件：`CreatePostsTable`

添加字段：

    ```php
    $this->string('title');
    $this->text('body');
    $this->unsignedInteger('user_id');
    ```

// 改写迁移文件`CreateUsersTable`

添加字段：

`$this->boolean('is_active')->default(false);`

用来判断用户是否登录。

// 创建数据库：

```sql
database: spa
user: root
password: root
```

// 执行数据迁移

`php artisan migrate`

// 填充数据`PostFactory.php`

```php
return [
    'title' => $faker->sentence,
    'body' => $faker->paragraph,
    'user_id' => factory(App\User::class)->create()->id
];
```

// 改写模型：`Post.php`

```php
protected $fillable = [
    'title', 'body', 'user_id'
];
```

// 进入tinker

`php artisan tinker`

// 填充数据

factory(App\Post::class, 50)->create();

数据生成完毕。

---

// 创建post控制器

`php artisan make:controller PostsController`

// 定义两个方法：index, show

```php
public function index ()
{
    // 返回10条记录
    return Post::paginate(10);
}


public function show (Post $post)
{
    return $post;
}
```

// API路由`api.php`

```php
Route::get('/posts', 'PostsController@index');
Route::post('posts/{id}', 'PostsController@show');
```

// Home.vue

挂载事件里请求数据

```js
mounted () {
    axios.get('/api/posts').then(response => {
        this.posts = response.data.data;
    });
},

data () {
    return {
        posts: [],
    }
}
```

改写组件模板，遍历数据，将文章显示出来。

处理文章详情页。

---

## 第三节：重构Vue组件目录

重新组织一下目录结构。

## 第四节：用户注册

创建register目录，创建Register.vue和RegisterForm.vue。

RegisterForm.vue里提交表单数据到`/api/register`。

收到响应后，跳转到邮箱激活`confirm`路由：`this.$router.push({name: 'confirm'});`，并注册这个路由，编写对应的组件。

---

创建`register`路由，指向`Auth\RegisterController@register`。

在`Http/Controllers/Auth/RegisterController.php`里，把`RegistersUsers`这个`trait`里的`register`方法拿过来，删除掉没用的部分。并在头部引入`Request`，和`Registered`。

返回一个响应:`return response()->json(['status'=>true, 'msg'=>'注册成功']);`

去掉`validator`方法里密码的`confirm`

## 第五节：Vue里的表单验证

使用`vee-validate`插件：`npm install vee-validate --save`。

```js
import VeeValidate from 'vee-validate';

Vue.use(VeeValidate);
```

---

在`RegisterForm.vue`里，使用属性`v-validate`进行验证。

```html
<div class="form-group row" :class="{'has-error': errors.has('name')}">
    <label for="name" class="col-md-4 col-form-label text-md-right">用户名</label>
    <div class="col-md-6">
        <input v-model="name" v-validate="{rules: {required: true, min: 3}}" id="name" type="text" class="form-control" name="name" autofocus>
        <span class="text-danger" v-show="errors.has('name')">{{ errors.first('name') }}</span>
    </div>
</div>
```

bootstrap4的表单验证类无效，自行添加。

```css
.has-error{
    color: #dc3545 ;
}
.has-error .form-control {
    border-color: #dc3545 ;
}
```

## 第六节：验证消息自定义

## 第七节：使用Laravel Passport配置后端OAuth服务

SPA应用使得传统后端登录以session验证的方式不再适用，一般用OAuth解决。

我们需要在后端布署一个OAuth Server，并使用JWT来完成这一功能。

在Laravel中使用官方的passport包来完成这一功能。

`composer require laravel/passport`

`php artisan mirgrate`