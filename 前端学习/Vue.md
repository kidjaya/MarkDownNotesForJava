# 0。21Vue

> 视频参考狂神：https://www.bilibili.com/video/BV18E411a7mC?p=3

## 概要

> SOC原则：（Separation of concerns）关注点分离原则
>
> HTML+CSS+JS：视图
>
> 网络通信框架：axios
>
> 页面跳转：vue-router
>
> 状态管理：vuex
>
> Vue-UI：ICE 
>
> css：less编译css

- M：模型，V：视图，C：控制器 （MVC）

- MVVM分为三个部分：分别是M（Model，模型层 ），V（View，视图层），VM（ViewModel，V与M连接的桥梁，也可以看作为控制器)
   
   1、 M：模型层，主要负责业务数据相关；
   2、 V：视图层，顾名思义，负责视图相关，细分下来就是html+css层；JSP { }
3、 VM：V与M沟通的桥梁，负责监听M或者V的修改，是实现MVVM双向绑定的要点；
   
- 虚拟Dom：利用内存，提高效率

- 计算属性-->Vue特色  Vue：支持MVVM/虚拟Dom

---

> UI框架

- Ant-Design：阿里巴巴触屏，基于React的UI框架
- ElementUI，iview、ice：饿了么出品，基于Vue的UI
- Bootstrap：Twitter推出的一个用于前端开发的开源工具包
- AmazeUI：又叫“妹子UI”，一款HTML跨屏前端框架

---

> JavaScript构建工具

- Babel:JS编译工具，主要用于浏览器不支持的ES新特性，比如用于编译TypeScript
- WebPack:模块打包器，主要作用是打包、压缩、合并以及按顺序加载

---

> 三端统一

- 混合开发（Hybrid App）：主要目的是实现一套代码三端统一（PC、Android、IOS）并能够调用到设备底层硬件（传感器、GPS、摄像头），打包方式主要有以下两种：
  - 云打包：HBuild -> HBuildX,DCloud 出品；API Cloud
  - 本地打包：Cordova（前身是PhoneGap）
- 微信小程序：
  - 方便微信小程序UI开发的框架：WeUI

---

> 后端技术

NodeJs后端诞生，不过**太笨重**，作者放弃更新 出现新 Deno后端 （语法方便前端程序员）

- NodeJS框架以及项目管理工具
  - Express:NodeJS框架
  - Koa：Express简化版
  - NPM：项目综合管理工具，类似Maven
  - YARN：NPM代替方案，类食欲Maven和Gradle的关系

---

> 主流前端框架

VueJS

- iView
  - 基于Vue的UI库，主要服务于PC界面的中后台产品。
  - 使用Vue组件化开发模式，基于npm+webpack+babel开发
  - 支持ES2015，高质量API
  - 特点：主流前端框架，移动端支持较多

- ElementUI
  - Element是饿了么前端开源维护的Vue UI组件，基本涵盖后端所需的所有组件，文档详细，例子丰富。质量高
  - vue-element-admin
  - 前端主流框架，桌面端支持比较多
- ICE
  - 阿里巴巴出品
  - 主要支持React
- VantUI
  - 基于有统一规范实现的Vue组件库，提供了一套UI基础组件和业务组件
  - 可以快速搭建出风格统一的页面，提高开发效率
- AtUI
  - 基于Vue 2.x的前端UI组件库，主要用于快速开发PC网页产品
  - 提供一套npm+webpack+babel前端开发流程
  - css样式独立，实现UI统一风格
- CubeUI
  - 滴滴打车基于Vuejs实现的精致移动端组件库
  - 支持安需引入和后编译，轻量级，扩展性强
  - 有效实现组件二次开发

小程序

- mpVue
  - 美团开发的使用vueJS开发的小程序的前端框架，目前支持，各种平台的小程序。
  - 基于VueJS 修改了运行时框架runtime和代码编译起complier实现，使其可以运行在小程序环境中

- WeUI
  - 微信官方为小程序设计
  - 同微信视觉体验一致的基础样式库，让用户使用感知更加统一
  - 包含button、cell、dailog、toast、article、icon等元素

---

## Vue

> 用于构建用户界面的渐进式框架。核心库只关注视图层

---

### View

- View是视图层，也就是用户界面，前端主要由CSS和HTML来构建，为了更方便的展现ViewModel和Model层的数据，已经产生各种各样的前后端模版语言（FreeMarker、Thymeleaf），各大MVVM框架比如Vue，AngularJS EJS等也有自己用来构建用户界面的模版语言（动态修改页面数据）

### Model

- Model是只数据模型层，指的是后端进行各种业务逻辑操作和处理，主要围绕数据库系统展开的，这里的难点主要在于需要和前端约定统一的**接口规则**。

### ViewModel

