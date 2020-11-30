# JavaScript

> 视频参考 https://www.bilibili.com/video/BV1JJ41177di?p=30

## 1.什么是JavaScript

### 1.1.概述

> JavaScript是一门最流行的脚本语言

**一个合格的后端程序员必须精通javaScript**

### 1.2.历史

https://blog.csdn.net/kese7952/article/details/79357868

- ECMAScript它可以理解为javaScript的一个标准

- 最新版本已经到ES6

- 但是大部分浏览器还只停留在es5版本上

- 产生问题：开发环境和线上环境的版本不一致

- 产生工具：webpack使开发环境转换为线上环境

  

## 2.快速入门

### 2.1.引入JavaScript

1. 内部标签

```html
<script>
//....
</script>
```

2. 外部引入

test.js

```javascript
//todo
```

test.html

```html
<script src="test.js"></script>
```

测试代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
<!--    <script>-->
<!--        alert("hello world")-->
<!--    </script>-->

    <!--外部引入-->
    <!--注意 script标签必须成对出现-->
    <script src="js/test.js"></script>

    <!--不用显示定义，默认就是-->
    <script type="text/javascript"> </script>

</head>
<body>

<!--这里也可以定义script-->
</body>
</html>
```



### 2.2.基础语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--JavaScript变量严格区分大小写-->
    <script>
        //定义变量，变量类型 变量名 = 变量值；
        let score = 71;
        //条件控制
        if (score>60 && score<70){
            alert("60~70");
        }else if(score>70 && score<80){
            alert("70~80");
        }else{
            alert("other");
        }

        //console.log输出控制台

    </script>
</head>
<body>

</body>
</html>
```

- 浏览器调试必备

