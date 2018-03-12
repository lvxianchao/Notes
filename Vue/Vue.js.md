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

```
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

```
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


```
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


```
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


```
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


```
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


```
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






