# Vue.js

`v-text`指令可以防止vue的大胡子语法和后台框架冲突。

`v-html`可以用来原样输出html代码。

`v-bind`可以将属性绑定到变量

`v-bind:class="var"` 绑定`class`属性给var变量

`v-bind`简写成`:`

`:class="'var'"`其中的`var`为实体字符，不再解析成vue的变量。

`v-model`数据双向绑定的指令，数据绑定到属性，同时又绑定到输入的数据

`v-once`只绑定一次数据，下次更改变量的值时视图

`v-text`指令可以防止vue的大胡子语法和后台框架冲突。

`v-html`可以用来原样输出html代码。

vue的`{{}}`里可以放入表达式。

`computed`为计算属性，其中如果想引用vue里的变量，需要指定前提对象`this`

```
// 计算属性：求两个数的和。
var app = new Vue({
    el: '#app',
    data: {
        a: 0,
        b: 0
    },
    computed: {
        sum: function () {
            return parseInt(this.a) + parseInt(this.b);
        }
    }
});
```

`watch`可以用来监听变量的更改。

发送ajax请求可以使用`axios`库，安装方法：`npm install axios`。

```
// 模拟请求后台服务器
// 在文本框里输入数据，由于文本框通过word双向绑定，这时监听属性会触发word方法，并自动传入文本框的值更改前的值和新的值，接收到这两个参数后，使用axios库向后台发送请求并处理返回的数据。
<body>
    <main id="app">
        <input type="text" v-model="word">
        <div v-text="result"></div>
    </main>
</body>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            word: '',
            result: ''
        },
        watch: {
            word: function (newV, oldV) {
                axios.get('watch.php?word=' + newV).then(function (response) {
                    return app.result = response.data;
                });
            }
        }
    });
</script>
```

---

使用`lodash`库减少请求，减轻服务器的压力。
安装方法：`npm install lodash`

```
var app = new Vue({
    el: '#app',
    data: {
        word: '',
        result: ''
    },
    watch: {
        word: _.debounce(
            function (newV, oldV) {
                axios.get('watch.php?word='+newV).then(function (response) {
                    app.result = response.data;
                });
            }, 500
        )
    }
});
```

`v-bind`可以将属性节点绑定给vue属性，这个属性可以是字符串，数组，或者对象，如果是对象的话，属性为真的才会生效。

`v-bind`里也可以使用表达式，例如三元运算符。

`v-if` `v-else-if` `v-else` 循环判断。

可以用`<template></template>`标签装载空的模板。

为了使不同的的表单区分，可以添加`key`属性（添加不同的name属性可能做到啊？为什么多此一举）

`v-show`也可以隐藏元素，但实际上是给元素添加style属性：`display: none`，并不会真的移除节点。

但`v-if`会真的移除掉节点。

`v-for`用来循环：

```vue
// items 为要遍历的变量，通常是一个数组。
// item 为当前遍历的一条数据。
v-for="item in items"

// 如果想启用当前遍历的索引，可以将其指定给一个变量。
// index 为当前的索引。
v-for="(item, index) in items"

// 如果遍历的数据是一个对象，还可以接收第三个参数
// item 为当前遍历的属性，key 为键，index 为索引。
v-for="(item, key, index) in items"

// 也可以直接遍历一个数字，表示循环。
// i分别会被指定为1到100的整数数字。
v-for="i in 100"
```

`v-for` 和 `computed`计算属性结合使用：

```
<body>
    <main id="app">
        <ul>
            // 遍历计算属性students, students这个数组是动态的，受到下方的单选按钮影响。
            <li v-for="user in students">
                {{ user.name }} -- {{ user.sex }}
            </li>
        </ul>

        <input type="radio" value="girl" v-model="sex">　女孩
        <input type="radio" value="boy" v-model="sex">　男孩
    </main>
</body>
<script>
    let app = new Vue({
        el: '#app',
        computed: {
            // 计算属性students的实体。
            // 如果性别为all(初始化的时候), 则返回所有学生。
            // 如果指定了性别(通过单选按钮), 则将所有学生信息的属性users过滤，选择性别与当前选择的性别相同的学生信息组成数组反回。
            // 这样，VUE的计算属性改变了，便会重新渲染遍历的节点，此时数组里只有学生的性别为选中的性别的学生信息。 
            students () {
                if (this.sex === 'all') {
                    return this.users;
                } else {
                    return this.users.filter(v => v.sex === this.sex);
                }
            }
        },
        data: {
            // 性别变量，初始值为所有，男孩女孩都能看到。
            sex: 'all',
            users: [
                {name: '老大', sex: 'boy'},
                {name: '老二', sex: 'boy'},
                {name: '老三', sex: 'girl'},
                {name: '老四', sex: 'girl'}
            ]
        },
    });
</script>

```

`unshift`, `shift`, `pop`也是变异方法。

`splice`也是变异方法: `splice(key, length)`

`reverse()`方法反转。

`sort(func)` 排序。

`filter`过滤。

```
<body>
    <main id="app">
        <ul>
            <li v-for="user in users">
                {{ user.name }}
            </li>
        </ul>

        <input type="text" v-model="name" placeholder="请输入新用户的名称">
        <button v-on:click="add('start')">在前面添加</button>
        <button v-on:click="add('end')">在后面添加</button>
        <button v-on:click="del('start')">删除第一条</button>
        <button v-on:click="del('end')">删除最后一和</button>
        <br>
        <input type="text" v-on:keyup.enter="search" v-model="search_content" placeholder="回车搜索">
    </main>
</body>
<script>
    let app = new Vue({
        el: '#app',
        methods: {
            search () {
                this.users = this.users.filter(user => {
                    var reg = new RegExp(this.search_content, 'i');
                    return reg.test(user.name);
                });
            }
        },
        data: {
            name: '',
            search_content: '',
            users: [
                {name: '张三'},
                {name: '李四'},
                {name: '王老五'}
            ]
        },
    });
</script>
```

---

事件

`v-on:事件名称(参数)`来绑定事件。
也可以简写成：`@事件名称(参数)`
如果没有参数还可以简写成：`@事件名称`


事件修饰符

