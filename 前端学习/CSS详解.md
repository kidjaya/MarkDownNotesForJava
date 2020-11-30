# CSS详解

> 视频参考：https://www.bilibili.com/video/BV1YJ411a7dy

> 1. CSS是什么
> 2. CSS怎么用
> 3. **CSS选择器**
> 4. 美化网页
> 5. 盒子模型
> 6. 浮动
> 7. 定位
> 8. 网页动画

## 1.CSS是什么

### 1.1CSS简介

Cascading Style Sheet 层叠联级央视表

CSS：表现（美化网页）

- 字体，颜色，边距，高度，宽度，背景图片，网页定位，网页浮动...

![tyzpp8.png](https://s1.ax1x.com/2020/06/06/tyzpp8.png)



### 1.2CSS发展史

CSS1.0 

CSS2.0    DIV(块)+CSS，HTML与CSS结构分离的思想，网页变得简单，SEO

CSS2.1	浮动、定位

CSS3.0	圆角，阴影，动画...需要浏览器兼容



### 1.3快速入门

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--规范 style可以编写css代码，每一个声明最好用分号结尾
    语法：
        选择器{
            声明1;
            声明2;
            声明3;
        }
    -->
    <style>
        h1{
            color: cadetblue;
        }
    </style>

    <!--建议使用这种方式-->
    <link rel="stylesheet" href="css/style.css">
</head>
<body>

<h1>我是标题</h1>

</body>
</html>
```

**css的优势：**

1. 内容和表现分离
2. 网页结构表现统一，可以实现复用
3. 样式十分丰富
4. 建议使用独立于html的css文件
5. 利于SEO，容易被搜索引擎收录



### 1.4CSS的导入方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>导入方式</title>
    <!--内部样式-->
    <link rel="stylesheet" href="css/style.css">

    <style>
        h1{
            color: green;
        }
    </style>

</head>
<body>
<!--优先级：就近原则-->
<!--行内样式：在标签元素中，编写一个style属性，编写样式即可-->
<!--<h1 style="color: red">Demo02-1</h1>-->
<h1>Demo02-2</h1>
</body>
</html>
```

扩展：外部样式两种写法

- 链接式

```html
    <!--链接式-->
    <link rel="stylesheet" href="css/style.css">
```

- 导入式
  - @import是css2.1之后特有的

```html
 <style>
        /*CSS2.1的写法！！导入式 缺点：先出骨架，后出肉*/
        @import "css/style.css";
</style>
```



## 2.选择器

> 作用：选择页面上的某一个或者某一种元素

### 2.1.基本选择器

1. 标签选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        /*标签选择器，会选择到页面上的所有这个标签的元素
        */
        h1{
            color: green;
        }

        p{
            font-size: 80px;
        }
    </style>
</head>
<body>

<h1>标题1</h1>
<h1>标题2</h1>
<h1>哈哈哈哈哈</h1>
<p>文字p</p>

</body>
</html>
```

2. 类选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        /* 类选择的格式 .class的名称{}
        好处：可以多个标签归类，就是同一个class；可以复用
        */
        .test1{
            color: red;
        }
        .test2{
            color: yellow;
        }
        .test3{
            color: blue;
        }
    </style>
</head>
<body>

<h1 class="test1">test</h1>
<h1 class="test2">test</h1>
<h1 class="test3">test</h1>

</body>
</html>
```

3. ID选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        /*id选择器：id必须保证全局唯一
        #id的名称{}
        不遵循就近原则，固定的
        id选择器>class选择器（群组）>标签选择器
        */
        #test{
            color: yellow;
            background-color: cornflowerblue;
            border-radius: 20px;
            font-size: 40px;
        }

        .aaa{
            color: green;
            font-size: 60px;
        }
    </style>
</head>
<body>

<h1 class="aaa" id="test">test</h1>
<h1 class="aaa" id="test2">test</h1>
<h1 class="aaa">test</h1>
<h1 class="aaa">test</h1>

