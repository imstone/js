# 原型

## 1. 即
> 原型看似平时工作中不会用到，其实我们工作中一直在用原型，只是大家不知道而已。
#### 工作中用到的原型
 

我们看下面的代码，当我们定义了两个函数并用console.log打印一些信息时，其实job变量没有在函数内声明，而是声明在函数外部。但是通过作用域概念，函数是可以拿到函数外部的变量的。这样我们就可以把一些公共变量声明在2个函数共同的外部，达到声明一个变量，给两个函数使用的目的。**这个是作用域的强大之处**。

``` javascript { .theme-peacock }
var job = '前端';
function xiaowu() {
	var team = '国际化';
	console.log(team + '的' +  job);
}
function stone() {
	var team = '商业安全'; 
	console.log(team + '的' +  job)
}
xiaowu() // 国际化的前端
stone() // 商业安全的前端
```

我们把上面的代码用对象的方式再写一遍，我们发现，虽然整体代码结构清晰很多，但是出现了大量重复代码。比如job没法公用了，并且每个对象出现了一个getInfo去打印信息。我们第一反映肯定是，这个肯定需要优化。

``` javascript { .theme-peacock }
var xiaowu = {
	team: '国际化',
	job: '前端',
	getInfo: function(){
		console.log(this.team + '的' + this.job)
	}
}
var stone = {
	team: '商业安全',
	job: '前端',
	getInfo: function(){
		console.log(this.team + '的' + this.job)
	}
}

xiaowu.getInfo() // 国际化的前端
stone.getInfo() // 商业安全的前端
```

对象是不是也有类似作用域的机制，可以把公用部分单独提出去呢？
> 虽然JS是Brendan Eich 用了仅仅十天就发明的语言，但是还是有这个机制的。

所有的对象都有一个内部属性[[prototype]] (原型 [ˈproʊtətaɪp])，当对象去查找自己的属性或者方法时，如果没找到，就会取查询[[prototype]]，这个属性引用了一个对象， 我们称之为**原型对象**。

所以当我们把这些共有的属性和方法都放到这个所谓的**原型对象**上，是不是就可以达到我们上面所设想的，把公共属性和方法抽离出来呢？看下面代码：如果我们把xiaowu和stone两个对象的原型对象设置为对象fe。那么，当下面调用xiaowu.getInfo()时候，对象xiaowu上没有getInfo这个方法，那么就会查找自己的内部属性[[prototype]]原型,这个原型指向原型对象fe，xiaowu对象顺利在原型对象fe上找到方法getInfo,同时找到了属性job，顺利完成里输出。等等。。貌似还不行，少了一个设置原型原型对象为fe，请看下下面的代码。
``` javascript { .theme-peacock }
var xiaowu = {
	team: '国际化'
}
var stone = {
	team: '商业安全'
}

var fe= {
	job: '前端',
	getInfo: function(){
		console.log(this.team + '的' + this.job)
	}
xiaowu.getInfo() // 国际化的前端
stone.getInfo() // 商业安全的前端
```
这里讲怎么设置一个对象的原型对象，就是让一个对象在自己的属性和方法里找不到时，去另一个对象查找的对象。
这里我们引出了另一个属性**\_\_proto\_\_**，我们上面讲了[[prototype]]是内部属性，即看不到也没法设置。但是各个游览器提供给我们一个属性叫做**\_\_proto\_\_**， 我们可以用这个属性查看[[prototype]]和设置它。

``` javascript { .theme-peacock }
xiaowu.__proto__ = fe;
stone.__proto__ = fe;

```





#### 我们先理清下面几个名词，[[prototype]]、\_\_proto\_\_、prototype

 - [[prototype]] ：ECMAScript 规定的内部属性，存在于每个对象中，对外不可见。
 -  \_\_proto\_\_ ：各家游览器为了访问[[prototype]]实现的属性（支持IE11以上）。
 - prototype ：prototype（原型） [ˈproʊtətaɪp]是函数的独有属性，其他类型没有，这个属性是给构造函数用的。
 