`prevent`: 阻止默认事件。

如：表单提交时，如果用JS去操作提交动作，一般会在事件触发的函数里写：`event.preventDefault()`来阻止表单的默认提交动作。

为了方便VUE里可以直接使用：`@submit.prevent`来阻止，非常方便。


```
<body>
    <main id="app">
        <form action="" @submit.prevent="submit">
            <button>submit</button>
        </form>
    </main>
</body>
<script>
    let app = new Vue({
        el: '#app',
        methods: {
            submit () {
                alert('触发到表单的提交事件');
            }
        }
    });
</script>
```

`stop`修饰符可以阻止事件冒泡

`self`用来只修饰事件触发的对象是绑定事件元素的本身。
例如有div下有一个a标签，都绑定点击事件，正常不阻止冒泡事件的情况下，点击了a标签，点击事件会继续向外冒泡到div的点击事件上。
这时，如果不想点击链接却触发到div的点击事件上，可以给div的事件绑定添加修饰符`self`，这样就不会触发通过冒泡或者捕获传递来的点击事件了。
也可以通过给链接标签添加`stop`修饰符来阻止事件的冒泡。

`capture`可以用来改变事件的传递方式为捕获。

`once`可以指定绑定的事件和修饰符只触发一次。
`<a href="https://www.google.com/" @click.prevent.stop.once="onceClick">`
当这个链接在第一次点击后，会触发VUE的`onceClick`方法，并阻止冒泡，阻止页面跳转，但不刷新网页的情况下，第二次和以后的点击就会正常跳转到Google网站了。


事件修饰符可以链式调用，如：`@click.prevent.stop.self.once`

---

键盘修饰符

支持的键盘修饰符：`enter, space, delete, ctrl, alt, shift, tab, esc, left, right, up, down`, 其中，`ctrl, alt, shift`需要结合其它键才能使用。

---

鼠标

`click`左键，`middle`中键，`contextmenu`右键

也可是使用`prevent`阻止默认事件。

---

表单

表单修饰符：`lazy, number, trim`

---

**组件**

组件可以达到代码复用的效果。

VUE里组件分为：全局组件和局部组件。

声明全局组件要在实例化VUE对象之前：

局部组件在VUE对象内部声明。

使用`template`声明组件的HTML模板

```html
<body>
    <main id="app">
        <vue-com></vue-com>
        <v-com-local></v-com-local>
    </main>
</body>
<script>
    // 声明全局组件：要在实例化VUE对象前。
    // 第一个参数为组件的名称
    // 名称可以用‘-’分割
    // 也可以写成驼峰形式，如：`vueCom`, 等同于`vue-com`
    Vue.component('vue-com', {
        template: '<h1>VUE全局组件</h1>'
    });
    
    // 在外部声明的局部组件，名称需要为大驼峰
    var vComLocal = {
        template: '<h3>VUE局部组件在外部声明</h3>'
    };
    
    let app = new Vue({
        el: '#app',
        
        // 在内部声明组件
        components: {
            // 外部定义的组件内容
            vComLocal
            
            // 内部定义的组件内容
            // 名称如果使用-作为分割符，需要使用引号括起。
            // 也可以使用`vComLocal`声明，不需要添加引号
            // 'v-com-local': {
            //     template: '<h2>VUE局部组件</h2>'
            // }
        }
    });
</script>
```

组件的模板必须且只能有一个父级元素。

模板里如果有多个HTML元素，用引号包裹会比较麻烦，可以使用空白模板来声明。


```html
<body>
    <main id="app">
        // 向组件传递参数，以声明属性的方式。
        // 直接写属性名称，后接的数据都会被VUE解析成字符串，如：name="string"，这是的"string"就是一个普通的字符串。
        // 如果想要传递其它类型的数据，比如对象，数组，布尔类型的数据，需要在属性名称前加":"，如：`:lists="lists"`
        // 这时的属性值"lists"就不会被解析成普通的字符串，而是VUE里的变量。
        <vue-com name="string" :lists="lists"></vue-com>
    </main>
</body>

<script type="text/x-template" id="tpl">
    <div>
        <p>{{ name }}</p>
        <ul>
            <li v-for="v in lists">
                {{ v.id }}
            </li>
        </ul>
    </div>
</script>

<script>
    var vueCom = {
        template: '#tpl',
        data () {
            return {};
        },
        // 用props属性接收参数：
        // 需要用到的参数都要在这里声明。
        // 用法和data一样，直接调用即可。
        props: ['name', 'lists']
    };


    let app = new Vue({
        el: '#app',
        components: {vueCom},
        data: {
            lists: [
                {id: 1},
                {id: 2},
                {id: 3},
            ],
        }
    });
</script>

```

**props的数据验证**


```js
props: {
    // 验证lists属性
    lists: {
        // 支持的类型有：Boolean, Array, String, Function, Object, Number。
        // 如何可以为多个类型，可以写成数组的方式：type: [Array, Number]
        type: Array,  // 类型必须为Array
        
        // 如果不传递这个参数，采用的默认值。
        default: [],
        
        // 是否必填　
        required: true
    },
    
    // 写成validator方法的形式验证数据。
    sex: {
        validator (arg) {
            return arg===1 || arg===0;
        }
    }
    
}
```

**组件间的通信：子组件调用父组件的事件**


```html
<body>
<!--子组件触发父组件的事件，需要触发父组件留给子组件的触发器。-->
<!--所谓触发器，类似于一个接口的东西，是子组件和父组件事件间的唯一关联。-->
<!--当子组件需要通知父组件调用某事件时，通过this.$emit触发父组件给子组件留下的触发器，这个触发器会去调用父组件里的方法。-->

    <main id="app">
        // @call为父组件留给子组件的触发器，当子组件触发call事件时，会调用父组件的parentEvent
        <vue-com @call="parentEvent"></vue-com>
    </main>
</body>

<script type="text/x-template" id="tpl">
    // 子组件绑定单击事件到childEvent
    <div :style="style" @click="childEvent"></div>
</script>

<script>
    var vueCom = {
        template: '#tpl',
        data () {
            return {
                style: {
                    border: '1px solid red',
                    width: '100%',
                    height: '200px'
                }
            }
        },
        methods: {
            // 定义子组件里的事件：childEvent
            childEvent () {
                // 触发父组件留给子组件的触发器：call
                this.$emit('call');
            }
        }
    };


    let app = new Vue({
        el: '#app',
        components: {vueCom},
        methods: {
            parentEvent () {
                alert('调用了父组件里的parent事件');
            }
        }
    });
</script>

```

