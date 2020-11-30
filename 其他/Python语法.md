# Python语法

> 视频参考：https://www.bilibili.com/video/BV1nt4y1q7R1?t=17（土妹）

---

## 1. 没有`{ }`，全部使用：缩进代替

- If,for
- def method

- example

```python
a = 33
b = 200
if b > a:
  print("b is greater than a")
```

```python
class MyClass:
  x = 5
```

---

## 2.注释

- #
- doc使用```或"""

```python
#这是注释

'''
    第一行要注释的内容
    第二行要注释的内容
    第三行要注释的内容
'''

"""
    第一行要注释的内容
    第二行要注释的内容
    第三行要注释的内容
"""
```

---

## 3.变量类型

- python是动态强类型语言
- 不用声明类型，使用前必须赋值

- str

  - 单引号和双引号没有区别，没有char类型
  - 三引号用于字符串，可以跨行

  ```python
  a = """this is a sentence
  this is a sentence
  this is a sentence
  """
  print(a)
  ```

  - r"123\n"  r"xxx" 可以让里面的内容转义失败
    - 格式化：
      - '%s %s' % ('one','two')
      - '{} {}'.format('one','two')
  - 常用方法：
    - len()
    - split
    - strip
    - ==(比较字符串是否一致，非地址比较)
  - 字符串是list，可直接用list的方法

- list
  - a = [1,2,'ok']
  - len(a)
  - 按索引访问 a[0],a[-1],切片a[2:5] 不包含5
  - append

- tuple
  - 元组
  - 多值返回 b = ('l',l)
  - 也按索引访问 b[0]
  - 可以用_表示不关心这个变量的值
    - (a,_) = b
- 可以拆包
    - a(1,2)
      b,c = a
      b==1,c==2
  
- dict

  - 字典

  ```python
  dict1 = {"summer":"夏天","winter":"冬天"}
  dict1["summer"] = "夏天的风"
  ```
---
## 4.操作符

- In, not in, for in
  - for a in list
- not 没有 ！
- 没有++，只能用+=1

---

## 5.面向对象

- 继承使用()
  - class ClassA(Base):
- 两边两个下划线：系统定义的
  - `__init__`：初始函数
  - `__new__`：构造函数
  - `__del__`：析构函数
  - `__str__`：相当于toString，自动被调用
- 前面两个下划线：私有成员 _private_method
- 前面一个下划线：受保护，不能用from module import导入
- 用self，没有this
- 类里的方法，必须第一个self参数(对象指针),后面跟参数列表
- @classmethod，第一个参数cls（类的指针）和self一样在外面调用时不用传
- 实例化 Animal("cat",12.3)，不用使用new

```python
class Person:
  def __init__(self,fname,lname):
    self.firstname=fname
    self.lastname=lname
  
  def printname(self):
    print(self.firstname,self.lastname)

x = Person("John","Doe")
x.printname()
```

```python
class Student(Person):
  pass
```

---

## 6.风格

- 类名：CamelCase
- 方法名，变量名：class_grade_rank
- 不用加分号

---

## 7.大道理

- 动态强类型语言
- 一切皆对象

---

## 8.工作常用笔记

- python2 print "hello world"
  python3 print("hello world")
- def method_l(a,b)
- 异常捕获：使用except代替catch
  - raise xxxException("error!!")：抛出异常

```python
try:
  pass
except:
  pass
finally:
  pass
```

- mian函数跟java不一样

```python
if __name__ == "__main__":
  pass
```

- 空语句必须不能留用，要加pass
- range(10) 生成0到9的列表
  range(1,10) 生成1到9的列表（都是不包含右边的）
  for x in range(1,10)

- if
  elif
  else
- 没有null，用None