#### 你可以理解这个3个名词都指向的是原型，只是表述或者用的地方不同而已。

1. \_\_proto\_\_ 是 [[prototype]] 的实现，\_\_proto\_\_ 可以在游览器中看到。
2. \_\_proto\_\_  在所有的实例中都存在，除了null（后面我们会说到）。
3. 不过函数中除了\_\_proto\_\_ 还有一个 prototype，（函数也是对象）


> 所以我们可以忽略 [[prototype]] 用\_\_proto\_\_代替就好了。



 \_\_proto\_\_ 是内部的属性，是原型链的根本，当一个使用一个变量时候在局部变量中找不到这个变量就会去访问这个对象的\_\_proto\_\_属性.
这个\_\_proto\_\_会指向另外一个对象，这个对象本身也有自己的\_\_proto\_\_，这样原型链就形成了。
```javascript { .theme-peacock }
var my = function () {}
var fun = my.__proto__
fun
function() {}

var obj = fun.__proto__ 
obj
Object {}

var who = obj.__proto__
who
null

my.__proto__.__proto__.__proto__ === null

```
我们看到上面的例子，当我们在一个函数内去调用一个变量时，整个取值过程会沿着\_\_proto\_\_一直找到Object的\_\_proto\_\_。再往上就是null了，因为null本身没有\_\_proto\_\_，所以查找到此为止。

这个是最根本的JS基础，不涉及高等级的概念。


貌似整个过程和prototype没什么关系。
如果我们这样理解：\_\_proto\_\_是整个机制的内部属性，不想对外暴露，但是提供一个接口可以让我们更改原型链。而这个接口就是prototype，这样是不是就很好理解他俩的关系了呢？

```javascript { .theme-peacock }
var Fe = function () {}
var xiaowu = new Fe()
xiaowu.__proto__ === Fe.prototype



```




这个对象称为这个函数的原型，

### 5.2.2 构造函数
 
> 所有对象都有一个constructor （构造）[kən'strʌktə] 属性，指向这个对象的构造函数，因为一个对象是肯定是一个函数创建的。
> 包含对象、函数、字符串、数字、布尔值等等

> JS 是面向对象最友好的语言，因为很少有语言可以不用过类，直接通过字面量创建。






2. 这个对象只在被New调用的时候被创建，最后被关联到函数的portotype，
3. new会劫持所有被new调用的函数


```javascript { .theme-peacock }
//按tab键跳到这里
```
## 一点铺垫

> 函数prototype
```javascript { .theme-peacock }
var info = {
	age: 12,
	jon: 'fe'
}
var obj = {
	name: 'xiaoming',
	info: info // 我们看到getNameFn引用的是函数。
}

```

## 为什么有原型
> 原型里有很多公用方法，当我们声明一个对象、数组、字符串等时候就会自动去原型上找这些公共方法。不用在每个实例上都实现一遍这些公共方法，比如toString()方法等。

## 举个例子
> 任何函数都有一个属性prototype指向一个对象
```javascript { .theme-peacock }
var superObj = function(){
}
superObj.prototype => {
}
new superObj()
```

### for...in 查找整个原型链

### 5.1.2属性设置和屏蔽
1. 普通属性没有设置只读，会产生屏蔽属性。
2. 如果属性标识了只读，会忽略，不会赋值。
3. 如果这个值是setter，一定会调用这个setter。不会赋值。
> 上述只影响 "="  赋值.

## 5.2 类

##### 构造函数 => new 构造函数 => {  }


```javascript { .theme-peacock }

var a= {}
a.constructor
// 所以对象的构造函数就是Object
Object() { [native code] }

function b (){}
b.constructor
// 函数的构造函数就是Function
Function() { [native code] }

// 对象没有prototype
b.prototype
Object {}
```