**组件间的通信：数据同步**


```html
<body>
    <main id="app">
        <!-- 这里不用.sync也可以同步数据！！！？？？ -->
        <vue-com :lists.sync="items"></vue-com>
        <p>总计：{{ total }}个</p>
    </main>
</body>

<script type="text/x-template" id="tpl">
    <div>
        <ul>
            <li v-for="v in lists">{{ v.name }}</li>
        </ul>
        <p>
            <input type="text" v-model="itemName" @keyup.enter="add">
        </p>
    </div>
</script>

<script>
    var vueCom = {
        template: '#tpl',
        props: ['lists'],
        data () {
            return {
                itemName: ''
            }
        },
        methods: {
            add () {
                if (this.itemName) {
                    var newItem = {
                        name: this.itemName
                    }
                    this.lists.push(newItem);
                    this.itemName = '';
                }
            }
        }
    };


    let app = new Vue({
        el: '#app',
        components: {vueCom},
        data: {
            items: [
                {name: 'item1'},
                {name: 'item2'},
                {name: 'item3'}
            ]
        },
        computed: {
            total () {
                return this.items.length;
            }
        }
    });
</script>
```

**组件间的通信：数据分发**


```
<body>
    <!-- 感觉这个内容分发其实就类似于后台模板里的点位符，比如Laravel里的yeild -->
    <main id="app">
        <vue-com>
            <h1 slot="desc">将这个标题分发到组件里</h1>
        </vue-com>
    </main>
</body>

<script type="text/x-template" id="tpl">
    <div>
        <!-- 调用组件时分发的desc将会替换下面这个标签 -->
        <slot name="desc"></slot>
    </div>
</script>

<script>
    var vueCom = {
        template: '#tpl'
    };

    let app = new Vue({
        el: '#app',
        components: {vueCom}
    });
</script>
```

**父组件里定义子组件的元素结构：slot-scope**

> 下面这段代码，我们在根组件里定义了三个用户，每个用户分别有两个属性：名字和年龄。
> 然后定义了一个子组件，遍历用户列表，显示用户的信息。
> 这时候，如果有需求：我想在调用组件的时候给名字和年龄加点颜色（或者其它需求）。
> 我们就会发现，目前的代码是不能很好的实现的。
> 因为我第一次调用组件的时候，我可能想给名字加红色，年龄加绿色，其中一种实现方式就是给名字和年龄字段外套上一个`span`标签，然后给标签绑定想要的颜色。这样的话，就直接在组件模板里写死就行了。
> 但当我第二次调用的时候，我决定不用这俩颜色了，我们就会发现，用这种方式是行不通的。
> 我们需要在调用组件的时候直接定义子组件里的代码。
> 于是，我们想到用**数据分发**来做这件事，请看第二段代码。

```
// 第一段代码

<body>
<main id="app">
    <vue-com :lists="users"></vue-com>
</main>
</body>

<script type="text/x-template" id="tpl">
    <ul>
        <li v-for="v in lists">{{ v.name }} - {{ v.age }}</li>
    </ul>
</script>

<script>
    var vueCom = {
        template: '#tpl',
        props: ['lists']
    };

    let app = new Vue({
        el: '#app',
        components: {vueCom},
        data: {
            users: [
                {name: '张三', age: 23},
                {name: '李四', age: 24},
                {name: '王五', age: 25},
            ]
        }
    });
</script>
```

> 这段代码里，我们用`slot`替换了写死在组件里的`li`标签，并如期地给名字和年龄分别加上了颜色标签。
> 但是我们发现，在输出变量名字和年龄的时候无法正确地表达这两个字段。
> 因为我们在组件模板的外面，不能直接调用模板里的`v`。
> 这时候，就该到`slot-scope`出场了。
> 请看第三段代码。

```
// 第二段代码

<body>
<main id="app">
    <vue-com :lists="users">
        <template slot="li">
            <li>
                <span :style="{color: 'green'}">{{ 名字 }}</span>
                -
                <span :style="{color: 'red'}">{{ 年龄 }}</span>
            </li>
        </template>
    </vue-com>
</main>
</body>

<script type="text/x-template" id="tpl">
    <ul>
        <slot name="li" v-for="v in lists"></slot>
        <!--<li v-for="v in lists">{{ v.name }} - {{ v.age }}</li>-->
    </ul>
</script>
```


```
// 第三段代码

<main id="app">
    <vue-com :lists="users">
        // 这里用scope属性接收所有模板在渲染时定义的属性并赋值给变量`v`
        // 变量v里就包含了`data`。
        // 我们在模板的代码里已经把每一次遍历得到的一个用户信息对象`{name: 'xxx', age: xx}`赋值给了data
        // 所以我们现在可以在调用模板的地方，也就是这里，直接调用`v.data`来取得当前遍历的用户信息了。
        <template slot="li" slot-scope="v">
            <li>
                <span :style="{color: 'green'}">{{ v.data.name }}</span>
                -
                <span :style="{color: 'red'}">{{ v.data.age }}</span>
            </li>
        </template>
    </vue-com>
</main>
</body>

<script type="text/x-template" id="tpl">
    <ul>
        <!--将每一次遍历得到的结果赋值给属性`data`以供在调用组件时调用。-->
        <slot name="li" v-for="v in lists" :data="v"></slot>
        <!--<li v-for="v in lists">{{ v.name }} - {{ v.age }}</li>-->
    </ul>
</script>

```

结果如下：