- ViewModel是前端开发人员组织生成和维护的视图数据层。在这一层，前端开发者对从后端获取的Model数据进行**转换处理**。做第二次封装，以生成复合View层使用预期的数据模型。
- 需要注意的是ViewModel所封装出来的数据模型包括视图的状态和行为两个部分，而Model层的数据模型只是包含状态
  - 页面的展示，属于视图状态
  - 页面加载、点击、滚动做什么行为都属于视图行为（交互）
- 这样封装可以完整的去描述View层 
- 开发者只需要处理和维护ViewModel层即可，实现**事件驱动编程**
- View展现的不是**Model**层的数据，而是**ViewModel**的数据，由ViewModel负责与Model层交互，**这就完全解耦了View层和Model层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环**

### 什么是MVVM

- MVVM（Model-VIew-ViewModel）是一种软件架构设计模式。是一种简化用户界面的**事件驱动编程**方式。  

- MVVM源自于经典的MVC模式，MVVM的核心是ViewModel层，负责转换Model中的数据对象（Model）来让数据变得更容易管理和使用。

  - ViewModel层向上与视图层进行双向数据绑定
  - 向下与Model层通过接口请求进行数据交互

  ![image-20200710190142144](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200710190142144.png)

  ![image-20200711075338929](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200711075338929.png)

### MVVM模式的实现者

- Model：模型层，在这里表示JavaScript对象，Json数据
- View：视图层，在这里表示DOM（HTML操作的元素）
- ViewModel：连接视图和数据的中间件，Vue.js就是MVVC中间ViewModel的实现者

在MVVM架构中，是不允许 数据 和 视图 直接通信的，只能通过ViewModel来通信，而ViewModel就是定义了一个Observer观察者

- VIewModel能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel能够监听到视图层的变化，并能够通知数据发生改变

至此，我们就明白了，Vue.js就是一个MVVM的实现者，他的核心就是实现了DOM监听与数据绑定

### 为什么使用Vue

- 轻量级
- 适合移动端
- 易上手
- 模块化，虚拟DOM，计算属性
- 开源，社区活跃度高

---

## Vue的基础语法

> v-bind等被称之为指令，指令带有前缀v-，以表示他们是Vue提供的特殊特性。他们会在渲染的Dom上应用特殊的响应式行为。

### v-bind

>  绑定元素属性

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
 <span v-bind:title="message">鼠标悬停</span>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{
            message:"hello,Vue!"
        }
    });
</script>
</body>
</html>
```



### v-if /v-else/v-else-if

> 判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <h1 v-if="statue">{{message}}</h1>
    <h1 v-else-if="type === 'a'">else success</h1>
    <h1 v-else>No</h1>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{
            message:"hello,Vue!",
            statue:false,
            type:'a'
        }
    });
</script>
</body>
</html>
```



### v-for

> 遍历

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <li v-for="(item,index) in items">
        {{item.message}}---{{index}}
    </li>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{
            message:"hello,Vue!",
            items:[
                {message:'123456789'},
                {message:'abcdefghij'},
                {message:'!@#$%%&^&'},
            ]
        }
    });
</script>
</body>
</html>
```



### v-on

> 绑定事件

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <button v-on:click="sayHello">
        click me!
    </button>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{
            message:"hello"
        },
        methods:{//方法都必须定义在Vue的Methods对象中
            sayHello:function(){
                alert(this.message)
            }
        }
    });
</script>
</body>
</html>
```



### 数据双向绑定

#### 概念

- Vuejs是一个MVVM框架，即数据双向绑定，当数据发生变化的时候，视图也就发生了变化。当视图发生变化的时候，数据也会跟着同步变化。（精髓）
- 数据双向绑定，一定是对于UI控件来说的，非UI控件不会涉及到数据双向绑定。单向数据绑定是使用状态管理工具的前提。如果我们使用了**vuex**，那么数据流也是单向的，这是就会和双向绑定有冲突

#### 优点

- 在Vuejs中，如果使用vuex，实际上数据还是单向的，之所以说是数据双向绑定，是基于使用的UI控件来说的，对于我们处理表单，双向数据绑定用起来就很舒服。两者并不排斥，在全局性的数据流使用单向，方便跟踪；局部数据流使用双向，简单易于操作

#### 使用

```html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
<!--输入框-->
    <input type="text" v-model="message">{{message}}
    <br>
    
<!--单选框-->
    性别：
    <input type="radio" name="sex" value="男" v-model="checked"> 男
    <input type="radio" name="sex" value="女" v-model="checked"> 女
    选中的是：{{checked}}
    <br>
    
<!--下啦框-->
    <select name="classes" id="GDP" v-model="selected">
        <option value="" disabled>--请选择--</option>
        <option>语文</option>
        <option>数学</option>
        <option>英语</option>
    </select>
    <span>value:{{selected}}</span>

</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{
            message:123,
            checked:'',
            selected:'语文'
        }
    });
</script>
</body>
</html>
```



