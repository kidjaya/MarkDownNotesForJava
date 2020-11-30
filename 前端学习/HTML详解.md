# HTML详解

> 视频参考：https://www.bilibili.com/video/BV1x4411V75C?p=1

## 概述

> Hyper Text Marked Language（超文本标记语言）
>
> - 文本，视频，图片等...

- 优势
  1. 浏览器厂商对HTML5的支持
  2. 市场需求
  3. 跨平台

- w3c标准（World Wide Web Consortium）万维网联盟
  1. Web技术领域最具影响力的国际**中立性技术标准机构**
  2. https://www.chinaw3c.org/
- 标准包括
  1. 结构化标准语言 HTML XML
  2. 表现标准语言 CSS
  3. 行为标准 DOM ECMAScript

## 基本标签

```html
<!--DOCTYPE:告诉浏览器，我们要使用什么规范-->
<!DOCTYPE html>

<html lang="en">
<!--head标签代表网页头部-->
<head>
    <!--meta描述性标签，它用来描述我们网站的一些信息-->
    <!--meta一般来做SEO-->
    <meta charset="UTF-8">
    <meta name="keywords" content="kidjaya,记录个人学习记录">
    <meta name="keywords" content="编程学习经验交流">
    <!--title网页标题-->
    <title>Title</title>
</head>
  
<!--body标签代表网页主体-->
<body>

Hello World!~

</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>基本标签学习</title>
</head>
<body>
    <!--标题标签-->
    <H1>hhh</H1>
    <H2>hhh</H2>
    <H3>hhh</H3>
    <H4>hhh</H4>
    <H5>hhh</H5>
    <H6>hhh</H6>

    <!--水平线标签-->
    <hr/>

    <!--段落标签-->
    <p>你好，</p>
    <p>世界～</p>

    <!--水平线标签-->
    <hr/>

    <!--换行标签-->
    你好 <br/>
    世界 <br/>

    <!--水平线标签-->
    <hr/>

    <!--粗体，斜体-->
    <h1>字体样式标签</h1>

    粗体；<strong>pass the exam</strong>
    斜体：<em>pass the exam</em>
    <br/>

    <hr/>
    <!--特殊符号-->
    空格：&nbsp;&nbsp;&nbsp;&nbsp;空格！<br>
    大于号：&gt;<br/>
    小于号：&lt;<br/>
    版权：&copy;kidjaya <br/>

    <!--特殊符号记忆方法
        &+字母
        记住几个常用就好！
                       -->
</body>
</html>
```



## 图片标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片标签学习</title>
</head>
<body>
    <!--img学习
    src：图片地址
      相对地址（推荐使用）/绝对地址
      ../ 表示上一级目录
    alt 图片名字
    title 悬停文字
    -->
    <a href="4.LinkTag.html#down">
        <img src=".//image/test.jpg" alt="一拳超人" title="悬停文字" width="300" height="300">
    </a>
</body>
</html>
```

## 链接标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>链接标签学习</title>
</head>
<body>

<!--使用name作为标记-->
<a name="top">顶部</a>

<!--a标签
href：必填，表示要跳转到那个页面
_self：在自己的网页中打开
_blank：在新标签中打开
-->
<a href="1.Fist.html" target="_self">点击我跳转到页面一</a>
<a href="https://www.baidu.com/" target="_blank">点击我跳转到页面一</a>
<br/>
<!--图片超链接-->
<a href="2.baseTag.html">
    <img src=".//image/test.jpg" alt="一拳超人" title="悬停文字" width="300" height="300">
</a>

<br>
<!--锚链接
1.需要一个锚标记
2.跳转到标记
实现页面间跳转
-->
<a href="#top">回到顶部</a>
<a name="down">底部</a>

<br>
<!--功能性链接
邮件链接: mailto:
QQ推广链接
-->
<a href="mailto:1725236585@qq.com">点击联系我</a>

<br>

<a target="_blank" href="http://wpa.qq.com/msgrd?v=3&uin=1725236585&site=qq&menu=yes">
    <img border="0" src="http://wpa.qq.com/pa?p=2:1725236585:41" alt="你好！加我！" title="你好！加我！"/>
</a>

</body>
</html>
```

## 列表标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表学习</title>
</head>
<body>
<!--有序列表
应用范围：试卷，问答
-->
<!--ordered lists-->
<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JS</li>
    <li>Jquery</li>
    <li>Java</li>
</ol>
<hr>
<!--无序列表
应用范围：导航，侧边栏
-->
<!--unordered lists-->
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JS</li>
    <li>Jquery</li>
    <li>Java</li>
</ul>
<!--自定义列表
dl：标签
dt：列表名称
dd：列表内容
-->
<!--definition list-->
<dl>
    <dt>技术</dt>

    <dd>HTMl</dd>
    <dd>JS</dd>
    <dd>CSS</dd>
    <dd>前端</dd>

    <dt>位置</dt>

    <dd>广东</dd>
    <dd>北京</dd>
    <dd>上海</dd>
    <dd>重庆</dd>