![](https://ws4.sinaimg.cn/large/006tNc79ly1fp9bpn7xpij30c007ajr7.jpg)


**动态设置组件：`:is`**


```
<body>
    <main id="app">
        <!--下面这个DIV将会替换成模板, :is属性绑定的是一个变量名称type-->
        <!-- 这个DIV不能用template标签代替！ -->
        <div :is="type"></div>

        <!--当选择不同的单选按钮时，会将当前选中的值传递给VUE变量type，而type的改变会引发VUE重新将上面的DIV渲染，以此达到动态的效果-->
        <div>
            <input type="radio" v-model="type" value="comInput"> 文本框
            <input type="radio" v-model="type" value="comTextarea"> 文本域
        </div>
    </main>
</body>

<script>
    let comInput = {
        template: '<div><input></div>',
    };

    let comTextarea = {
        template: '<textarea></textarea>'
    };

    let app = new Vue({
        el: '#app',
        components: {comInput, comTextarea},
        data: {
            type: 'comInput'
        }
    });
</script>
```

---

**动画**

> 可以使用animate.css动画库。

```html
<body>
<main id="app">
    <button @click="type=!type">点击切换</button>
    <!--动画用transition标签包裹-->
    <!--这个标签的name属性用来指定动画类的名称，名称的格式如下: -->
    <!--name-enter: 准备显示-->
    <!--name-enter-active: 正在显示-->
    <!--name-enter-to: 显示完毕-->
    <!--name-leave: 准备隐藏-->
    <!--name-leave-active: 正在隐藏-->
    <!--name-leave-to: 隐藏完毕-->
    <transition name="animate">
        <h1 v-show="type">我是一个动画</h1>
    </transition>
</main>
</body>

<script>
    let app = new Vue({
        el: '#app',
        data: {
            type: true
        }
    });
</script>

<!-- 这里书写上面所指定的动画类 -->
<style>
    .animate-enter {
        opacity: 0;
    }

    .animate-leaves {
        opacity: 1;
    }

    .animate-enter-active, .animate-leave-active {
        -webkit-transition: all 2s;
        -moz-transition: all 2s;
        -ms-transition: all 2s;
        -o-transition: all 2s;
        transition: all 2s;
        opacity: .5;
    }

    .animate-enter-to {
        opacity: 1;
    }

    .animate-leave-to {
        opacity: 0;
    }
</style>
```

---

**自定义指令**


```
<body>
<main id="app">
    <h1 v-global>我是一个全局指令</h1>
    <h2 v-local>我是一个非全局指令</h2>
</main>
</body>

<script>
    // 定义全局指令和定义全局组件差不多
    // 第一个参数为指令名称，第二个参数是一个对象，其中可以包含触需要的钩子
    Vue.directive('global', {
        // 三个钩子都自动传入两个参数：
        // 第一个参数是当前元素
        // 第二个参数是包含当前元素一些信息的对象

        // 元素绑定的钩子
        bind (el, bind) {
            console.log(bind);
        },

        // 元素更新的钩子
        update (el, bind) {
            console.log(bind);
        },

        // 元素插入父节点的钩子
        inserted (el, bind) {
            console.log(bind);
        }
    });
    let app = new Vue({
        el: '#app',

        // 局部指令，定义在Vue内部
        directives: {
            // 常用的钩子有bind和update
            // 如果我们只需要这两种钩子时，就这简写成下面这样
            // 但inserted钩子不可，需要单独定义
            local (el, bind) {
                console.log(bind);
            }
        }
    });
</script>
```

---

**CLI**

安装：`npm install -g vue-cli`

注意使用淘宝镜像源或者cnpm或者yarn

安装前会询问几个问题，比如项目名称，作者，使用`npm`还是`yarn`，，是否安装`vue-route`等，`vue-router`选是，其它的暂时选否。

安装完成后，可以初始化一个webpack模板的项目：`vue init webpack cli`

这样经过一段下载时间以后，会在当前目录下生成一个`cli`的目录，这就是我们刚才初始化的项目。

进入项目，执行`npm run dev`，初始化完成后，会收到提示，访问`http://localhost:8080`即可以看到我们的项目了。

好激动，终于看到点像样的东西了。

![](https://ws1.sinaimg.cn/large/006tNc79ly1fpah4o1sd9j31kw0wejsa.jpg)

---

**路由**


```Html
<body>
<script>
    // VUE 路由
    // 按照我的理解，路由的原理就是锚点。
    // 通过点击不同的锚点，路由器会将页面处理，显示我们要拜访的锚点（组件），将其它的锚点隐藏掉。
</script>
    <main id="app">
        <!--定义锚点-->
        <router-link to="home">Home</router-link>
        <router-link to="admin">Admin</router-link>
        <!--定义显示组件的容器: 路由处理后的组件会显示在这个容器里-->
        <router-view></router-view>
    </main>
</body>
<script src="vue.js"></script>
<!--引入路由文件-->
<script src="vue-router.js"></script>
<script>
    // 定义两个组件：home, admin
    const home = {
        template: '<h1>Home</h1>'
    };
    const admin = {
        template: '<h1>Admin</h1>',
    };

    // 定义路由
    // 每一个路由都是一个对象
    // 每个对象有两个属性：path, component
    // path: 访问的地址（锚点）
    // component: 当访问这个地址时，要显示的组件
    let routes = [
        {path: '/home', component: home},
        {path: '/admin', component: admin}
    ];

    // 实例化路由器
    let router = new VueRouter({
        // routes: routes
        routes
    });

    // 实例化 VUE
    new Vue({
        el: '#app',
        // router: router
        router
    });

</script>
```

**路由参数**

``` html
<body>

<div id="app">
    <router-link to="/home">Home</router-link>
    <router-view></router-view>
</div>

<!--组件模板-->
<script type="text/x-template" id="home">
    <div>
        <!--在模板里输入路由参数：使用 $route.params 来获取所有参数，`id`为在路由规则里定义的具体的参数名称-->
        {{ $route.params.id }}
        <br>
        <button @click="showAll">获取全部参数对象</button>
        <button @click="showID">获取参数：ID的值</button>
    </div>
</script>

<script>
    // 定义组件
    const home = {
        template: '#home',
        // 在组件里获取参数：this.$route.params.id
        methods: {
            showAll () {
                console.log(this.$route.params);  // {id: 99}
            },
            showID () {
                console.log(this.$route.params.id);  // 99
            }
        }
    };

    // 定义路由
    let routes = [
        // 路由规则里定义参数：`:id`代表参数名称。
        // 如果想传参99，可以写成：`/home/99`
        {path: '/home/:id', component: home},
    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化 VUE
    new Vue({
        el: '#app',
        router
    });

</script>
</body>
```

**参数默认值**

```html
<body>

<div id="app">
    <router-link to="/home">Home</router-link>
    <router-view></router-view>
</div>

<!--组件模板-->
<script type="text/x-template" id="home">
    <div>
        {{ id }}
    </div>
</script>

<script>
    // 定义组件
    const home = {
        template: '#home',
        // 可以将参数的默认值定义在data里
        // 但如果只在这里定义，那么我们获取到的参数值，永远都是这里定义的数据。
        data () {
            return {
                id: 0
            }
        },
        // 所以，当我们真的传参进来的时候，如果想获取正确的参数，需要通过data里的变量来存储一下
        // Vue给提供挂载点用于在页面元素渲染前做一些准备。
        // 我们在这个方法里，先判断有没有传参进来。
        // 如果有传参，那么我们就将这个参数的值赋给data里的变量
        // 如果没有，那么参数就默认是data里的变量
        mounted () {
            if (this.$route.params.id) {
                this.id = this.$route.params.id;
            }
            // 这里也可以定义成其它的，比如变量里的ID需要预告定义我们给了0为初始值。
            // 如果我实际上需要定义为1的话，在这里也可以这样写：
            this.id = this.$route.params.id? this.$route.params.id: 1;
        }
    };

    // 定义路由
    let routes = [
        // 参数名后接`?`代码这个参数有没有都可以。
        // 如果访问路由的时候，没传这个参数，这个参数会定义为`undefinde`。
        // 我们也可以通过手段给其赋予默认值。
        {path: '/home/:id?', component: home},
    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化 VUE
    new Vue({
        el: '#app',
        router
    });

</script>
</body>
```

**路由：别名的使用和文章列表到文章详情的处理**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vue-router.js"></script>
    <title>router别名与文章列表与文章详情页面的跳转</title>
</head>

<body>
    <div id="app">
        <router-view></router-view>
    </div>
</body>

<!--首页模板，用来显示文章列表-->
<script type="text/x-template" id="home">
    <div>
        <ul>
            <!-- 遍历文章数据 -->
            <li v-for="v in data">
                <!--设置一个链接，连接到文章详情页的路由，并传递当前文章的ID-->
                <router-link :to="{name: 'content', params: {id: v.id}}">{{ v.title }}</router-link>
            </li>
        </ul>
    </div>
</script>

<!--文章详情页模板：输入文章的相关信息-->
<script type="text/x-template" id="content">
    <div>
        <ul>
            <li>编号：{{ article.id }}</li>
            <li>标题：{{ article.title }}</li>
            <li>内容：{{ article.content }}</li>
        </ul>

        <!--返回首页链接-->
        <router-link to="/">返回首页</router-link>
    </div>
</script>

<script>
    // 定义一组文章数据，假设有这么一组文章数据。
    let data = [
        {id: 1, title: '文章1', content: '这里是文章1的内容'},
        {id: 2, title: '文章2', content: '这里是文章2的内容'},
    ];

    // 首页组件：把所有文章的数据存储在属性里，再由模板里遍历输入每一篇文章的标题链接
    const home = {
        template: '#home',
        data () {
            return {
                data
            }
        }
    };

    // 文章详情页组件：输入文章的相关信息。
    // 当在首页点击了带有文章ID参数的标题链接以后，路由器将请求下发到这个组件。
    // 组件里定义了一个article属性用来存放即将要获取的文章的数据。
    // 获取文章数据要在挂载点完成（视图渲染完成前）
    const content = {
        template: '#content',
        data () {
            return {
                // 用来存放文章相关信息的属性
                article: {}
            }
        },

        // 挂载点：
        // 获取路由传递来的参数ID。
        // 通过ID，获取这篇文章的数据。
        // 将文章数据转存到当前组件的属性article里，组件里就可以渲染了。
        mounted () {
            // 获取路由传递来的参数ID
            let id = this.$route.params.id;

            // 将对应id的文章数据转存到属性里
            data.forEach((article)=>{
                if (article.id == id) {
                    this.article = article;
                }
            });
        }
    };

    // 定义路由规则
    let routes = [
        {path: '/', component: home},
        {path: '/content/:id', component: content, name: 'content'}
    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化VUE
    let app = new Vue({
        el: '#app',
        router
    });
</script>

</html>
```

**路由嵌套**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vue-router.js"></script>
    <title>路由嵌套</title>
    <script>
        // 假设我们有这样一个需求：
        // 当我们从首页的文章列表里点击任意一个链接进入到相应的文章详情页面后
        // 我们依然想看到文章列表，这时候应该怎么处理呢？
        // 如果按照MVC的设计思想，那么此时，我们应该把文章列表页单独放到一个视图中
        // 然后，在基本布局结构页面中加载
        // 而，文章详情页继承基本布局页面以后，就自然会有了文章列表
        // 依我看，在VUE中也大致是这个思路，不过做法有些不同
        // 因为我们的首页即文章列表页，其实就是一个组件
        // 当路由将请求转发到这个组件后，组件将模盛放在根路由视图中（根节点的<router-view></router-view>）
        // 所以，按照这种思路走下去，我们也应该把文章详情页的组件渲染到文章列表组件里，这样就能达到我们想要的效果了。
        // 所以，我们需要在首页组件的模板里，定义一个路由视图，用来盛放文章详情页的渲染结果。
        // 这种做法，也称之为路由嵌套
    </script>
</head>

<body>
<div id="app">
    <!--根节点的路由视图，用来盛放首页组件的渲染效果-->
    <router-view></router-view>
</div>
</body>

<!--首页模板，用来显示文章列表-->
<script type="text/x-template" id="home">
    <div>
        <ul>
            <!--遍历文章数据-->
            <li v-for="v in data">
                <!--设置一个链接，连接到文章详情页的路由，并传递当前文章的ID-->
                <router-link :to="{name: 'content', params: {id: v.id}}">{{ v.title }}</router-link>
            </li>
        </ul>

        <!--在首页模板里定义一个路由视图，用来盛放文章详情页-->
        <router-view></router-view>
    </div>
</script>

<!--文章详情页模板：输入文章的相关信息-->
<script type="text/x-template" id="content">
    <div>
        <ul>
            <li>编号：{{ article.id }}</li>
            <li>标题：{{ article.title }}</li>
            <li>内容：{{ article.content }}</li>
        </ul>
    </div>
</script>

<script>
    // 定义一组文章数据，假设有这么一组文章数据。
    let data = [
        {id: 1, title: '文章1', content: '这里是文章1的内容'},
        {id: 2, title: '文章2', content: '这里是文章2的内容'},
    ];

    // 首页组件：把所有文章的数据存储在属性里，再由模板里遍历输入每一篇文章的标题链接
    const home = {
        template: '#home',
        data() {
            return {
                data
            }
        }
    };

    // 文章详情页组件：输入文章的相关信息。
    const content = {
        template: '#content',
        data() {
            return {
                // 用来存放文章相关信息的属性
                article: {}
            }
        },

        // 挂载点：
        mounted() {
            // 获取路由传递来的参数ID
            let id = this.$route.params.id;
            // 将对应id的文章数据转存到属性里
            data.forEach((article) => {
                if (article.id == id) {
                    this.article = article;
                }
            });
        }
    };

    // 定义路由规则
    let routes = [
        {
            // 首页的路由
            // 因为我们需要将文章详情页渲染到这个组件里
            // 这时候，首页和详情页的关系感觉像是所属关系
            // 即：详情页属于首页
            // 所以我们需要用children这个属性，将文章详情页的路由定义为首页的子路由。
            path: '/', component: home, children: [
                {path: '/content/:id', component: content, name: 'content'}
            ]
        },

    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化VUE
    let app = new Vue({
        el: '#app',
        router
    });
</script>
<script>
    // 存在的问题：
    // 如果你测试了代码，相信你一定会发现一个问题：
    // 现在我们的页面跳转不灵了。
    // 且只有在每次刷新的时候才会得到正确的文章详情信息，这可不是我们所期待的。
    // 至于为什么会这样以及如何解决这个问题，见下一篇代码
</script>
</html>
```

**路由监听**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vue-router.js"></script>
    <title>路由监听</title>
    <script>
        // 上一篇代码中，我们假定了一个需求，并学习了路由嵌套来应对这个需求场景。
        // 虽然效果达到了（一定程度上），但却仍然有一个问题：页面不能正确地渲染数据。
        // 现在我们来分析一下为什么会产生这个问题。
        // 首先，我们知道，VUE的组件是懒加载的
        // 也就是说，我们的详情页是放在首页里渲染的
        // 而当我们无论点击哪一篇文章的标题链接时，过是在首页里渲染的
        // 所以这对于VUE来说，并没有发生变化（其实路由是变了的）。
        // 所以视图不会重新渲染
        // 而要解决这个问题也很简单：我们去监听路由的参数
        // 当我们所处在文章1的详情页时，路由应该是：`/content/1`
        // 而当我们点击了文章2的标题链接后，路由应该达到了：`/content/2`
        // 所以我们监听路由变化，然后去重新渲染视图，就可以解决这个问题了。
    </script>
</head>

<body>
<div id="app">
    <!--根节点的路由视图，用来盛放首页组件的渲染效果-->
    <router-view></router-view>
</div>
</body>

<!--首页模板，用来显示文章列表-->
<script type="text/x-template" id="home">
    <div>
        <ul>
            <!--遍历文章数据-->
            <li v-for="v in data">
                <!--设置一个链接，连接到文章详情页的路由，并传递当前文章的ID-->
                <router-link :to="{name: 'content', params: {id: v.id}}">{{ v.title }}</router-link>
            </li>
        </ul>

        <!--在首页模板里定义一个路由视图，用来盛放文章详情页-->
        <router-view></router-view>
    </div>
</script>

<!--文章详情页模板：输入文章的相关信息-->
<script type="text/x-template" id="content">
    <div>
        <ul>
            <li>编号：{{ article.id }}</li>
            <li>标题：{{ article.title }}</li>
            <li>内容：{{ article.content }}</li>
        </ul>
    </div>
</script>

<script>
    // 定义一组文章数据，假设有这么一组文章数据。
    let data = [
        {id: 1, title: '文章1', content: '这里是文章1的内容'},
        {id: 2, title: '文章2', content: '这里是文章2的内容'},
    ];

    // 首页组件：把所有文章的数据存储在属性里，再由模板里遍历输入每一篇文章的标题链接
    const home = {
        template: '#home',
        data() {
            return {
                data
            }
        }
    };

    // 文章详情页组件：输入文章的相关信息。
    const content = {
        template: '#content',
        data() {
            return {
                // 用来存放文章相关信息的属性
                article: {}
            }
        },

        // 挂载点：
        mounted() {
            // 绑定数据的方法
            this.load();
        },

        // 监听路由
        watch: {
            // 注意写法
            // 每次访问路由的时候，都会携带两个参数：，到哪个路由去，从哪个路由来。
            // 只有在路由发生变化的时候，才会触发这个方法
            // 所以我们在这个方法里重新处理一下文章详情组件里数据VUE就会重新渲染了（因为属性是绑定的）。
            '$route' (to, from) {
                // 因为代码是一样的，所以我们提取到methods里
                this.load();
            }
        },

        // 方法
        methods: {
            // 处理文章数据的绑定
            load () {
                // 获取路由传递来的参数ID
                let id = this.$route.params.id;
                // 将对应id的文章数据转存到属性里
                data.forEach((article) => {
                    if (article.id == id) {
                        this.article = article;
                    }
                });
            }
        }
    };

    // 定义路由规则
    let routes = [
        {
            // 首页的路由
            // 因为我们需要将文章详情页渲染到这个组件里
            // 这时候，首页和详情页的关系感觉像是所属关系
            // 即：详情页属于首页
            // 所以我们需要用children这个属性，将文章详情页的路由定义为首页的子路由。
            path: '/', component: home, children: [
                {path: '/content/:id', component: content, name: 'content'}
            ]
        },

    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化VUE
    let app = new Vue({
        el: '#app',
        router
    });
</script>
</html>
```

**路由：程序控制跳转**

* 使用`this.$router.[X]`来进行跳转。注意这里是`$router`

`[X]`包括：`push()`, `replace()`, `go()`

比如说我们想跳转到文章2详情页，可以这样写：`this.$router.push('/content/2')`

当然也可以把路由和参数摘出来：
```js
let url = {name: 'content', params:{id: 2}};
this.$router.push(url);
```

`push()`的好处是会写入历史记录。

**路由：命名视图**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vue-router.js"></script>
    <title>命名视图</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        .header {
            border: 1px solid red;
            width: 100%;
            height: 200px;
        }
        .content {
            border: 1px solid greenyellow;
            height: 500px;
            width: 60%;
            float: right;
        }
        .slide {
            border: 1px solid cyan;
            width: 30%;
            height: 500px;
            float: left;
        }
        .home1 {
            border-radius: 20px;
        }
    </style>
</head>

<body>
<div id="app">
    <router-view class="header"></router-view>
    <router-view class="slide" name="slide"></router-view>
    <router-view class="content" name="content"></router-view>
</div>
</body>

<script type="text/x-template" id="home">
    <div class="home1">
        home1
    </div>
</script>

<script type="text/x-template" id="slide">
    <div>
        slide1
    </div>
</script>

<script type="text/x-template" id="content">
    <div>
        content1
    </div>
</script>

<script>
    const home = {
        template: '#home',
    };

    const slide = {
        template: '#slide',
    }

    const content = {
        template: '#content',
    };

    // 定义路由规则
    let routes = [
        {
            // 注意组件写法为复数
            // default为没有命名的那个视图
            path: '/', components: {
                default: home,
                slide: slide,
                content: content
            }
        },

    ];

    // 实例化路由器
    let router = new VueRouter({routes});

    // 实例化VUE
    let app = new Vue({
        el: '#app',
        router
    });
</script>
</html>
```

**路由：重定向**

路由规则里的`redirect`可用于重定向

```js
redirect: {
    name: 'content',
    params: {
        id: 1,
    }
}
```

**路由：别名**

> 我们在路由规则里使用`alias`属性给路由规则定义别名。
> 这样，在访问about路由时候，实际上解析到的是`/content/3`，但地址栏显示还是`about`

```js
{path: '/content/3', component: content, alias: ['/about']}
```

**路由：美化地址，隐藏锚点**

需要web服务器做相应配置。

在路由规则里定义模式

```js
let router = new VueRouter({
    // 开启history模式
    mode: 'history',
    routes
});
```

**路由：404页面**

```js
let routes = [
    {path: '/', component: home},
    // 这条规则会匹配我们路由规则里没有定义的路由。
    {path: '*', component: NotFound}
]
```

**路由：设置过渡动画**

使用`transition`标签可以设置动画

```html
<transition enter-active-class="xxx-class">
    <!-- 这里可以包含组件的模板，也可以是路由视图 -->
</transition>
```

---

### Vuex

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vuex.js"></script>
    <link href="https://cdn.bootcss.com/bootstrap/4.0.0/css/bootstrap.min.css" rel="stylesheet">
    <title>Vuex.state</title>
</head>

<body>
    <div id="app" class="container">
        <cart></cart>
    </div>
</body>

<!-- 购物车组件模板 -->
<script type="text/x-template" id="cart">
    <div>
        <table class="table table-sm">
            <tr class="thead-dark">
                <th>ID</th>
                <th>商品名称</th>
                <th>价格</th>
            </tr>
            <tr v-for="v in goods">
                <td>{{ v.id }}</td>
                <td>{{ v.title }}</td>
                <td>{{ v.price }}</td>
            </tr>
        </table>
        <div class="alert alert-success">总价：{{ totalPrice }} 元</div>
    </div>
</script>

<script>
    // 定义一个购物车组件
    let cart = {
        template: '#cart',
        // 因为购物车里的商品数量及总价是动态变化的，所以我们用计算属性更方便一些
        // 我们在计算属性里，将Vuex仓库里需要的数据拿到这里，分发到组件的数据中
        computed: {
            totalPrice () {
                return this.$store.state.totalPrice;
            },
            goods () {
                return this.$store.state.goods;
            }
        }
    }

    // 实例化Vuex
    let store = new Vuex.Store({
        // 这个属性相当于一个仓库，相当于Vue根实例的data属性
        // 可以在其中定义一些属性，这些属性是全局的，在哪里都可以访问到
        state: {
            // 模拟购物车总价
            totalPrice: 99,
            // 模拟商品数据
            goods: [
                {id: 1, title: 'iPhone 7 Plus', price: 399},
                {id: 2, title: 'iPhone 8 Plus', price: 599},
                {id: 3, title: 'iPhone X Plus', price: 799},
            ],
        }
    });

    // 实例化Vue
    let app = new Vue({
        el: '#app',
        store,
        components: {
            cart
        }
    });
</script>

</html>
```

![vuex](https://ws1.sinaimg.cn/large/006tKfTcly1fphcsmqfn1j31kw0bzq34.jpg)

**Vuex: getters**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vuex.js"></script>
    <link href="https://cdn.bootcss.com/bootstrap/4.0.0/css/bootstrap.min.css" rel="stylesheet">
    <title>Vuex.state</title>
</head>

<body>
    <!-- vuex相当于模型的概念 -->
    <!-- state属性相当于Vue根实例里的data -->
    <!-- getter属性相当于Vue根实例里的computed -->

    <!-- 为了更近一步地模拟购物车效果，我们需要给每种商品添加一个数量的属性 -->
    <!-- 然后计算根据：总价 = 单价 * 数量 计算出总价格。 -->
    <div id="app" class="container">
        <cart></cart>
    </div>
</body>

<!-- 购物车组件模板 -->
<script type="text/x-template" id="cart">
    <div>
        <table class="table table-sm">
            <tr class="thead-dark">
                <th>ID</th>
                <th>商品名称</th>
                <th>价格</th>
                <th>数量</th>
                <th>小计</th>
            </tr>
            <tr v-for="v in goods">
                <td>{{ v.id }}</td>
                <td>{{ v.title }}</td>
                <td>{{ v.price }}</td>
                <td>{{ v.quantity }}</td>
                <td>{{ v.total }}</td>
            </tr>
        </table>
        <div class="alert alert-success">总价：{{ totalPrice }} 元</div>
    </div>
</script>

<script>
    // 定义一个购物车组件
    let cart = {
        template: '#cart',

        computed: {
            totalPrice () {
                return this.$store.getters.totalPrice;
            },
            goods () {
                return this.$store.getters.goods;
            }
        }
    }

    // 实例化Vuex
    let store = new Vuex.Store({
        // 相当于Vue根实例里的data，存放一些死的数据。
        state: {
            // 模拟商品数据
            goods: [
                {id: 1, title: 'iPhone 7 Plus', price: 399, quantity: 1},
                {id: 2, title: 'iPhone 8 Plus', price: 599, quantity: 1},
                {id: 3, title: 'iPhone X Plus', price: 799, quantity: 1},
            ],
        },

        // 相当于Vue根实例里的computed，存放一些计算属性
        getters: {
            // 总价格：参数state为$store.state
            totalPrice (state) {
                let totalPrice = 0;
                // 遍历购物车里的数据，根据商品数量和商品价格计算总价
                state.goods.forEach(item => {
                    totalPrice += item.price * item.quantity;
                });
                return totalPrice;
            },

            // 为了得到每种商品的小计，我们需要给goods数据里的每种商品添加一个属性
            // 我们就需要重新搞出一个带有小计属性的goods数据
            goods (state) {
                // 把state里的死数据拿过来
                let goods = state.goods;
                // 遍历goods，为第一种商品添加小计属性
                goods.forEach(item => {
                    item.total = item.price * item.quantity;
                });

                return goods;
            }
        }

    });

    // 实例化Vue
    let app = new Vue({
        el: '#app',
        store,
        components: {
            cart
        }
    });
</script>

</html>
```

![vuex.getters](https://ws3.sinaimg.cn/large/006tKfTcly1fphe4s3oj1j31kw0ay3yn.jpg)

**vuex: mutations**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="vue.js"></script>
    <script src="vuex.js"></script>
    <link href="https://cdn.bootcss.com/bootstrap/4.0.0/css/bootstrap.min.css" rel="stylesheet">
    <title>Vuex.state</title>
</head>

<body>
    <!-- vuex相当于模型的概念 -->
    <!-- state属性相当于Vue根实例里的data -->
    <!-- getters属性相当于Vue根实例里的computed -->
    <!-- mutations属性相当于methods，里面声明一些事件方法 -->
    <div id="app" class="container">
        <cart></cart>
    </div>
</body>

<!-- 购物车组件模板 -->
<script type="text/x-template" id="cart">
    <div>
        <table class="table table-sm">
            <tr class="thead-dark">
                <th>ID</th>
                <th>商品名称</th>
                <th>价格</th>
                <th>数量</th>
                <th>小计</th>
                <!-- 添加操作栏 -->
                <th>操作</th>
            </tr>
            <tr v-for="(v, index) in goods">
                <td>{{ v.id }}</td>
                <td>{{ v.title }}</td>
                <td>{{ v.price }}</td>
                <td>
                    <input type="text" v-model.number="v.quantity" class="form-control form-control-sm">
                </td>
                <td>{{ v.total }}</td>
                <td>
                    <!-- 当点击删除按钮时，触发remove事件，将当前商品的索引传递给事进 -->
                    <!-- 事件里根据传递进来的索引，删除模拟数据中对应索引的数据对象达到删除的效果 -->
                    <button class="btn btn-sm btn-danger" @click="remove(index)">删除</button>
                </td>
            </tr>
        </table>
        <div class="alert alert-success">总价：{{ totalPrice }} 元</div>
    </div>
</script>

<script>
    // 定义一个购物车组件
    let cart = {
        template: '#cart',

        computed: {
            totalPrice() {
                return this.$store.getters.totalPrice;
            },
            goods() {
                return this.$store.getters.goods;
            }
        },

        methods: {
            remove(index) {
                // this.goods.splice(index, 1);
                // 主动触发mutations里的remove方法，并传递参数index
                this.$store.commit('remove', index);
            }
        }
    }

    // 实例化Vuex
    let store = new Vuex.Store({
        // 相当于Vue根实例里的data，存放一些死的数据。
        state: {
            // 模拟商品数据
            goods: [{
                    id: 1,
                    title: 'iPhone 7 Plus',
                    price: 399,
                    quantity: 1
                },
                {
                    id: 2,
                    title: 'iPhone 8 Plus',
                    price: 599,
                    quantity: 1
                },
                {
                    id: 3,
                    title: 'iPhone X Plus',
                    price: 799,
                    quantity: 1
                },
            ],
        },

        // 相当于Vue根实例里的computed，存放一些计算属性
        getters: {
            // 总价格：参数state为$store.state
            totalPrice(state) {
                let totalPrice = 0;
                // 遍历购物车里的数据，根据商品数量和商品价格计算总价
                state.goods.forEach(item => {
                    totalPrice += item.price * item.quantity;
                });
                return totalPrice;
            },

            // 为了得到每种商品的小计，我们需要给goods数据里的每种商品添加一个属性
            // 我们就需要重新搞出一个带有小计属性的goods数据
            goods(state) {
                // 把state里的死数据拿过来
                let goods = state.goods;
                // 遍历goods，为第一种商品添加小计属性
                goods.forEach(item => {
                    item.total = item.price * item.quantity;
                });

                return goods;
            },
        },

        // 事件声明的的地方，相当于methods
        mutations: {
            remove(state, index) {
                state.goods.splice(index, 1);
            }
        }

    });

    // 实例化Vue
    let app = new Vue({
        el: '#app',
        store,
        components: {
            cart
        }
    });
</script>

</html>
```