### 组件使用实例

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">

<!--组件：传递给组件中的值：props-->
<kidjaya v-for="item in items" v-bind:item="item"></kidjaya>

</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script>
    //定义组件
    Vue.component("kidjaya",{
        props:['item'],
        template:'<h1>{{item}}</h1>'
    });
    let vm = new Vue({
        el:"#app",
        data:{
            items:['Java','PHP','Python']
        }
    });
</script>
</body>
</html>
```



### Axios

> Axios是一个开源的可以用在浏览器端和NodeJs的异步tongx框架，它的主要功能就是实现Ajax移步通信，其功能特点如下：

- 从浏览器端创建XMLHTTPRequests
- 从node.js创建http请求
- 支持Promise API【Js中链式编程】
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换json格式
- 客户端支持防御XSRF（跨站请求伪造）

#### 为何使用

- Vue是一个视图层框架，并且遵守Soc原则（关注度分离原则），所欲Vue并不包含Ajax的通信功能，为了解决通信问题，单独开发了一个名为**vue-resource**的插件，不过在进入2.0时代，推荐使用**axios**框架，少用JQuery，因为Dom操作太频繁，会造成网页的内存消耗，造成卡顿

#### 实例

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <div>{{info.name}}</div>
    <h1>{{info.address.street}}</h1>
    <a v-bind:href="info.url">个人博客</a>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{//属性，vm

        },
        data(){//
            return{//事先定义可以避免报错，虽然报错可以显示
                info:{
                    name:null,
                    address:{
                        street:null,
                        country:null,
                        city:null
                    },
                    url:null
                }
            }
        },
        mounted(){//钩子函数 链式编程 ES6新特性
            axios.get('./data.json').then(response=>(this.info = response.data));
        }
    });
</script>
</body>
</html>
```



### Vue相关核心

### 计算属性

> 计算属性的重点突出在**属性**两个字上，搜线他是个属性，其次这个属性有计算的能力，这里的计算就是这个函数；简单的说，他就是一个能够将计算结果缓存起来的属性（将行装华为了静态属性），仅此而已，可以想象为缓存。
>
> - 计算出来的结果，保存在属性中，内存中运行，虚拟DOM

#### 例子

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <h1>{{message}}</h1>
    <h1>01:{{currentTime01()}}</h1>
    <h1>02:{{currentTime02}}</h1>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    let vm = new Vue({
        el:"#app",
        data:{//属性，vm
            message:'test'
        },
        methods:{
            currentTime01:function(){
                return Date.now();//返回一个时间戳
            }
        },
        computed:{//计算属性：methods，computed方法名不能重复，重名之后，只会调用methods的方法
            currentTime02:function(){
                this.message;
                return Date.now();
            }
        }

    });
</script>
</body>
</html>
```

- 注意：methods和computed里的方法等不能重名
- 说明：
  - Methods:定义方法，调用方法使用 funcitonName(),需要带括号
  - computed:定义计算属性，调用属性使用functionName，不需要带括号，this.message是为了能够让curerntTime02观察到数据的变化而变化。
- 结论
  - 调用方法时，每次都需要进行计算，既然有计算过程，必定产生系统开销，那如果这个结果是不经常变化的呢？此时就可以考虑将这个结果缓存起来，使用计算属性可以很方便做到这一点，计算属性的主要特征就是为了将不经常变化的计算结果进行缓存，节约系统的开销。



### 内容分发

> 在vueJs中我们使用<slot>元素作为承载分发内容的出口，作者称其为 **插槽**，可以应用在组合组件的场景中

#### 测试

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-items slot="todo-items" v-for="item in items" :item="item"></todo-items>
    </todo>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    Vue.component("todo",{
        template: ' <div>\
                        <slot name="todo-title"></slot>\
                        <ul>\
                           <slot name="todo-items"></slot>\
                        </ul>\
                    </div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template:'<div>{{title}}</div>'
    });

    Vue.component("todo-items",{
        props:['item'],
        template:'<li>{{item}}</li>'
    });

    let vm = new Vue({
        el:"#app",
        data:{
            title:"kidjaya的博客",
            items:[
                'java',
                'php',
                'python'

            ]
        }

    });
</script>
</body>
</html>
```



### 自定义事件

> Vue提供了自定义事件解决了参数传递和事件分发的问题，使用
>
> $emit('自定义事件名',参数)

#### 例子

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!--View层-->
<div id="app">
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-items slot="todo-items" v-for="(item,index) in items"
                    :item="item" :index="index"
                    v-on:remove="removeItems(index)" :key="index">

        </todo-items>
    </todo>
</div>