![tcp05n.png](https://s1.ax1x.com/2020/06/06/tcp05n.png)



### 2.3.数据类型

> 数值，文本，图形，音频，视频...

- 变量

```javascript
var 不要以数字开头 可以用中文命名
var 王者荣耀 = "倔强青铜";
```



- number
  - js不区分小数和整数 Number

```javascript
123//整数
123.1//浮点数
1.121e3//科学计数法
-899//负数
NaN// Not a number
Infinity//表示无限大
```

- 字符串

```javascript
'abc'
"abc"
```

- 布尔值

```javascript
true
false
```

- 逻辑运算

```javaS
&& 与 两个都为真，结果为真 
|| 或 一个为真，结果为真
!  非 真即假，假即真
```

- 比较运算符 **！！！重要，不规范使用会出异常**

```javascript
=
==  等于 类型不一样，值一样，判断为true
=== 绝对等于 类型一样，值一样，判断为true 
```

**这是js的一个缺陷**，坚持不要使用==比较

- 须知
  - NaN===NaN，这个值与所有数值都不相等，包括自己
  - 只能通过isNaN(NaN) 来判断这个数是否是NaN
- 浮点数问题

```javascript
console.log((1/3)===(1-2/3));//false
```

**尽量避免使用浮点数进行运算，存在精度问题**

```javascript
Math.abs(1/3-(1-2/3))<0.000000001)
```

- null 和 undefined
  - null 空
  - undefined 未定义
- 数组
  - Java数组必须是一系列相同类型的对象，JS不需要

```javascript
			//保证数组的可读性 尽量使用[]
        var arr = ['a',1,2,true,null,'sss'];
        var arr2 = new Array(1,2,3,'a','c',null,true);
```

- 取数组下标：如果越界了，不会报异常，会报

```javascript
> undefined
```



- 对象
  - 对象是大括号，数组是中括号

> 每个属性之间用使用逗号隔开，最后一个不需要添加

```javascript
var person = {
            name:"kidjaya",
            age:22,
            skill:[
                'js',
                'php',
                'java',
                'html'
            ]
        }
```

- 取对象的值

```javascript
console.log(person.age);
> 22
console.log(person.skill[0]);
> js
```



### 2.4.严格检查模式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--
    前提：IDEA需要设置支持ES6语法
    'use strict'；严格检查模式，预防javaScript的随意性导致的一些问题
    必须写在JavaScript的第一行！
    局部变量都建议使用let去定义
    -->
    <script>
        'use strict'
        let i = 1;
    </script>
</head>
<body>

</body>
</html>
```



## 3.数据类型

### 3.1、字符串

1. 正常字符串使用单引号或双引号包裹
2. 注意字符串

```javascript
\'
\n
\t
\u4e2d \u#### Unicode字符
\x41	 \x## 	Ascll字符
```

3. 多行字符编写

```javascript
``//Tab上面的符号
var msg =
            `你好
        四大空间
        四大皆空
        `;
```

4. 模版字符串

 ```javascript
``//Tab上面的符号
let name = "kidjaya";
let string = "nihao";
let msg2 = `你好，${name}`;
console.log(msg2);
 ```

5. 字符串长度

```javascript
str.length
```

6. 字符串的可变性：**不可变**

![tgFBMq.png](https://s1.ax1x.com/2020/06/07/tgFBMq.png)

7. 大小写转换

```javascript
//这里是方法
str.toUpperCase();
str.toLowerCase();
```

8. 获取字符index起始

```javascript
student.indexOf('test');
```

9. substring 截取字符串

```javascript
[)
str.substring(1);//起始（包含）开始截取
str.substring(1,3);//[1,3)
```



### 3.2、数组

Array可以包含任意的数据类型

```javascript
var arr = [1,2,3,4];//通过下标取值和赋值，数值可变！
arr[0]//取值
arr[0] = 1//赋值
```

1. 长度

```javascript
arr.length;//获取数值长度
/*
注意：
假如给arr.length赋值，数组大小就会发生变化，如果赋值过小，元素就会丢失
*/
```

2. indexOf，通过元素获得下标索引

```javascript
arr.indexOf(1);//2
/*
注意：字符串"1"和数字1是不同的
*/
```

3. slice() 截取数组arr的一部分，返回新数组，参数类似于String的substring
4. push(),pop() 尾部

 ```javascript
push: 将元素压入尾部
pop: 弹出尾部元素，并获取值
 ```

5. unshift(),shift() 头部

```javascript
unshift: 将元素压入头部
shift: 弹出头部一个元素
```

6. sort() 排序

```javascript
arr
> (3) ["A", "C", "D"]
arr.push('B')
> 4
arr.sort()
> (4) ["A", "B", "C", "D"]
```

7. reverse() 反转

```javascript
arr.reverse()
> (4) ["D", "C", "B", "A"]
```

8. concat()

```javascript
arr.concat([1,2,3])
> (7) ["D", "C", "B", "A", 1, 2, 3]
arr
> (4) ["D", "C", "B", "A"]
```

注意：concat()并没有修改数组，只是会返回一个新的数组

9. 连接符join

- 打印拼接数组，用特定的字符串连接

```javascript
arr
> (4) ["D", "C", "B", "A"]
arr.join('-')
> "D-C-B-A"
```

10. 多维数组

```javascript
var arr = [[11,22],[33,44],['a','b']]
> undefined
arr[1][2]
> undefined
arr[2][1]
> "b"
```



数组：存储数据（如何存，如何取，方法都可以自己实现）



### 3.3、对象

若干个键值对

```javascript
var 对象名 = {
  属性名 ：属性值，
  属性名 ：属性值，
  属性名 ：属性值
}

//定义一个person对象，他有四个属性
var person = {
  name:"kidjaya",
  age:22,
  email:"1725236585@qq.com",
  score:60
}
```

- js中对象，{...}表示一个对象，键值对描述属性 xxxx:xxxx，多个属性之间用逗号隔开，最后一个属性不加逗号。
- JavaScript中的所有key都是字符串，value可以是任意对象

1. 对象赋值

```javascript
person.skill='java'
>"java"
person
>{name: "kidjaya", age: 22, email: "1725236585@qq.com", score: 60, skill: "java"}
person.skill
>"java"
```

2. 使用一个不存在的对象属性，不会报错! Undefined

```javascript
person.sss
>undefined
```

3. 动态删减属性,通过delete删除对象的属性

```javascript
delete person.name
>true
person
>{age: 22, email: "1725236585@qq.com", score: 60, skill: "java"}
```

4. 动态添加,直接给新的属性添加值即可

```javascript
person.sss = 1
>1
person
>{age: 22, email: "1725236585@qq.com", score: 60, skill: "java", sss: 1}
```

5. 判断属性值是否在这个对象中！xxx in xxx

```javascript
'age' in person
>true
//继承，判断这个方法是否存在
'toString' in person
>true
```

6. 判断一个属性是否是这个对象自身拥有的

```javascript
person.hasOwnProperty('age')
>true
person.hasOwnProperty('toString')
>false
```



### 3.4、流程控制

1. If判断

```javascript
let age = 3;
if(age > 3){
  alert(1);
}else if (age <3){
  alert(2);
}else{
  alert(3);
}
```

2. while循环 避免死循环

```javascript
while(age < 100){
  age++;
  console.log(age);
}

do {
  age++;
  console.log(age);
}while (age < 1000);
```

3. for循环

```javascript
 for (let i = 0; i < 100; i++) {
   console.log(i);
 }
```

4. forEach循环

```javascript
let age = [1,2,3,4,5,6];
//函数
age.forEach(function (value) {
  console.log(value);
})
```

5. for...in 

```javascript
//for(var index in object){}
for (let num in age){
  if (age.hasOwnProperty(num)){
    console.log("exist");
    console.log(age[num]);
  }
}
```



### 3.5、Map和Set

> ES6新特性

- Map

```javascript
//ES6 MAP
//学生的成绩，学生的名字
//var names = ["aa","bb","cc"];
//var scores = [100,10,60];

let map = new Map([['aa',100],['bb',10],['cc',60]]);
let name = map.get('aa');//通过key获得value

map.set('admin',123456);//新增或修改
map.delete('aa');//删除
```

- Set 无序不重复的集合

```javascript
let set = new Set([1,2,3,4,3,3,4,4]);//set可以去重
set.add(666);//添加
set.delete(4);//删除
console.log(set.has(2));//是否包含某个元素
```



### 3.6、iterator

> ES6新特性

遍历数组：

```javascript
//通过for of 获取值 ---- for in 获取下标
let arr = [3,4,5];
for (let x of arr){
  console.log(x)
}
```

遍历map:

```javascript
let map = new Map([['tom',99],['ken',88],['yang',77]]);
for (let x of map){
  console.log(x)
}
```

 遍历set:

```javascript
let set = new Set([1,2,2,2,2,3,3,3,3,4,4,4,4])
for (let y of set){
  console.log(y)
}
```



## 4. 函数

### 4.1、定义函数

> 定义方式一

绝对值函数

```javascript
function abs(x) {
  if (x>0){
    return x;
  }else{
    return -x;//javaScript会默认给每行最后输出; 如果没有的话
  }
}
```

一旦执行到return代表函数结束，返回结果

如果没有执行return，函数执行完也会返回结果，结果就是undefined

> 定义方式二

```javascript
var abs = function(x){
  if (x>0){
    return x;
  }else{
    return -x;//javaScript会默认给每行最后输出; 如果没有的话
  }
}
```

function(x){......}这是一个匿名函数。但是可以把结果赋值给abs，通过abs就可以调用函数！

**方式一和方式二等价**

> 调用函数

```javascript
abs(10)//10
abs(-10)//10
```

- 参数问题：javaScript可以传递任意个参数，也可以不传递参数
- 参数进来是否存在的问题，假设不存在，如何规避

```javascript
var abs = function(x){
  if (typeof x!== 'number'){
    throw 'Not a Number';
  }
  if (x>0){
    return x;
  }else{
    return -x;//javaScript会默认给每行最后输出; 如果没有的话
  }
}
```

> arguments

**arguments**是一个js免费赠送的关键字；

代表，传递进来的所有参数，是一个数组！

```javascript
var abs = function(x){
  if (typeof x!== 'number'){
    throw 'Not a Number';
  }
  console.log("x=>"+x);

  for (let i = 0; i < arguments.length; i++) {
    console.log(arguments[i])
  }

  if (x>0){
    return x;
  }else{
    return -x;//javaScript会默认给每行最后输出; 如果没有的话
  }
}
```

问题：arguments包含所有的参数，我们有时候想使用多余的参数来进行附加操作。需要排出已有的参数

以前：

```javascript
if(arguments.length>2){
  for(var i = 2; i < aruments.length;i++){
    //todo
  }
}
```

ES6引入的新特性，获取除了已经定义的参数之外的所有参数

```javascript
function aaa(a,b,...reset){
  console.log("a=>"+a);
  console.log("b=>"+b);
  console.log(reset);
}
```

reset参数只能写在最后面，必须使用...



### 4.2、变量的作用域

- 在javaScript中，var定义变量实际是有作用域的。

- 假设在函数体中声明，则在函数体外不可以使用（非要实现的话，使用闭包）

```javascript
function test(){
  var x = 1;
  x = x+1;
}
x = x+2//Uncaught ReferenceError: x is not defined
```

- 如果两个函数用了相同的变量名，只要在函数内部，就不冲突

```javascript
function test(){
  var x = 1
  x = x+1
}

function test2(){
  var x = 'A'
  x = x+2
}
```

- 内部函数可以反问外部函数的成员，反之则不行

```javascript
function test(){
  var x = 1
  //内部函数可以访问外部函数的成员，反之则不行
  function test2(){
    var y = x + 1
    console.log(y);//2
  } 
  test2()
  var z = y + 1
 	console.log(z);//Uncaught ReferenceError: y is not defined
}
```

- 假设，内部函数变量和外部函数的变量，重名

```javascript
function test(){
  var x = 1;
  function test2(){
    var x = "A"
    console.log("inner"+x);//inner
  }
  test2()
  console.log("outer"+x);//outer
}
test()
```

- 假设在JavaScript中函数查找变量从自身函数开始，由内向外查找，假设外部存在这个同名的函数变量，则内部函数会屏蔽外部函数的变量（就近原则）

> 提升变量的作用域

```javascript
function test(){
  var x = "x"+y;
  console.log(x);
  var y = 'y';
}
//结果：xundefined
```

- 说明：js执行引擎，自动提升了y的声明，但是不会提升变量y的赋值

```javascript
function test2(){
  var y;
  
  var x = "x"+u;
  console,log(x);
  y = "y";
}
```

这个是在JavaScript建立之初就存在的特性。养成规范：所有的变量定义都放在函数头部（JS），不要乱放，便于代码维护

```javascript
function test2(){
  var x = 1,
      y = x + 1,
      z,i,a;//后期使用，值为undefined
}
```

> 全局函数

```javascript
//全局变量
x = 1;

function f(){
  console.log(x)
}
f()
console.log(x)
```

- 全局对象window

```javascript
var x = 'xxx'
alert(x)
alert(window.x);//默认所有的全局变量，都会自动绑定在window对象下
```

- alert()这个函数本身也是一个window变量

```javascript
var x = 'xxx'
window.alert(x)

var old_alert = window.alert
//old_alert(x)

window.alert = function(){}
//发现alert失效了
window.alert(x+"old");
//恢复
window.alert = old_alert
window.alert(x+"new");
```

- JavaScript实际只有一个全局作用域，任何变量（函数也可以视为变量），假设没有在函数作用范围内找到，就会向外查找，如果在全局作用域都没有找到，就会报错  ReferenceError

> 规范

- 由于我们所有的全局变量都会绑定到我们的window上。如果不同的js文件，使用了相同的全局变量，就会发生冲突，出现奇怪错误

```javascript
//唯一全局变量
var DefinedScrope = {};

//定义全局变量
DefinedScrope.name = 'kidjaya';
DefinedScrope.add = function(x,y){
  	return x*y;
}
```

- 把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突问题
- JQuery使用这种设计

> 局部作用域let

```javascript
function test(){
  for(var i = 0;i< 100; i++){
    console.log(i)
  }
  console.log(i+1);// i出了作用域还可以使用
}
```

- ES6 let关键字，解决局部作用域冲突问题！

```javascript
function test(){
  for(let i = 0;i< 100; i++){
    console.log(i)
  }
  console.log(i+1);// Uncaught ReferenceError: i is not defined
}
```

- 建议使用let去定义局部作用域

> 常量 const

- 在ES6之前，怎么定义常量：只有用全部大写字母命名的变量就是常量

```javascript
var PI = '3.14'

console.log(PI)
PI = '31231'//可以改变，比较恶心
console.log(PI)
```

- 在ES6引入了关键字const

```javascript
const PI = '3.14'//只读变量
console.log(PI)
PI = 'a1sd23'//Uncaught TypeError: Assignment to constant variable.
console.log(PI)
```



### 4.3、方法

> 定义方法

- 方法就是把函数放在对象的里面，对象只有两个东西：属性和方法

```javascript
var test = {
  name:"coding",
  birth:2000,
  //方法
  age:function(){
    //今年-出生的年
    var now = new Date().getFullYear()
    return now - this.birth
  }
}

//属性
test.name
//方法
test.age()
```

- this是无法指向的，是默认指向调用它的那个对象

> apply

- 在js中可以控制this的指向

```javascript
function getAge(){
  //now - brith
  var now = new Date().getFullYear();
  return now - this.birth;
}

var person = {
  name:'yang',
  birth:1998
  age:getAge
}


var person2 = {
  name:'kidjaya',
  birth:1997
  age:getAge
}

//person.age()

getAge.apply(person,[])//this，指向了person，参数为空
getAge.apply(person2,[])//this，指向了person2，参数为空
```



## 5.内置对象

> 标准对象

```javascript
typeof 123
"number"
typeof '123'
"string"
typeof true
"boolean"
typeof NaN
"number"
typeof []
"object"
typeof {}
"object"
typeof Math.abs
"function"
typeof undefined
"undefined"
let id = Symbol('id');
typeof id
"symbol"
```



### 5.1、Date

- 基本使用

```javascript
let now = new Date();
now.getFullYear();//年
now.getMonth();//月 0～11
now.getDate();//日
now.getDay();//星期
now.getHours();//时
now.getMinutes();//分
now.getSeconds();//秒
now.getTime();//时间戳

console.log(new Date(now.getTime()));//时间戳转换为时间
```

- 转换

```javascript
now = new Date()
Wed Jun 10 2020 15:13:30 GMT+0800 (中国标准时间)
now.toLocaleString//是一个方法
ƒ toLocaleString() { [native code] }
now.toLocaleString()
"2020/6/10 下午3:13:30"
now.toGMTString()
"Wed, 10 Jun 2020 07:13:30 GMT"
```



### 5.2、json

> json是什么，扩展bson

- 早期，所有数据传输习惯使用XML

- 在JavaScript一切皆为对象，任何js支持的类型都可以用JSON来表示
  - 格式：
    - 对象都用{}
    - 数组都用[]
    - 所有的键值对都是用 key:value

- JSON字符串和JS对象的转化

```javascript
let user = {
  name:"kidjaya",
  age:3,
  sex:'男'
}

//对象转化为json字符串
let jsonUser = JSON.stringify(user);

//json字符串转换为对象 参数为json字符串
let obj = JSON.parse(jsonUser);
```

- JSON和JS对象的区别

```java
json: "{"name":"kidjaya","age":3,"sex":"男"}"
js对象：{name: "kidjaya", age: 3, sex: "男"}
```



### 5.3、Ajax

- 原生js写法 xhr异步请求
- JQuery封装好的方法 #("#name").ajax("")
- axios请求



## 6.面向对象编程

### 6.1、什么是面向对象

>  javaScript、java、C#...面向对象，不过JavaScript有些区别！

- 类：模版

- 对象：具体实例

在JavaScript这个需要换一下思维方式！

```javascript
let Student = {
  name:"kidjaya",
  age:3,
  run:function(){
    console.log(this.name+"run!!!")
  }
};

let Teacher = {
  name:"teacher"
};

//原型对象
Teacher.__proto__ = Student;
Teacher.run();

let Bird = {
  name:"bird",
  fly:function () {
    console.log(this.name+"fly!!!")
  }
};

//原型对象
Teacher.__proto__ = Bird;
Teacher.fly();
```

```javascript
function Student(name) {
            this.name = name;
        }

        // 给Student新增一个方法
        Student.prototype.hello = function () {
            alert('Hello');
        }
```



> class对象

- class关键字，是在ES6引入

1. 定义一个类，属性，方法

```javascript
//ES6之后====================
//定义一个学生的类
class Student{
  constructor(name) {
    this.name = name;
  }

  hello(){
    alert("Hello")
  }

}
```

2. 继承

```javascript
//ES6之后====================
//定义一个学生的类
class Student{
  constructor(name) {
    this.name = name;
  }

  hello(){
    alert("Hello")
  }

}

class Pupil extends Student{
  constructor(name,grade) {
    super(name);
    this.grade = grade;
  }

  myGrade(){
    alert('我是一名小学生');
  }
}

let xiaowang = new Student("xiaowang");
let xiaohong = new Pupil("xiaohong",90);
```

- 本质：查看原型对象

![tTZOWd.png](https://s1.ax1x.com/2020/06/10/tTZOWd.png)

> 原型链

__ proto__

 ![tTeiFg.png](https://s1.ax1x.com/2020/06/10/tTeiFg.png)



## 7.操作BOM对象（重点）

> 浏览器介绍

- JavaScript和浏览器的关系

  - JavaScript诞生就是为了让他在浏览器中运行

- BOM：浏览器对象模型(内核)

  - IE 6～11
  - Chrome
  - Safari
  - FireFox
  - Opera

  

- 第三方浏览器

  - QQ浏览器
  - 360浏览器



> window

window代表 浏览器窗口

```javascript
window.alert(1)
undefined
window.innerHeight
1307
window.outerHeight
1024
window.innerWidth
980
window.outerWidth
768
```



> Navigator

Navigator,封装了浏览器的信息

```javascript
navigator.appCodeName
"Mozilla"
navigator.appVersion
"5.0 (iPad; CPU OS 11_0 like Mac OS X) AppleWebKit/604.1.34 (KHTML, like Gecko) Version/11.0 Mobile/15A5341f Safari/604.1"
navigator.userAgent
"Mozilla/5.0 (iPad; CPU OS 11_0 like Mac OS X) AppleWebKit/604.1.34 (KHTML, like Gecko) Version/11.0 Mobile/15A5341f Safari/604.1"
navigator.platform
"MacIntel"
```

大多数时候，我们不会使用**navigator**对象，因为会被人为修改

不建议使用这些属性来判断和编写代码



> screen

- 代表屏幕尺寸

```javascript
screen.width
768px
screen.height
1024px
```



> location*

- 代表当前页面的URL信息

```javascript
host: "www.baidu.com"
hostname: "www.baidu.com"
href: "https://www.baidu.com/"
origin: "https://www.baidu.com"
reload:f reload() //刷新网页
//设置新地址
location.assign('http://www.kidjaya.xyz')
```



> document

- 代表当前的页面，HTML DOM文档树

```javascript
document.title = "yang"
"yang"
```

- 获取具体的文档树节点

```html
<dl id="app">
  <dt>Language</dt>
  <dd>Java</dd>
  <dd>PHP</dd>
</dl>

<script>
  let dl = document.getElementById("app");
</script>
```

- 获取cookie

```javascript
document.cookie
"BIDUPSID=D4AFCEC236BE28864E925D05371BA8AB; PSTM=1589280302; BAIDUID=D4AFCEC236BE2886894ADFA90B06164A:FG=1; BD_UPN=123253; MCITY=-340%3A; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BD_HOME=1; sug=0; sugstore=0; ORIGIN=0; bdime=0; BDSVRTM=78; H_PS_PSSID="
```

- 劫持cookie原理

```html
<script>
	//获取cookie
</script>
//恶意人员：获取的你cookie上传到他的服务器
```

服务器端可以设置 cookie：httpOnly 来保证cookie的安全性

> history

- 代表浏览器的历史记录

```javascript
history.back();//后退
history.forward();//前进
```



## 8.操作DOM对象*

> 核心

浏览器网页就是一个Dom树形结构！

- 更新：更新DOM节点
- 遍历DOM节点：得到DOM节点
- 删除：删除一个DOM节点
- 添加：添加一个新的节点

要操作一个Dom节点，就必须先获得这个Dom节点

> 获得Dom节点

```html
<div id="father">
  <h1>title</h1>
  <p id="p1">这是使用id</p>
  <p class="p2">这是使用class</p>
</div>

<script>
  //对应的css选择器
  let h1 = document.getElementsByTagName("h1");
  let p1 = document.getElementById("p1");
  let p2 = document.getElementsByClassName("p2");
  let father = document.getElementById("father");

  let children = father.children;//获取父节点下的所有子节点
  let firstChildren = father.firstChild;//获取第一个子节点
  let endChildren = father.lastChild;//获取第二个子节点
</script>
```

- 以上是原生代码，比较长！

```html
<div id="id1">
</div>

<script>
  let id1 = document.getElementById("id1");
</script>
```

> 更新节点

- **Id1.innerText="456" **修改文本的值
- **id1.innerHTML = '<h1>123</h1>'** 可以解析HTML文本标签

操作css

```javascript
id1.style.color = 'red'
id1.style.fontSize = '30px'
id1.style.padding = '2em'
```



> 删除节点

删除节点步骤：

1. 先获取父节点
2. 通过父节点删除自己

```html
<div id="id1">
  <h1>title</h1>
  <p id = "p1">p1</p>
  <p class = "p2">p2</p>
</div>

<script>
  let self = document.getElementById("p1");
  let father = self.parentElement;
  father.removeChild(self);

  //删除是一个动态的过程：
  father.removeChild(father.children[0]);
  father.removeChild(father.children[1]);
  father.removeChild(father.children[2]);
</script>
```

- 注意：删除多个节点的时候，children是在时刻变化的，删除节点的时候一定要注意！

> 插入节点

- 我们获得了某个dom节点，假设这个dom节点是空的，我们通过innerHTML就可以增加一个元素了，但是这个DOM节点已经存在元素了，我们就不能这么干了，会覆盖原来的节点

![tT0qAS.png](https://s1.ax1x.com/2020/06/10/tT0qAS.png)

> 创建一个新标签，实现插入

```html
<script>
        let js = document.getElementById('js');//已存在的节点
        let list = document.getElementById('list');
        //通过js 创建一个新节点
        let newP = document.createElement('p');//创建一个p标签
        newP.id = 'newP';
        //newP.setAttribute('id','newP');等价上面
        newP.innerText = "hello JavaScript";
        // list.appendChild(js);

        // 创建一个标签节点
        let myScript = document.createElement('script');
        myScript.setAttribute('type','text/javascript');

        //修改元素style
        let body = document.getElementsByTagName('body')[0];
        body.style.background = '#ff983b';
    </script>
```



> InsertBefore

```javascript
let j = document.getElementById('j');
let rp = document.getElementById('rp');
let js = document.getElementById('js');
let list = document.getElementById('list');
//要包含的节点.insertBefore(newNode,targetNode)
list.insertBefore(js,j);
list.replaceChild(rp,js);
```



## 9.操作表单

> 表单是什么form DOM树

- 文本框 text
- 下拉框 <select>
- 单选框 radio
- 多选框 checkbox
- 隐藏框 hidden
- 密码框 password
- ...

表单的目的：提交信息

> 获得要提交的信息

```html
<form action="post">
    <p>
        <span>用户名：
            <input type="text" id="username">
        </span>
    </p>

    <!--单选框，就是定义好的value-->
    <p>
        <span>性别：</span>
        <input type="radio" name="sex" value="man" id="boy">男
        <input type="radio" name="sex" value="woman" id="girl">女
    </p>
</form>

<script>
    let input_text = document.getElementById("username");
    let boy_radio = document.getElementById("boy");
    let girl_radio = document.getElementById("girl");

    //得到输入的值
    let value = input_text.value;
    //修改输入的值
    input_text.value = "target_value";

    //对于单选框，多选框等固定的值，value方法只能获取到当前节点的值
    boy_radio.checked;//查看返回结果，是否为true，如果为true，则被选中
    girl_radio.checked;
</script>
```



> 提交表单

 ```html
    <!--
    表单绑定提交事件
    onsubmit=绑定一个提交检测的函数，true,false
    将这个结果返回给表单，使用onsubmit接收

    !!注意取的方法名，不要是内置的，不然会出现未知错误
    -->
    <form action="https://www.baidu.com/" method="post" onsubmit="return test()">
        <p>
            <span>用户名：
             <input type="text" id="username" name="username" required>
            </span>
        </p>

        <p>
            <span>密码：
              <input type="password" id="password" required>
            </span>
        </p>
        <input type="hidden" id="md5" name="password">

        <!--绑定事件-->
        <button type = "submit">提交</button>
    </form>

    <script>
        function test() {
            alert(1);
            let username = document.getElementById("username");
            let pwd = document.getElementById("password");
            let md5 = document.getElementById("md5");
            md5.value = hex_md5(pwd.value);
            //true就是通过提交，false就是阻止提交
            return true;
        }

    </script>
 ```



## 10.JQuery

JavaScript

**JQuery库，里面存在大量的JavaScript函数**

文档大全：https://jquery.cuishifeng.cn/index.html

> 获取JQuery

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
</head>
<body>
<a href="#" id="test_jquery">click me</a>
<script>
    // var id =document.getElementById("test_jquery");
    // id.click();

    //选择器就是css的选择器
    $("#test_jquery").click(function () {
        alert('hello')
    })
</script>
</body>
</html>
```



> 选择器

```javascript
//原生js，选择器少，麻烦不好记
//标签
document.getElementsByTagName();
//id
document.getElementById();
//类
document.getElementsByClassName();

//JQuery css 选择器他都能用
$('p').click();//标签选择器
$('#id1').click();//id选择器
$('.class').click();//class选择器
```



> 事件

鼠标事件，键盘事件，其他事件。

![t7g8AI.png](https://s1.ax1x.com/2020/06/11/t7g8AI.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <style>
        #divMove{
            width: 500px;
            height: 500px;
            border: 1px solid black;
        }
    </style>
</head>
<body>
<!--获取鼠标当前的一个坐标-->
mouse:<span id="mouseMove"></span>
<div id="divMove">
    在这里移动试试
</div>
<script>
    //当网页加载完毕后，响应事件
    // $(document).ready(function(){
    //     //todo
    // })

    //上面简写
    $(function () {
        $('#divMove').mousemove(function (e) {
            $('#mouseMove').text('x:'+e.pageX+',y:'+e.pageY)
        })
    })
</script>
</body>
</html>
```



> 操作dom

- 节点文本操作

```javascript
$('#test_ul li[name=python]').text();//属性选择器，获得值
$('#test_ul li[name=python]').text('PHP');//属性选择器，设置值
$('#test_ul').html();//属性选择器，获得值
$('#test_ul').html('<strong>123</strong>');//属性选择器，设置值
```

- css的操作

```javascript
$('#test_ul li[name=python]').css("color","red")
```

+ 元素的显示和消失:**本质 display:none**

```javascript
$('#test_ul li[name=python]').hide();
$('#test_ul li[name=python]').show();
```

- 测试

```javascript
$(window).width()
980
$(window).height()
1306
```

- **Ajax()**

```javascript
$.ajax({ url: "test.html", context: document.body, success: function(){
    $(this).addClass("done");
}});
```



> 小技巧

1. 如何巩固JS
   - 看JQuery框架源码
   - 看游戏源码
2. 巩固HTML、CSS（扒网站，全部download下来，然后修改看效果）

- 前端常用组件
  - LayUI弹窗组件
  - **Element组件**
  - Bootstrap
  - Ant Design



//todo 1.写一个组件 2.扒一个网站，吃透和修改样式