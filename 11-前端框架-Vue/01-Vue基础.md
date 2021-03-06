## 一 vue简介

### 1.1 MVVM

MVVM是前端视图层的分层开发思想：将前端视图的结构划分为M（model）、V（view）、VM（view-model）三层，VM是模型层与视图层的调度者。

```
M：数据模型层，这里是接口请求到的数据结果集，封装于data对象中，专门用来保存每个页面里单独的数据；

V：视图层，vue实例所控制的元素区域，每个页面的html结构；

VM：new出来的实例对象，就是MVVM中的vm调度者，分隔了m和v；
    控制数据与视图的关系，每当v层想要获取或保存数据的时候，都要由vm做中间的处理；
    数据发生改变，视图也对应做出响应   
```

vue基本代码和MVVM之间的对应:
```js
// V
<div id="app">
    <p>{{msg}}</p>
</div>


// VM 
let vm =new Vue({
   el:'#app' ,

   // M
   data:{
       msg:'hello world !'
   }
})




```

mvc里面的m是指数据库里面的数据，mvvm里的m是指页里面的数据（data里的数据）


###  1.2 vue

vue是一个渐进式MVVM框架（VM：View-Model视图模型，vue即是该层），只关注视图层（view），用来构建Web应用界面。  

所谓渐进式，即我们需要哪些功能就使用框架的哪些模块即可，vue这样处理使其减少侵入性。
- 声明式渲染：vue的核心库提供了数据渲染功能（vue模板引擎），实现视图与数据解耦。
- 组件系统：对界面进行组件化
- 前端路由：可以用来制作移动端单页面应用
- 状态管理：对共享数据进行管理
```

vue的核心特点：
- 数据绑定：当数据发生改变，自动更新视图。其原理是利用了Object.definedProperty中的setter/getter代理数据，监控对数据的操作。（由于该属性IE8不兼容，所以vue只支持IE9以上版本）
- 组合的视图组件：ui页面可以映射为组件数，这些组件具备可维护、可重用、可调试性。
- 虚拟DOM：在操作大量DOM时，js的运行速度会被严重拖累。时常在更新数据后需要重新渲染页面，这样会造成一个困扰：数据未发生改变的地方也要被重新渲染一遍，资源严重浪费。
利用在内存中生成与真实DOM与之对应的数据结构，这个在内存中的结构称之为虚拟DOM，当数据发生变化时，能智能的计算出重新渲染组件所需要的最小资源，并应用到DOM上。
```

当导入vue之后，在浏览器的内存中就多了一个vue构造函数；

## 二 HelloWorld

###  1.1 HelloWorld示例

```js
<div id="app">
    <div>{{msg}}</div>                                  <!-- 获取数据 -->
    <button v-on:click="change">点击弹出数据</button>     <!-- 绑定事件 -->
</div>

<script src="vue2.5.16.js"></script>
<script>
    new Vue({               //Vue实例
        el: '#app',         //挂载元素
        data:{              //数据源
            msg: 'hello world'
        },
        methods: {
            change(){
                alert(this.msg);
            }
        }
    });
</script>
```

###  1.2 示例介绍

vue实例的创建:每一个应用都是通过Vue这个构造函数来创建根实例来启动的：
```js
let app = new Vue({选项对象1,选项对象2....})
```

传入的选项对象，包含：挂载元素、数据、模板、方法等:
```
el： 挂载点，可以是元素，也可以是CSS选择器，支持原生JS写法，
data： 代理数据
methods：定义方法
```

data数据绑定与插值{{}}:每个Vue实例都会代理其对应data对象中的所有属性，这些属性是响应的，而新添加的属性不具备响应功能，改变后不会更新视图。
```js
let app = new Vue({
    el: '#app',
    data: {
        a: 2
    }
});
console.log(app);		    //打印vue实例，查看vue实例的属性
console.log(app.a);         //2
console.log(app._data.a);   //2
```

vue实例本身也代理了data对象里的所有属性，所以可以这样访问：
```js
let myData = {
    a: 1
};
let app = new Vue({
    el: '#app',
    data: myData
});
console.log(app.a);         //1
console.log(app._data.a);   //1
app.a = 2;                  //修改属性，源数据会更改
console.log(myData.a);      //2
myData.a = 3;               //修改源数据，属性也会更改
console.log(app.a);         //3
```

{{}}除了绑定属性外，可以使用js表达式进行简单的运算，以及使用三元运算符，但不支持语句与流程控制。  

注意：如果要真的输出 {{}} 此时可以使用v-pre指令：`<div v-pre>{{hi}}</div>`