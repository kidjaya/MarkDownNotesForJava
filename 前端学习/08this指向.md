# this指向

## 回顾：值的类型

- 变量是保存在**栈空间**的
  - 基本数据类型的赋值在栈空间
  - 引用数据类型的赋值是一串内存的**逻辑地址**，指向**堆空间**开辟的对象
- 对象是保存在**堆空间**的
  - 如果两个变量保存了相同的引用，一个修改另一个也会跟着改变



## this

> 解析器在调用函数时每次都会向函数内部传递一个隐式的参数，这个隐含的参数就是this，this指向的是一个对象，这个对象我们称为函数执行时的**上下文对象**

- 根据函数调用方法的不同，this会指向不同的对象（重点）
  1. 以函数的形式调用时，this永远都是window，比如`say()`,相当于`window.say()`
  2. 以方法的形式调用时，this是调用方法的那个对象
  3. 以构造函数的形式调用时，this是创建的那个对象
  4. 使用call和apply调用时，this是指定的那个对象

### 例子1

```javascript
function sayName(){
  console.log(this);
  console.log(this.name);
}
var obj1 = {
  name:"kidjaya",
  sayHello: sayName
};
var obj2 = {
  name:"yang",
  sayHello: sayName
}

var name = "我在window我是name";

sayName();
```

- 当以函数的形式调用时，this指向的是window，所以`this.name == window.name`

### 例子2

```javascript
function sayName(){
  console.log(this);
  console.log(this.name);
}
var obj1 = {
  name:"kidjaya",
  sayHello: sayName
};
var obj2 = {
  name:"yang",
  sayHello: sayName
}

var name = "我在window我是name";

obj2.sayHello();
```

- 当以方法的形式调用，this指向的是调用那个方法的对象，`this.name == obj2.name `

### 箭头函数中this的指向

- ES6中的箭头函数并会向使用上面四条标准来绑定规则，而是会继承外层函数调用的this绑定（无论this绑定到什么）



### 改变this的指向

- call/apply：通过这连个方法可以使得在调用函数的时候修改this的指向

```javascript
fn();
fn.call(obj);
student.like.call(obj,"a","b","c");
fn.apply(obj);
student.like.call(obj,["a","b","c"]);
```

- bind改变地址的指向

```javascript
var teacher = {
  name:"老师"
}
var student = {
  name:"学生",
  like:function(){
    console.log(this);
    console.log(this.name);
  }.bind(teacher)
}

var fn = function(){
  console.log(this);
  console.log(this.name+"喜欢打羽毛球");
}.bind(student);

fn();
student.like();
```



## 类数组arguments

- 调用函数时，浏览器每次都会传递进两个隐含的参数

  - 函数的上下文对象 this
  - 封装实参的对象arguments

  - 例如

    ```javascript
    function fn(){
      console.log(arguments);
      console.log(typeof arguments);
    }
    fn();
    ```

- arguments是一个类数组对象，它可以通过索引来操作数据，也可以获取长度
- arguments代表的是实际参数，在调用函数时，我们所传递的实际参数都会在arguments中保存，arguments只在函数中使用



### 1.返回函数的实际参数的个数：arguments.length

- arguments.length可以用来返回实际参数的长度

```javascript
fn(1,2,);
fn(4,5,6);
fn(4,5,6,7,8);
function fn(a,b){
  console.log(arguments);
  console.log(fn.length);
  console.log(arguments.length);
}
```

- 我们即使不定义形式参数，可以使用arguments来使用实际参数，只是比较麻烦。
  - arguments[0]代表第一个参数，arugments[1]代表第二个参数，以此类推



### 2.返回赈灾正在执行的函数：arguments.callee

- arguments里面有一个属性叫callee，这个属性是一个函数对象，就是当前正在使用的函数对象

```javascript
function func(){
  console.log(arguments.callee == func);
}

func();
```

- 在使用函数递**归调**用时，推荐使用arguments.callee代替函数名本身



### 3.arguments可以修改元素

- 之所以说arguments是伪数组，是因为：arguments**可以修改元素**，但是不能改变数组长度

```javascript
fn(2,4);
fn(1,3,5);

function fn(a,b){
  arugments[0] = 100;
  for(var i = 0;i<=arguments.length;i++){
    console.log(arguments[i])
  }
}
```