</body>
</html>
```

**优先级顺序：Id>class>tag**



### 2.2.层次选择器

1. 后代选择器 在某个元素的后面

```html
 <style>
        body p{
            color: green;
            font-size: 80px;
</style>
```

2. 子选择器 就传递一代

```html
<style>
        body>p{
            color: green;
            font-size: 80px;
        }
</style>
```

3. 相邻兄弟选择器

```html
<style>
/*相邻兄弟选择器: 只有一个，相邻（向下）,当前元素的向下兄弟一个*/
.test + p{
            background: green;
            font-size: 80px;
}
</style>
```

4. 通用选择器

```html
 <style type="text/css">
        /*通用兄弟选择器，当前选中元素的向下的所有兄弟*/
        .test~p{
            background: yellow;
        }
</style>
```



### 2.3.结构伪类选择器

伪类

```html
<style>
  			/*伪类*/
        a:hover{
            background: crimson;
        }
</style>
...
```



```html
<style>
        /*ul的第一个子元素*/
        ul li:first-child{
            background: coral;
        }
        /*ul的最后一个子元素*/
        ul li:last-child{
            background: cornflowerblue;
        }

        /*选中p1：定位到父元素，选择当前第一个元素
        选择当前p元素的父级元素，选中父级元素的第一个，并且是当前元素才生效 按顺序
        */
        p:nth-child(2){
            background: aqua;
        }
        /*选中父元素，下的p元素的第二个，按类型*/
        p:nth-of-type(3){
            background: violet;
        }
</style>
...
```



### 2.4.属性选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        .demo a{
            float: left;
            display: block;
            height: 50px;
            width: 50px;
            border-radius: 10px;
            background: sandybrown;
            text-align: center;
            color: whitesmoke;
            text-decoration: none;
            margin-right: 5px;
            font: bold 20px/50px Arial;
        }

        /*存在id属性的元素 a[]{}
        1.[属性名]
        2.[id=属性值（可以使用正则）]

      相当于正则表达式：
        = 绝对等于
        *= 包含这个元素
        ^= 以这个开头
        $= 以这个结尾

        */

        a[class*="links"]{
            background: blueviolet;
        }

        a[id]{
            background: red;
        }

        a[id=end]{
            background: cornflowerblue;
        }

        /*选中href中以http开+头的元素*/
        a[href^=http]{
            background: black;
        }

        /*以xyz结尾的*/
        a[href$="xyz/"]{
            background: sandybrown;
        }

    </style>
</head>
<body>
<p class="demo">
    <a href="http://kidjaya.xyz/" class="links item first" id="first">1</a>
    <a href="" class="links item active" target="_blank" title="i am two">2</a>
    <a href="" class="links item">3</a>
    <a href="" class="links item">4</a>
    <a href="" class="links item">5</a>
    <a href="" class="links item">6</a>
    <a href="" class="links item">7</a>
    <a href="http://www.baidu.com/" id="end" class="links item">8</a>
</p>
</body>
</html>
```



## 3.美化网页元素

### 3.1为什么要美化网页元素

1. 有效的传递页面消息
2. 美化网页，页面漂亮，才能吸引用户
3. 凸显页面的主题
4. 提高用户体验

**span标签：重点要突出的字，使用span标签套起来**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        #title1{
            font-size: 50px;
        }
    </style>
</head>
<body>
    欢迎学习<span id="title1">java</span>
</body>
</html>
```



### 3.2.字体样式

```html
 <!--
        font-family:字体风格
        font-size:字体大小
        font-weight:字体粗细
        color:字体颜色
    -->
    <style>
        body{
            font-family: Arial,"Fira Code";
            color: blueviolet;
        }

        h1{
            /*px像素 em缩进*/
            font-size: 20px;
        }

        .p1{
            font-weight: 900;
        }
</style>

 <style>
        /*字体风格*/
        p{
            font:oblique lighter 60px Arial;
        }
    </style>
```

### 3.3.文本样式

1. 颜色
   - color
   - rgb
   - rbga
2. **文本对其方式**
   - **text-align:center**
3. **首行锁进**
   - **text-indent**
4. **行高**
   - **line-height：单行文字上下居中！line-hight=height**
5. 装饰
   - text-decoration
6. 文本图片水平对其
   - vertical-align：center

```html
    <style>
        /*
        颜色：
            单词
            RGB red green blue 0～F
            RGBA A:0～1 透明度

        排版 居中：   text-align: center;
        段落首行锁进： text-indent: 2em;

        行高和块的高度一致就可以上下居中   height: 300px;line-height: 300px;
        */
        h1{
            color: rgba(255,0,255,0.9);
            text-align: center;
        }

        .p1{
            text-indent: 2em;
        }

        .p2{
            background: cornflowerblue;
            height: 300px;
            text-align: center;
            line-height: 300px;
        }

        /*下划线*/
        .l1{
            text-decoration: underline;
        }
        /*中划线*/
        .l2{
            text-decoration: line-through;
        }
        /*上划线*/
        .l3{
            text-decoration: overline;
        }
    </style>
```

```html
    <style>
        /*水平对其，参照物  A,B*/
        img,span{
            vertical-align: middle;
        }
    </style>