</dl>
</body>
</html>
```

## 表格标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表格标签</title>
</head>
<body>

<table border="1px">
    <tr>
        <!--colspan 跨列
            table header cel ：th 表头单元格
        -->
        <th colspan="4">1-1</th>
    </tr>

    <tr>
        <!--rowspan 跨列
            table data cell ：td 数据单元
        -->
        <td rowspan="2">2-1</td>
        <td>2-2</td>
        <td>2-3</td>
        <td>2-4</td>
    </tr>

    <!--table row 表行-->
    <tr>
        <td>3-1</td>
        <td>3-2</td>
        <td>3-3</td>
    </tr>

</table>

</body>
</html>
```

## 媒体元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>媒体元素</title>
</head>
<body>

<!--音频和视频
src：资源路径地址
controls：控制条
autoplay：自动播放
-->

<video src="http://vfx.mtime.cn/Video/2019/03/19/mp4/190319222227698228.mp4" controls autoplay></video>
<audio src="https://m10.music.126.net/20200603100627/49025242e91c8aa14b6c28a881129032/yyaac/obj/wonDkMOGw6XDiTHCmMOi/2600780306/8e93/bc84/233a/84939e1464374d08955975b86649fe7b.m4a" controls autoplay></audio>

</body>
</html>
```

## 页面结构分析

![tUdC24.png](https://s1.ax1x.com/2020/06/03/tUdC24.png)

- 一种规范，让别人看得懂你写的代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>页面结构分析</title>
</head>
<body>

<header>
    <h2>网页头部</h2>
</header>

<section>
    <h2>网页主体</h2>
</section>

<footer>
    <h2>网页脚部</h2>
</footer>
</body>
</html>
```

## IFrame内联框架

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>内联框架</title>
</head>
<body>
<!--iframe内联框架
src : 地址
w-h ：宽度高度
-->
<iframe src="" name="hello" frameborder="0" width="1000px" height="800px"></iframe>

<a href="http://kidjaya.xyz/" target="hello">点击跳转</a>

<!--<iframe src="//player.bilibili.com/player.html?aid=55631961&bvid=BV1x4411V75C&cid=97257967&page=11"-->
<!--        scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true">-->
<!--</iframe>-->
</body>
</html>
```

## 表单

### 元素格式



![twMoS1.png](https://s1.ax1x.com/2020/06/04/twMoS1.png)



###  初级验证

> 设置input属性的初级验证

- email
- url
- number



> 设置关键字的提示

- placeholder
- requird
- pattern



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登陆注册</title>
</head>
<body>
<!--表单form
    action：表单提交的位置，可以是网站，也可以是一个请求处理地址
    method：post，get 提交方式
    maxlength:：最长能写几个字符
    size：文本框长度
-->
<form action="1.Fist.html" method="get">
    <!--文本输入框：<input type="text"/>-->
    <p>名字：
        <input type="text" name="username" value="admin"  maxlength="10" size="30" placeholder="请输入用户名">
    </p>
    <!--密码框 <input type="password"/>-->
    <p>密码
        <input type="password" name="password" value="123456" hidden>
    </p>

    <!--单选框标签
        input type="radio"
        value：单选框的值
        name：表示组
    -->
    <p>性别：
        <input type="radio" value="boy" name="gender" checked disabled />男
        <input type="radio" value="girl" name="gender" />女
    </p>
    
    <!--多选框
    input type="checkbox"
    -->
    <p>
        <input type="checkbox" value="sleep" name="hobby"/>睡觉
        <input type="checkbox" value="running" name="hobby"/>运动
        <input type="checkbox" value="coding" name="hobby"/>敲代码
    </p>
    
    <!--按钮
    input type="button" 普通按钮
    input type="image"  图片按钮
    -->
    <p>
        <input type="button" name="btn1" value="点击变长"/>
        <input type="image" src="./image/test.jpg" width="100px" height="100px">
    </p>
    
    <!--下拉框，列表框-->
    <p>国家：
        <select name="country">
            <option value="china">中国</option>
            <option value="America">美国</option>
            <option value="Japan">日本</option>
        </select>
    </p>

    <!--文本域
    cols="30" rows="10"
    -->
    <p>反馈：
        <textarea name="textarea" cols="30" rows="10">文本内容</textarea>
    </p>

    <!--文件域
    input type="file" name="files"
    -->
    <p>
        <input type="file" name="files">
        <input type="button" value="上传" name="upload">
    </p>

    <!--邮件验证-->
    <p>邮箱：
        <input type="email" name="email">
    </p>

    <!--URL-->
    <p>URL：
        <input type="url" name="url">
    </p>

    <!--数字-->
    <p>数字：
        <input type="number" name="num" max="100" min="0" step="10" required>
    </p>

    <!--滑块-->
    <p>音量：
        <input type="range" name="voice" min="0" max="100" step="2"/>
    </p>

    <!--搜索框-->
    <p>搜索框：
        <input type="search" id="search" name="search">
    </p>

    <!--自定义邮箱-->
    <p>自定义邮箱：
        <input type="text" name="define_mail" pattern="^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$">
    </p>

    <!--增强鼠标可用性-->
    <p>
        <label for="search">点我试试</label>
        <input type="text" id="mark">
    </p>

    <!--单选框标签
    提交表单按钮 submit
    重置表单按钮 reset
    -->
    <p>
        <input type="submit">
        <input type="reset">
    </p>
</form>
</body>
</html>
```



## 总结（思维导图）

![t0Ax3T.png](https://s1.ax1x.com/2020/06/04/t0Ax3T.png)