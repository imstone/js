## 关于this
 
被定义在所有函数内
 
###
## 调用位置
关心调用栈，在当前执行函数的前一个调用位置。

## 绑定规则
### 函数调用时候
```javascript { .theme-peacock }
function f1(){
  return this;
}

f1() === window; // true
``` 
### 作为一个对象的方法调用

```javascript { .theme-peacock }
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

console.log(o.f()); // logs 37
```
 
### 显示绑定
> call apply bind
s
 
 
```javascript { .theme-peacock }
function add(c, d){
  return this.a + this.b + c + d;
}

var o = {a:1, b:3};

add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16
add.apply(o, [5,7])
```

```javascript { .theme-peacock }
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var o = {a:37, f:f, g:g};
console.log(o.f(), o.g()); // 37, azerty
```
### new 绑定
```javascript { .theme-peacock }
function People(name){
  this.name = name;
}

1, var obj = {}
2, this => {}  => this.a  obj.a =37
3. return obj
var ls = new People('lisen');
var trx = new People('tanruixing');
console.log(ls.name); // logs 37


function C2(){
  this.a = 37;
  return {a:38};
}

o = new C2();
console.log(o.a); // logs 38
```
### DOM事件处理函数中的 this
当函数被用作事件处理函数时，它的this指向触发事件的元素（一些浏览器在使用非addEventListener的函数动态添加监听函数时不遵守这个约定）。
```javascript { .theme-peacock }

function bluify(e){
  console.log(this === e.currentTarget); // 总是 true

  // 当 currentTarget 和 target 是同一个对象是为 true
  console.log(this === e.target);        
  this.style.backgroundColor = '#A5D9F3';
}

// 获取文档中的所有元素的列表
var elements = document.getElementsByTagName('*');

// 将bluify作为元素的点击监听函数，当元素被点击时，就会变成蓝色
for(var i=0 ; i<elements.length ; i++){
  elements[i].addEventListener('click', bluify, false);
```
 
### 内联事件处理函数中的 this
当代码被内联处理函数调用时，它的this指向监听器所在的DOM元素：
```javascript { .theme-peacock }
<button onclick="alert(this.tagName.toLowerCase());">
  Show this
</button>

上面的alert会显示button。注意只有外层代码中的this是这样设置的：
<button onclick="alert((function(){return this})());">
  Show inner this
</button>
```
## 优先级
 
按照上面的顺序从下往上一次起作用
## this 绑定的其他情况
### 传null 进去绑定

类似解构数组
## 箭头函数

ES6 的 箭头函数 this 不遵循上述约定
this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。