```

 ### 3.5.阴影

![tDfZrj.png](https://s1.ax1x.com/2020/06/05/tDfZrj.png)

```css
        #price{
            /*阴影颜色 水平偏移 垂直偏移 阴影半径*/
            text-shadow: cornflowerblue 10px 10px 2px;
        }
```

### 3.5 超链接伪类

```css
    <style>
        /*默认颜色*/
        a{
            text-decoration: none;
            color: black;
        }

        /*已访问*/
        a:visited{
            color: saddlebrown;
        }

        /*鼠标悬停颜色*/
        a:hover{
            color: orange;
        }
        /*鼠标点击未释放*/
        a:active{
            color: green;
        }


        #price{
            /*阴影颜色 水平偏移 垂直偏移 阴影半径*/
            text-shadow: cornflowerblue 10px 10px 2px;
        }
    </style>
```



### 3.6列表

```css
#nav{
    width: 300px;
    background: grey;
}
.title{
    font-size: 30px;
    font-weight: bold;
    text-indent: 1em;
    line-height: 35px;
    background: coral;
}

/*ul{*/
/*    background: grey;*/
/*}*/

/*
list-style
none 去掉圆点
circle 空心圆
decimal 数字
square 正方形
*/
ul li{
    height: 30px;
    list-style: none;
    text-indent: 2em;
}

a{
    text-decoration: none;
    font-size: 20px;
    color: antiquewhite;
}
a:hover{
    color: orange;
    text-decoration: underline;
}
```



### 3.7背景

1. 背景颜色
2. 背景图片

```css
    <style>
        div{
            width: 1000px;
            height: 700px;
            border: 1px solid red;
            /*默认全部平铺 repeat*/
            background-image: url("images/img.jpeg");
        }

        .div1{
            background-repeat: repeat-x;
        }

        .div2{
            background-repeat: repeat-y;
        }

        .div3{
            background-repeat: no-repeat;
        }
    </style>
```

![trP5VA.png](https://s1.ax1x.com/2020/06/05/trP5VA.png)



### 3.8.渐变

grabient:https://www.grabient.com/

```css
 <style>
       body{
           background-color: #52ACFF;
           background-image: linear-gradient(90deg, #52ACFF 25%, #FFE32C 100%);
       }
</style>
```



## 4.盒子模型

### 4.1什么是盒子模型

![trK8UO.png](https://s1.ax1x.com/2020/06/05/trK8UO.png)

- margin：外边距
- padding：内边距
- border：边框

### 4.2边框

1. 边框的粗细
2. 边框的样式
3. 边框的颜色

```css
<style>
        body,h1,h2,ul,li{
            /*总有一个默认的外边距*/
            margin: 0;
            padding: 0;
            text-decoration: none;
        }
        /*border：粗细，样式，颜色*/
        #box{
            width: 300px;
            border: 1px solid black;
        }

        h2{
            font-size: 20px;
            background-color: cornflowerblue;
            line-height: 40px;
            color: whitesmoke;
        }

        form{
            background: cadetblue;
        }

        div:nth-of-type(1) input{
            border: 3px solid black;
        }

        div:nth-of-type(2) input{
            border: 3px dashed red;
        }

        div:nth-of-type(3) input{
            border: 2px dashed black;
        }
</style>
```

### 4.3 内外边距

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body,h1,h2,ul,li{
            /*总有一个默认的外边距*/
            margin: 0;
            padding: 0;
            text-decoration: none;
        }
        /*border：粗细，样式，颜色*/
        #box{
            width: 300px;
            border: 1px solid black;
            /*上下 左右*/
            margin: 0 auto;
        }
        /*
            margin:0;
            margin:0 1px;
            margin:0 1px 2px 3px;
        */
        h2{
            font-size: 20px;
            background-color: cornflowerblue;
            line-height: 40px;
            color: whitesmoke;

        }

        form{
            background: cadetblue;
        }

        input{
            border: 1px solid black;
        }

        div:nth-of-type(1){
            padding:10px 2px;
        }
    </style>
</head>
<body>
<div id="box">
    <h2>会员登录</h2>
    <form action="#">
        <div>
            <span>用户名:</span>
            <input type="text">
        </div>
        <div>
            <span>密码:</span>
            <input type="text">
        </div>
        <div>
            <span>邮箱:</span>
            <input type="text">
        </div>
    </form>
</div>
</body>
</html>
```

盒子计算方式：你这个元素到底多大？

