
## 创建对象
方法：声明方式和构造方式，
> 绝大部分内部对象也是使用字面量方式
 
1. null 本身是基本类型，但是
```javascript { .theme-peacock }
typeof null === object
```
2. 函数可以看做可以调用的对象，所以可以像对象一样去操作函数。
3. 数组，比对象的内容更加复杂一些而已
4. 内置对象：
  String Number Boolean Object Function Array Date RegExp Error
  实际上是内置函数，当做构造函数用，用new调用
1. String：
    对比用字面量生成的字符串，用String生成的是一个对象。
```javascript { .theme-peacock }
var name = 'xiaowu'
typeof name
// "string"
var name2 = new String('xiaowu2')
typeof name2
// "object"
```
但是还好，必要时JS会自动把字面量字符串转化为String对象
```javascript { .theme-peacock }
name.length
// 6
```
2. Number类似
3. null、undefind没有构造形式的声明
4. Date只有构造形式
5. Object, Array, Function, RegExp 都是对象，没有字面量形式。
 
## 内容
1. 存在对象内部的内容，只是存了对象的名称，是指向其他内容的指针。
 
### 对象的属性
object.name 
> 只能使用符合标识符标准
 
object['name'] 
> 可以是任何符合标准的字符串，如果不是字符串会调用toString方法转成字符串

``` javascript { .theme-peacock }
a = {}
a[true] = 's'

a[true]
// "s"
b = {}
b.toString()
// "[object Object]"
a[b]

a[b] = 'd'
a
// Object {true: "s", [object Object]: "d"}
```
### 对象的计算属性名
ES6
### 属性的方法

        
