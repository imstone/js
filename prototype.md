# 原型
### 引子和说明


1. 这篇文章虽然比较长，但是内容不是那么多，因为有大量代码，所以可以耐心看下去。 
2. 下面很多代码是基于chrome调试的。
3. 下面代码有些不会写全，因为是基于上面例子已经存在的变量继续写的，请注意
4. 看下面的文章我们已经假定你对JS已经比较熟悉了
5. 这篇文章屏蔽了一些点和最优方式，文章着重点在于理解原型这个概念。


> 原型看似平时工作中不会用到，其实我们工作中一直在用原型，只是大家不知道而已。
### 原型对象
 

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

所以当我们把这些共有的属性和方法都放到这个所谓的**原型对象**上，是不是就可以达到我们上面所设想的，把公共属性和方法抽离出来呢？看下面代码：如果我们把xiaowu和stone两个对象的原型对象设置为对象fe。那么，当下面调用xiaowu.getInfo()时候，对象xiaowu上没有getInfo这个方法，那么就会查找自己的内部属性[[prototype]]原型,这个原型指向原型对象fe，xiaowu对象顺利在原型对象fe上找到方法getInfo,同时找到了属性job，顺利完成里输出。等等。。貌似还不行，少了一个如何设置原型原型对象为fe，请看**下下面**的代码。
``` javascript { .theme-peacock }
var xiaowu = {
	team: '国际化'
}
var stone = {
	team: '商业安全'
}

var fe = {
	job: '前端',
	getInfo: function(){
		console.log(this.team + '的' + this.job)
	}
}
xiaowu.getInfo() // 国际化的前端
stone.getInfo() // 商业安全的前端
```
这里讲怎么设置一个对象的原型对象，就是让一个对象在自己的属性和方法里找不到时，去另一个对象查找的对象。
这里我们引出了另一个属性**\_\_proto\_\_**，我们上面讲了[[prototype]]是内部属性，即看不到也没法设置。但是各个游览器提供给我们一个属性叫做**\_\_proto\_\_**， 我们可以用这个属性查看[[prototype]]和设置它。其实我建议从这里开始忘记[[prototype]]，把\_\_proto\_\_ 想象为[[prototype]]就好了，毕竟多一个名词没啥好的，况且这个属性还看不到，只能通过其他属性或者方法去查看设置。
下面的代码很简单，就是简单的把两个对象的原型对象设置为拥有公共方法和属性的fe，这样当xiaowu和stone在调用自己不存在的属性和方法时就可以顺着原型找到原型对象fe上了。就可以从上面拿到自己想要的属性和方法。
> \_\_proto\_\_ 只支持IE11以及以上其他游览器都支持，ES6提供了更好的方法。大家可以自己查阅。

``` javascript { .theme-peacock }
xiaowu.__proto__ = fe;
stone.__proto__ = fe;
```

### 原型链

> 我们知道作用域一直会往外层作用域查找，直到查到最外层的window，这个叫做作用域链。那么原型也是有这个概念的，叫做原型链。


还是看下面这段代码的**分割线上一部分**，如果我们查找工作地址这个方法想查看stone的工作地址，显然在原型对象fe内是没有工作地址这个属性的。怎么办呢？仔细想下我们怎么称呼fe？是不是叫做原型对象？所以原型对象也是一个对象，是对象就都用原型，那么如果我给原型对象的原型指到一个有工作地点的对象上呢？
看下面代码的分割线的下一部分，我们把stone的原型对象fe的原型对象设置为baidu了这样stone就可以通过自己的原型找到自己的原型对象fe，在fe上找不到还可以通过fe的原型找到fe的原型对象baidu在他上面找到了工作地点。这样原型的就连成链状，俗称原型链。

```javascript { .theme-peacock }
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
}
xiaowu.__proto__ = fe;
stone.__proto__ = fe;

// stone.getWorkAddress()

// 看完上面的解释再看下面这段代码。
var baidu= {
	address: '后厂村路，百度科技园',
	getWorkAddress: function(){
		return this.address
	}
}

fe.__proto__ = baidu;

stone.getWorkAddress()

```

在下面的例子更能形象表现原型链，我们看到 \_\_proto\_\_  已经连成串了，其实还可以增加很多。
xiaowu 的原型的原型就是对象baidu

```javascript { .theme-peacock }
xiaowu.__proto__.__proto__ === baidu
// 返回 true
```

> 上面我们解释了什么是原型和原型对象，以及原型链概念。

### 默认原型

我相信很多聪明好奇的同学已经忍不敲出下面的代码了，既然是原型链，那么我看看baidu的原型对象是啥，我都没给baidu这个对象设置原型对象。结果打印出了一个对象，并不是我们肯能认为的空之类的东西，我们点开这个输出，发现一大堆的方法。
也许你猜到了，是的如果我们不设置一个对象的原型对象他会有一个自己的默认原型对象，并且这个对象拥有很多对象的公共方法，比如toString()等方法。

```javascript { .theme-peacock }
xiaowu.__proto__.__proto__.__proto__
// Object {}
// constructor: Object()
// hasOwnProperty: hasOwnProperty()
// toString: toString()
/// valueOf: valueOf()get 
```

所以你们又猜到了,我们可以直接调用xiaowu.toString(),打印了如下信息。对象的toString()方法可以打印出对象的类型。

```javascript { .theme-peacock }
xiaowu.toString();
// "[object Object]"
```

接下来我们继续接力原型链,我们再加一个\_\_proto\_\_， 这次打印出来的就是null了。这就到头了，当原型链找到null时候就代表原型链到头了，因为null没有原型。
```javascript { .theme-peacock }
xiaowu.__proto__.__proto__.__proto__.__proto__
// null
```

#### 字符串、数值、布尔等基本类型

每当基本类型在调用时，会自动把这个基本类型转换为对相应的类型的对象，从而让我们可以调用这个基本类型的一些方法，看下面的代码，当我们调用字符类型name的length属性时就会发生这个过程。
```javascript { .theme-peacock }
var name = 'xiaowu';
name.length
// 6
```
我们可以用下面的代码来模拟上述过程,name在此时是个对象，所以会访问他的原型链找到length属性，返回6。下面的代码不是真是的过程，我们只是模拟过程而已。

```javascript { .theme-peacock }
var name = new String('xiaowu');
name.length;
// 6
```

#### 我们先理清下面几个名词，[[prototype]]、\_\_proto\_\_

 - [[prototype]] ：ECMAScript 规定的内部属性，存在于每个对象中，对外不可见。
 -  \_\_proto\_\_ ：各家游览器为了访问[[prototype]]实现的属性（支持IE11以上）。
 


#### 你可以理解这个2个名词都指向的是原型，只是表述或者用的地方不同而已。

1. \_\_proto\_\_ 是 [[prototype]] 的实现，\_\_proto\_\_ 可以在游览器中看到。
2. \_\_proto\_\_  在所有的实例中都存在，除了null（后面我们会说到）。


> 所以我们可以忽略 [[prototype]] 用\_\_proto\_\_代替就好了。