<!--导入vueJs-->
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    Vue.component("todo",{
        template: ' <div>\
                        <slot name="todo-title"></slot>\
                        <ul>\
                           <slot name="todo-items"></slot>\
                        </ul>\
                    </div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template:'<div>{{title}}</div>'
    });

    Vue.component("todo-items",{
        props:['item','index'],
        //只能绑定当前组件的方法
        template:'<li>{{index}}----{{item}}<button @click="remove">删除</button></li>',
        methods: {
            remove:function (index) {
                //自定义时间分发
                this.$emit("remove",index)
            }
        }

    });

    let vm = new Vue({
        el:"#app",
        data:{
            title:"kidjaya的博客",
            items:[
                'java',
                'php',
                'python'

            ]
        },
        methods:{
            removeItems:function(index){
                console.log("删除了"+this.items[index]+"ok");
                this.items.splice(index,1);//一次只删除一个元素(当前下标)
            }
        }

    });
</script>
</body>
</html>
```



#### 逻辑整理

![image-20200711155008729](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200711155008729.png)

### Vue的生命周期(钩子函数)

![image-20200713103240846](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200713103240846.png)

### Vue 入门小结

核心：数据驱动，组件化

优点：借鉴了AngulaJs的模块化开发和React的虚拟DOM，虚拟DOM就是把DOM操作放到内存中行执

常用属性：

- v-if
- v-else-if
- v-else
- v-for
- v-on 绑定事件，可以简写为@
- v-model 数据双向绑定
- v-bind 给组件绑定参数，简写为:

组件化

- 组件组合slot插槽
- 组件内部绑定事件需要用到 this.$emit("事件名",参数)；
- 计算属性的特色，缓存计算数据

遵循Soc关注度分离原则，Vue是纯粹的视图框架，并不包括，比如Ajax之类的通信功能，为了解决通信问题，我们需要使用Axios框架做移步通信

说明

- Vue的开发都要基于NodeJs，实际开发采用vue-cli脚手架开发，vue-router路由，vuex状态管理；
- Vue，UI界面我们一般使用ElementUI（饿了么）或者ICE（阿里巴巴）来快速搭建前端项目

---

## Vue-cli

### 是什么

> 官方提供的一个脚手架，用于快速生成一个vue项目模版
>
> 预先定义好目录结构以及基础代码，就好比Maven项目时选择Module骨架，这个骨架项目就是脚手架，使我们的开发更加的快速

- 主要功能
  - 统一的目录结构
  - 本地调试
  - 热部署
  - 单元测试
  - 集成打包上线

### 环境需求

- NodeJs
  - node -v 测试安装是否成功
- Git
  - npm -v 测试安装是否成功

#### 相关命令

- Vue
  - vue -V查看vue版本
- vue-init webpack myvue 创建模版
- npm run dev 启动vue(加载依赖) ctrl c 退出
- npm更换淘宝镜像 npm config set registry https://registry.npm.taobao.org

---

## Webpack

#### 是什么（类似Maven）

​	本质上，webpack是一个现代JavaScript应用程序的静态模块打包器（module bundler）当webpack处理应用程序时，他会递归的构建一个**依赖关系图**（dependency graph），其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个**bundle**

​	Webpack是当下最热门的**前端资源模块化管理和打包工具**，他可以将许多松散耦合的模块按照依赖和规则打包成复合生产环境部署的前端资源。还可以将**按需加载的模块进行代码分离，等到时机需要加载模块进行代码分离**，等到实际需要时再异步加载。通过loader转换，**任何形式的资源**都可以当作**模块**，比如CommonsJs、AMD、ES6、CSS、JSON、CoffeeScript，LESS等

​	很多网站从网页模式进化到了WebApp模式。它们运行在现在浏览器里，使用HTML5、CSS3、ES6等新的技术来开发丰富的功能，网页已经不仅是完成浏览器的基本需求；WebApp通常是一个SPA（单页应用），**每一个视图通过异步的方式加载，这导致页面初始化和使用过程中会加载越来越多的代码**，这给前端的开发流程和资源组织带来了巨大挑战。

​	前端开发和其他开发工作的主要区别，首先前端是基于多语言、多层次的编码和组织工作，其次前端产品的交付是**基于浏览器**的，这些资源是通过增量的方式运行到浏览器端，**如何在开发环境组织好这些碎片化的代码和资源，并且保证它们在浏览器端快速、优雅的加载和更新，就需要一个模块化系统**，这个理想中的模块化系统是前端工程师一直探索的难题



#### 模块化的演进

##### Script标签

```html
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="module3.js"></script>
```

- 这是最原始的JavaScript文件加载方式，如果把每一个文件看作是一个模块，那么他们的接口通常是暴露在全局作用域下，也就是定义在window对象中，不同模块的调用都是一个作用域。
- 弊端
  1. 全局作用域下容易造成变了冲突
  2. 文件只能按照<script>的书写顺序进行加载
  3. 开发人员必须主观解决模块和代码库的依赖关系
  4. 在大型项目中各种资源难以管理，长期积累的问题导致代码库混乱不堪

##### CommonsJs

​	服务器端的NodeJs遵循CommonsJS规范，该规范核心思想是允许代码模块通过require方法来同步加载所需依赖和其他模块，然后通过exports或module.exports来到处需要暴露的接口

```javascript
require("module01");
require("../module02.js");
export.doStuff = function(){};
module.exports = someValue;
```

- 优点

  - 服务器端模块便于重用
  - NPM中已有超过45万个可以使用的模块包
  - 简单易用

- 缺点：

  - 同步的模块加载方式不适合在浏览器环境，同步意味着阻塞加载，浏览器资源是异步加载的
  - 不能非阻塞的并行加载多个模块

- 实现

  - 服务端的NodeJS
  - Browserify，浏览器端的CommonJs实现，可以使用NPM的模块，但是编译打包后的文件体积大
  - modules-webmake，蕾丝Browserify，但不如它灵活
  - wreq，Browserify的前身

- 概念知识

  - Browserify

    - Browserify 可以让你使用类似于 node 的 require() 的方式来组织浏览器端的 Javascript 代码，通过[预编译](https://baike.baidu.com/item/预编译/3191547)让前端 Javascript 可以直接使用 Node NPM 安装的一些库

    ![image-20200711232755568](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200711232755568.png)

##### AMD

- Asynchronous Modul Definition 规范其实就是一个主要接口define(id?,dependencies?,factory)；它要在声明模块的时候指定所有的依赖denpendencies，并且还要当作形式参数传到factory，对于以来的模块提前执行。

```javascript
define("module01",{"dep1","dep2"},function(d1,d2){
  return someExportedValue;
});
require(["module","../file.js"],function(module,file){});
```

- 优点
  - 适合在浏览器环境中异步加载模块
  - 可以并行加载多个模块
- 缺点
  - 提高了开发成本，代码的阅读和书写比较困难，模块定义方式的语义不畅
  - 不符合通用的模块化思维方式，是一种妥协的实现

- 实现
  - RequireJs
  - curl
- 概念知识
  - RequireJS 是一个JavaScript模块加载器。它非常适合在浏览器中使用，但它也可以用在其他脚本环境，就像 Rhino and Node。使用RequireJS加载模块化脚本将提高代码的加载速度和质量。

##### CMD

- Commons Module Definition 规范和AMD相似，尽量保持简单，并与CommonsJS和NodeJs的Modules规范保持了很大的兼容性

```javascript
define(function(require,exports,module){
  var $ = require("jquery");
  var Spinning = require("./spinning");
  exports.doSomething = ..;
  module.exports = ..;
});
```

- 优点
  - 依赖久近，延迟执行
  - 可以很容易在NodeJs中执行
- 缺点
  - 依赖SPM打包，模块的加载逻辑偏重
- 实现
  - Sea.Js
  - Coolie
- 概念知识
  - SPM 是 CMD 的包管理工具，需要和 [Sea.js](http://www.oschina.net/p/seajs) 配合使用。
  - SeaJS是一个遵循CMD规范的JavaScript模块加载框架，可以实现JavaScript的模块化开发及加载机制。

##### ES6模块

- EcmaScript6标准增加了javaScript语言层面的模块体系定义。ES6模块的设计，是尽量静态化，使编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonsJs和AMD模块，都只能在运行时确定这些东西。

```javascript
import "jquery";
export function doStuff(){}
module "localModule"{}
```

- 优点
  - 容易进行静态分析
  - 面向未来的EcmaScript
- 缺点
  - 原生浏览器端还没有实现该标准
  - 全新的命令，新版的NodeJs才支持
- 实现
  - Babel
- 相关概念
  - 利用babel就可以让我们在当前的项目中随意的使用这些新最新的es6，甚至es7的语法。说白了就是把各种javascript千奇百怪的语言统统专为浏览器可以认识的语言
  - babel的核心概念就是利用一系列的plugin来管理编译案列，通过不同的plugin，他不仅可以编译es6的代码，还可以编译react JSX语法或者别的语法，甚至可以使用还在提案阶段的es7的一些特性，这就足以看出她的可扩展性。
- 期望的模块画系统
  - 可以兼容多种模块风格，尽量可以利用已有的代码，不仅仅知识javaScript模块化，还有CSS、图片、字体等资源也需要模块化

#### Webpack使用

- Webpack是一款模块加载器兼打包工具，他能把各种资源，如JS、JSX、ES6、SASS、LESS、图片等都作为模块来处理和使用

- 安装

  - npm install webpack -g
  - npm install webpack-cli -g

- 测试

  - webpack -v
  - webpack-cli -v

- 配置

  > 创建webpack.config.js 配置文件

  

  - entry：入口文件，指定webpack用那个文件作为项目的入口
  - output：输出，指定webpack吧处理完的文件放到指定的路径
  - module：模块，用于处理各种类型的文件
  - plugins：插件，如：热更新，代码重用等
  - resolve：设置路径指向
  - watch：监听，用于设置文件改动后直接打包

  ```javascript
  module.exports = {//打包！
      entry:'./modules/main.js',//入口
      output:{
          filename:"./js/bundle.js"
      },
      module:{
          loaders:[
              {test:/\.js$/,loader:""}
          ]
      },
      plugins:{},
      resolve:{},
      watch:true
  };
  ```

  

- 运行
  - webpack
- 监听
  - webpack --watch

- 例子

```javascript
//暴露一个方法
exports.sayHello = function(){
    document.write("<h1>Hello,World</h1>")
};
```

```javascript
//引入
var hello = require("./hello");
hello.sayHello();
```

```javascript
module.exports = {//打包！
    entry:'./modules/main.js',//入口
    output:{
        filename:"./js/bundle.js"//出口
    },
    module:{
        loaders:[
            {test:/\.js$/,loader:""}
        ]
    },
    plugins:{},
    resolve:{},
    watch:true
};
```



## vue-router

### 安装

​	vue-router是一个插件包，所以我们还是需要用npm/cnpm来进行安装。输入命令

```bash
npm install vue-router --save-dev
```

如果在一个模块化开发工程使用它，必须通过Vue.use()明确地安装路由功能

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

//安装路由
Vue.use(VueRouter);
```