![trGUFx.png](https://s1.ax1x.com/2020/06/05/trGUFx.png)

**margin+padding+border+内容高度**

### 4.4 圆角边框

```css
<style>
    /*左上 右上 右下 左下*/
    div{
        width: 100px;
        height: 50px;
        border: 1px solid red;
        border-radius: 50px 50px 0 0;
    }

    img{
        border-radius: 100px;
    }
</style>
```

### 4.5 盒子阴影

```css
<style>
    div{
        width: 100px;
        height: 100px;
        border: 10px solid darkorange;
        border-radius: 100px;
        background-color: darkorange;
        box-shadow: 10px 10px 100px yellow;
    }

    img{
       margin: 0 200px;
    }
</style>
```



## 5.浮动

### 5.1 标准文档流

- 标准文档流
  - 没有布局的由上到下的顺序排布



- 块级元素

> h1~h6 	p	div	列表...

- 行内元素

> span 	a	img	strong...

- 行内元素 可以被包含在块级元素中，反之则不可以～

### 5.2display

```css
<style>
    /*
    block 块元素
    inline 行内元素
    inline-block 是块元素，但可以在一行，简称行内块元素
    */
    div{
        width: 100px;
        height: 100px;
        border: 1px solid red;
        display: inline-block;
    }

    span{
        width: 100px;
        height: 100px;
        border: 1px solid red;
        display: inline-block;
    }
</style>
```

**这个也是一种实现行内元素排列的方式，但是我们很多情况都用float**



### 5.3float

1.  左右浮动 float

```css
<style>
    div{
        margin: 10px;
        padding: 5px;
    }

    #father{
        border: 1px solid black;
    }

    .layer01{
        border: 1px dashed red;
        display: inline-block;
        float: right;
    }

    .layer02{
        border: 1px solid orange;
        display: inline-block;
        float: right;
        clear: both;
    }

    .layer03{
        border: 1px solid green;
        display: inline-block;
        float: right;
        clear: both;
    }

    .layer04{
        border: 1px solid cornflowerblue;
        display: inline-block;
        float: right;
        /*清除浮动 自己一个 既有浮动效果，也有块元素效果，保证页面变化时，不会飘来飘去*/
        clear: both;
    }

</style>
```



### 5.4 父级边框塌陷的问题

```css
 				/*
        clear: right 右侧不允许有浮动元素
        clear: left 左侧不允许有浮动元素
        clear: both 两侧都不允许有浮动元素
        clear: nome 默认
        */
```



解决方案：

1. 增加父级元素的高度

```css
#father{
    border: 1px solid black;
    height: 900px;
}
```

2. 增加一个空的div,清除浮动

```html
<div class="clear"></div>
<style>
	.clear{
            clear: both;
            margin: 0;
            padding: 0;
  }
</style>
```

3. 在父级元素增加一个overflow:hidden

```css
#father{
            border: 1px solid black;
            overflow: hidden;
        }
```

4. 父类添加一个伪类after，避免像第二种，在html生成多了一个div问题

```css
 #father:after{
            content: '';
            display: block;
            clear: both;
        }
```

**小结：**

1. 浮动元素后面增加div
   - 简单，代码中尽量避免空div
2. 设置父元素高度
   - 简单，元素假如有了固定·高度，就会被限制
3. overflow
   - 简单，下拉的一些场景避免使用
4. 父类添加一个伪类：after（推荐）
   - 写法稍微复杂一点，但没有副作用，推荐使用



### 5.5 对比

- display
  - 方向不可控制
- float
  - 浮动起来的话会脱离标准文档流，所以要解决父类边框塌陷问题



## 6.定位

### 6.1 相对定位

```html
    <style>
        div{
            margin: 10px;
            padding: 5px;
            font-size: 12px;
            line-height: 25px;
        }

        #father{
            border: 1px solid #666;
            padding: 0;
            margin-top: 20px;
            padding: 20px;
        }

        #first{
            background-color: #52AC99;
            border: 1px dashed #52ACFF;
            position: relative;
            top: -10px;
            left: 20px;
            box-shadow: 10px -10px 10px #52ACFF;
        }

        #second{
            background-color: #2e8b99;
            border: 1px dashed darkcyan;
            box-shadow: 10px -10px 10px darkcyan;
        }

        #third{
            background-color: #2b8e99;
            border: 1px dashed seagreen;
            position: relative;
            bottom: -10px;
            right: 30px;
            box-shadow: 10px -10px 10px seagreen;
        }
    </style>
```

相对定位：position：relative

- 相对于原来的位置，进行指定的偏移，相对定位的话，他仍然在标准文档流中，原来的位置会被保留

```css
top: -10px;
left: 20px;
bottom: -10px;
right: 30px;
```

- 简单练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div,p,body{
            margin: 0;
            padding: 0;
        }

        p{
            text-align: center;
            line-height: 100px;
        }

        p:hover{
            background-color: #52ACFF;
        }

        #father{
            padding: 10px;
            width: 300px;
            height: 300px;
            border: 2px solid red;
        }

        #left_top{
            width: 100px;
            height: 100px;
            background-color: hotpink;
        }

        #left_bottom {
            width: 100px;
            height: 100px;
            position: relative;
            top: -100px;
            left: 200px;
            background-color: hotpink;
       }

        #middle{
            width: 100px;
            height: 100px;
            position: relative;
            top: -100px;
            left:100px;
            background-color: hotpink;
        }

        #right_top{
            width: 100px;
            height: 100px;
            position: relative;
            top: -100px;
            background-color: hotpink;
        }

        #right_bottom{
            width: 100px;
            height: 100px;
            position: relative;
            top: -200px;
            left: 200px;
            background-color: hotpink;
        }
    </style>
