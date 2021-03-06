# <script>标签

## 使用方式

在HTML中使用`JavaScript`有两种方式：

1. 使用`<script></script>`标签，直接书写在HTMl页面上。
2. 使用`<script src=""></script>`，引入外部源文件。

**如果引入外部源文件，则标签内不可再书写内容，浏览器会自支忽略。**

## 标签属性

### defer 延迟脚本

**立即下载，但延迟执行。**

可让页面在完全渲染完后，再执行解析脚本。

### async 异步脚本

**异步加载页面其它内容，防止阻塞。**

可以让页面加载无需等待脚本的下载和解析，继续向下渲染。

所以，异步脚本不能保证页面解析它们的顺序。

## <noscript>

当**浏览器不支持脚本**或**浏览器支持脚本，但脚本被禁用了**时，`<noscript>`标签内的内容就会显示出来，否则不会显示。