### 步骤

1. 安装router并配置好 新建router文件夹并创建index.js文件

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

//安装路由
Vue.use(VueRouter);

//配置导出路由
export default new VueRouter({
  routes:[]
});

```

2. 在APP入口main.js导入router

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'  //自动扫描index名文件

Vue.config.productionTip = false;

/* eslint-disable no-new */
new Vue({
  el: '#app',
  //配置路由
  router,
  components: { App },
  template: '<App/>'
})
```

3. 创建相关test组件

```vue
<template>
  <h1>内容页</h1>
</template>

<script>
    export default {
        name: "Content"
    }
</script>

<style scoped>//代表当前组件范围内

</style>
```



```vue
<template>
    <h1>抬头仰望星空，路上脚踏实地</h1>
</template>

<script>
    export default {
        name: "Kid"
    }
</script>

<style scoped>

</style>
```

```vue
<template>
  <h1>首页</h1>
</template>

<script>
    export default {
        name: "Main"
    }
</script>

<style scoped>

</style>
```

4. 在router/index.js文件中配置路由指向，相关参数

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

import Content from "../components/Content";
import Main from "../components/Main";
import Kid from "../components/Kid";

//安装路由
Vue.use(VueRouter);

//配置导出路由
export default new VueRouter({
  routes:[
    {
      //路由路径
      path:'/content',
      name:'content',
      //跳转的组件
      component:Content
    },
    {
      //路由路径
      path:'/main',
      name:'main',
      //跳转的组件
      component:Main
    },{
      //路由路径
      path:'/kid',
      name:'kid',
      //跳转的组件
      component:Kid
    }
  ]
});
```

5. 在App.vue中写入路由命令

```vue
<template>
  <div id="app">
      <router-link to="/main">首页</router-link>
      <router-link to="/content">内容页</router-link>
      <router-link to="/kid">kid</router-link>
      <router-view></router-view>

  </div>