</head>
<body>
    <div id="father">
        <div id="left_top"><p>链接1</p></div>
        <div id="left_bottom"><p>链接2</p></div>
        <div id="middle"><p>链接3</p></div>
        <div id="right_top"><p>链接4</p></div>
        <div id="right_bottom"><p>链接5</p></div>
    </div>
</body>
</html>
```



### 6.2 绝对定位

> 定位：基于xxx，上下左右

1. 没有父级元素定位的前提下，相对于浏览器定位
2. 假设父级元素存在定位，通常会相对于父级元素进行偏移
3. 相对于父级元素或浏览器的位置，进行指定偏移，绝对定位的话，他不在标准文档流中，原来的位置不会被保留

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        div{
            padding: 5px;
            font-size: 12px;
            line-height: 25px;
        }

        #father{
            border: 1px solid #666;
            margin-top: 20px;
            position: relative;
        }

        #first{
            background-color: #52AC99;
            border: 1px dashed #52ACFF;
            position: absolute;
            top: 0px;
            right: 0px;
        }

        #second{
            background-color: #2e8b99;
            border: 1px dashed darkcyan;
            margin-bottom: 10px;
        }

        #third{
            background-color: #2b8e99;
            border: 1px dashed seagreen;
        }
    </style>
</head>
<body>
<div id="father">
    <div id="first">第一个盒子</div>
    <div id="second">第二个盒子</div>
    <div id="third">第三个盒子</div>
</div>
</body>
</html>
```



### 6.3 固定定位fixed

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body{
            height: 1000px;
        }
        div:nth-of-type(1){
            position: absolute;
            right: 0;
            bottom: 0;
            width: 200px;
            height: 200px;
            background-color: #2b8e99;
        }

        div:nth-of-type(2){
            position: fixed;
            right: 0;
            bottom: 0;
            width: 100px;
            height: 100px;
            background-color: orange;
        }
    </style>
</head>
<body>
<div>div1</div>
<div>div2</div>
</body>
</html>
```



### 6.4 z-index

![tyaqhj.png](https://s1.ax1x.com/2020/06/06/tyaqhj.png)

图层：

z-index: 默认0，最高无限

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div id="content">
        <ul>
            <li><img src="images/1.jpg" alt=""></li>
            <li class="tipText">学拍摄，找牛牛</li>
            <li class="tipBg"></li>
            <li>时间：2020-12-20</li>
            <li>地点：深圳湾公园</li>
        </ul>
    </div>
</body>
</html>
```

- **opacity: 0.5;背景透明度**

```css
body{
    margin: 0;
    padding: 0;
}

#content{
    width: 300px;
    overflow: hidden;
    font-size: 14px;
    line-height: 25px;
    border: 1px #000 solid;
}

/*父级元素相对定位*/
#content ul{
    position: relative;
}

.tipText,.tipBg{
    position: absolute;
    width: 300px;
    height: 25px;
    top: 175px;
}

img{
    width: 300px;
    height: 200px;
}

ul,li{
    margin: 0;
    padding: 0;
    list-style: none;
}

.tipBg{
    background: black;
    opacity: 0.5;/*背景透明度*/
    filter: alpha(opacity=50);
}

.tipText{
    color: #FFFFFF;
    z-index: 9;
}
```



## 7. 动画（待实践）

学习：https://www.w3school.com.cn/css3/css3_animation.asp

博客：https://www.cnblogs.com/qianguyihao/p/8435182.html



## 8. 总结

![tyxtyQ.png](https://s1.ax1x.com/2020/06/06/tyxtyQ.png)