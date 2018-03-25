# Require.js

> 前端模块化思想与AMD

## 目录结构与main.js配置说明

引用户require.js，属性data-main里声明配置文件的源

## paths与baseUrl配置项

## 解决页面加载时模块加载失败的三咱解决办法

* 把模块加载代码写在config.js里
* require.js和config.js分成两个script标签进行加载，并先加载require.js

# 使用define自定义模块

在config里定义好文件路径，然后在相应的路径下定义文件，文件名和模块名保持一致。

使用define函数定义模块，第一个参数为一个数组，里面包含了当前模块所要依赖的模块，第二个参娄为闭包。

闭包里返回一个对象，对象内包含了所要定义的模块的所有方法。

## 多个模块间的依赖关系

在config里的shim属性定义依赖关系，

```js
require.config({
    path: {
        // 加载CSS文件需要用到的库
        css: '../lib/css.min',
    }
    // 声明依赖关系
    shim: {
        声明使用bootstrap模块时的依赖关系
        bootstrap: {
            // 声明css文件时，需要用到css加载库，写法为:css!后面跟需要引入的css文件的路径，并且带后缀名。
            deps: ['jquery', 'css!../css/bootstrap.min.css']
        }
    }
});
```

## 处理非标准化的模块

当我们需要加载一些非标准化的写法模块的时候，需要声明。

在shim里声明

```js
shim: {
    'old': {
        // 第一个种方法，如果只有一个函数。
        exports: 'oldMethod',

        // 第二种方法：如果一个文件里有多个函数。
        init: function () {
            return {
                oldMethodOne: oldMethodOne,
                oldMethodTwo: oldMethodTwo
            }
        }
    }
}
```