</template>

<script>
  import Content from './components/Content'
export default {
  name: 'App',
  components: {
    Content
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

----

## Vue+ElementUI

- elementUI官网 https://element.eleme.cn/#/zh-CN/component/quickstart

### 步骤

1. 安装

```shell
npm i element-ui -S
```

2. 创建组件

```vue
<template>
  <div>
    <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="密码" prop="pass">
        <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="checkPass">
        <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" prop="age">
        <el-input v-model.number="ruleForm.age"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
    export default {
      name:'Login',
      data() {
        var checkAge = (rule, value, callback) => {
          if (!value) {
            return callback(new Error('年龄不能为空'));
          }
          setTimeout(() => {
            if (!Number.isInteger(value)) {
              callback(new Error('请输入数字值'));
            } else {
              if (value < 18) {
                callback(new Error('必须年满18岁'));
              } else {
                callback();
              }
            }
          }, 1000);
        };
        var validatePass = (rule, value, callback) => {
          if (value === '') {
            callback(new Error('请输入密码'));
          } else {
            if (this.ruleForm.checkPass !== '') {
              this.$refs.ruleForm.validateField('checkPass');
            }
            callback();
          }
        };
        var validatePass2 = (rule, value, callback) => {
          if (value === '') {
            callback(new Error('请再次输入密码'));
          } else if (value !== this.ruleForm.pass) {
            callback(new Error('两次输入密码不一致!'));
          } else {
            callback();
          }
        };
        return {
          ruleForm: {
            pass: '',
            checkPass: '',
            age: ''
          },
          rules: {
            pass: [
              { validator: validatePass, trigger: 'blur' }
            ],
            checkPass: [
              { validator: validatePass2, trigger: 'blur' }
            ],
            age: [
              { validator: checkAge, trigger: 'blur' }
            ]
          }
        };
      },
      methods: {
        submitForm(formName) {
          this.$refs[formName].validate((valid) => {
            if (valid) {
              //使用vue-router路由到指定页面，该方法称之为编程式导航
              this.$router.push("/main")
            } else {
              console.log('error submit!!');
              return false;
            }
          });
        },
        resetForm(formName) {
          this.$refs[formName].resetFields();
        }
      }
    }
</script>

<style scoped>

</style>
```

3. 配置路由

```javascript
import Vue from 'vue'
import VueRouter from "vue-router";
import Main from "../views/Main";
import Login from "../views/Login";

Vue.use(VueRouter);

export default new VueRouter({
  mode: 'history',
  routes:[//注意不要吧routes写成router
    {
      path:'/main',
      name:'Main',
      component:Main
    },
    {
      path:'/',
      name:'Login',
      component:Login
    }
  ]
});
```

4. 设置main.js入口，进行相关配置

```javascript
import Vue from 'vue'
import App from './App'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

import router from './router'

Vue.config.productionTip = false;

Vue.use(ElementUI);

new Vue({
  el: '#app',
  router,
  render: h => h(App)
});

```

5. 在APP vue 加入router-view

 ```vue
<template>
  <div id="app">
    <router-link to="/main">main</router-link>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App'
}
</script>
 ```

### 嵌套子路由

1. 创建子组件

```vue
<template>
  <h1>用户列表</h1>
</template>

<script>
    export default {
        name: "List"
    }
</script>

<style scoped>

</style>
```

2. 配置路由

```javascript
import Vue from 'vue'
import VueRouter from "vue-router";
import Main from "../views/Main";
import Login from "../views/Login";
import List from "../views/user/List";
import Profile from "../views/user/Profile";

Vue.use(VueRouter);

export default new VueRouter({
  mode: 'history',
  routes:[
    {
      path:'/main',
      name:'Main',
      component:Main,
      children:[
        {path:'/user/list',name:List,component:List},
        {path:'/user/profile',name:Profile,component:Profile},
      ]
    },
    {
      path:'/login',
      name:'Login',
      component:Login
    }
  ]
});
```

3. 载入到组件，设置跳转router-link 

```vue
<template>
  <el-container style="height: 500px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <el-menu :default-openeds="['1', '3']">
        <el-submenu index="1">
          <template slot="title">
            <i class="el-icon-message"></i>用户管理
          </template>
          <el-menu-item-group>
            <el-menu-item index="1-1">
              <router-link to="/user/profile">个人信息</router-link>
            </el-menu-item>
            <el-menu-item index="1-2">
              <router-link to="/user/list">用户列表</router-link>
            </el-menu-item>
          </el-menu-item-group>
        </el-submenu>

        <el-submenu index="2">
          <template slot="title">
            <i class="el-icon-menu"></i>内容管理
          </template>
          <el-menu-item-group>
            <el-menu-item index="2-1">分类管理</el-menu-item>
            <el-menu-item index="2-2">内容列表</el-menu-item>
          </el-menu-item-group>
        </el-submenu>

        <el-submenu index="3">
          <template slot="title">
            <i class="el-icon-menu"></i>文章管理
          </template>
          <el-menu-item-group>
            <el-menu-item index="3-1">分类管理</el-menu-item>
            <el-menu-item index="3-2">内容列表</el-menu-item>
          </el-menu-item-group>
        </el-submenu>

      </el-menu>
    </el-aside>

    <el-container>
      <el-header style="text-align: right; font-size: 12px">
        <el-dropdown>
          <i class="el-icon-setting" style="margin-right: 15px"></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>个人信息</el-dropdown-item>
            <el-dropdown-item>退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-header>

      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<style>
  .el-header {
    background-color: #b3c0d1;
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }
</style>

<script>
  export default {
    data() {
      const item = {
        date: "2020-07-12",
        name: "羊大大",
        address: "深圳市"
      };
      return {
        tableData: Array(20).fill(item)
      };
    }
  };
</script>
```

### 参数传递&&重定向

- 步骤

  1. 路由绑定参数&&**重定向**

  ```javascript
  export default new VueRouter({
    mode: 'history',
    routes:[
      {
        path:'/main',
        name:'Main',
        component:Main,
        children:[
          {
            path:'/user/list/:id',
            name:'List',
            component:List,
            props:true//声明可以使用props传递参数
          },
          {
            path:'/user/profile/:id',
            name:'Profile',
            component:Profile
          },
        ]
      },
      {
        path:'/login',
        name:'Login',
        component:Login
      },{
        path:'/goHome',
        redirect:'/main'//设置重定向
  
      }
    ]
  });
  ```

  2. 设置router-link路由参数

  ```vue
  <el-menu-item index="1-1">
    <router-link :to="{name: 'Profile',params:{id:1}}">个人信息</router-link>
  </el-menu-item>
  <el-menu-item index="1-2">
    <router-link :to="{name:'List',params:{id:1}}">用户列表</router-link>
  </el-menu-item>
  <el-menu-item index="1-3">
    <router-link to="/goHome">回到首页</router-link>
  </el-menu-item>
  ```

  3. 组件接收参数

  - 普通

  ```vue
  <template>
  <!--所有的元素，必须不能直接在根节点下-->
    <div>
      <h1>用户信息</h1>
      {{$route.params.id}}
    </div>
  </template>
  
  <script>
      export default {
          name: "Profile"
      }
  </script>
  
  <style scoped>
  
  </style>
  ```

  - props接收

  ```vue
  <template>
    <div>
      <h1>用户列表</h1>
      {{id}}
    </div>
  </template>
  
  <script>
      export default {
          props:['id'],
          name: "List"
      }
  </script>
  
  <style scoped>
  
  </style>
  ```

### 404和钩子函数

#### 路由模式和404

- 路由模式
  - hash：路径带#符号，如http://localhost/#/login
  - history:路径不带#符号，如http://localhost/login

- 修改路由配置

```javascript
export default new VueRouter({
  mode: 'history',
  routes:[]
});
```

- 配置404页面

  - 创建组件NotFound为404页面

  ```vue
  <template>
      <h1>404，你的页面走丢了</h1>
  </template>
  
  <script>
      export default {
          name: "NotFound"
      }
  </script>
  
  <style scoped>
  
  </style>
  ```

  - 修改路由配置

  ```javascript
  {//放在最下面，为默认匹配路径，相当于if，else中的else
    path:'*',
    component:NotFound
  }
  ```

#### 路由钩子与异步请求

- beforeRouterEnter：在进入路由之前执行
- beforeRouterLeave：在离开路由之后执行

```javascript
export default {
  name: "Profile",
  data:{

  },
  beforeRouteEnter:(to,from,next)=>{
    console.log("进入路由之前操作");
    next(vm=>{
      vm.getData();//进入路由之前执行该方法
    });
  },
  beforeRouteLeave:(to,from,next)=>{
    console.log("进入路由之后操作");
    next();
  }
}
```

- 参数说明：
  - to 路由将要跳转的路径信息
  - from 路由跳转前的路径信息
  - next 路由的控制参数
    - next() 跳入下一个路由
    - next('/path') 改变路由的跳转方向，使其跳到另一个路由
    - next(false) 返回原来的页面
    - next(vm=>{}) 仅在beforeRouteEnter中可用，vm是组件实例

- 安装axios
  - 教程：http://www.axios-js.com/zh-cn/docs/vue-axios.html

  - ```shell
    npm install --save axios vue-axios
    ```

  - 配置main.js

  - ```javascript
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
    
    Vue.use(VueAxios, axios)
    ```

- 使用axios获取数据

```vue
<template>
<!--所有的元素，必须不能直接在根节点下-->
  <div>
    <h1>用户信息</h1>
    {{$route.params.id}}
    <p>1{{userInfo.name}}</p>
    <p>2{{userInfo.address.street}}</p>
    <p>3{{userInfo.address.city}}</p>
    <p>4{{userInfo.address.country}}</p>
    <p>个人博客：{{userInfo.url}}</p>
  </div>
</template>

<script>
    export default {
      name: "Profile",
      data(){
        return{
          userInfo:{
            name: null,
            address: {
              street: null,
              city: null,
              country: null
            },
            url: null
          }
        }
      },
      beforeRouteEnter:(to,from,next)=>{
        console.log("进入路由之前操作");
        next(vm=>{
          vm.getData();//进入路由之前执行该方法
        });
      },
      beforeRouteLeave:(to,from,next)=>{
        console.log("进入路由之后操作");
        next();
      },
      mounted() {//官方推荐载入数据的钩子函数
        this.axios({
          method:'get',
          url:'http://localhost:8080/static/mock/data.json'
        }).then(response=>(this.userInfo = response.data))
      },
      methods:{
        getData:function () {
          this.axios({
            method:'get',
            url:'http://localhost:8080/static/mock/data.json'
          }).then(function (response) {
             console.log(response)
          })
        }
      }

    }
</script>

<style scoped>

</style>
```

- 效果

![image-20200713095440208](https://gitee.com/kidjaya/imageStorage/raw/master/images/image-20200713095440208.png)