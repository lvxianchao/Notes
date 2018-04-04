# Vue + Webpack

`npm init`

`npm install webpack vue vue-loader`

`src`目录下新建vue组件文件写入内容。

新建`webpack.config.js`

配置为：

```js
// 提取一个用来处理路径的包
const path = require('path');

modules.exports = {
    // 声明入口
    entry: path('index.js'),
}
```