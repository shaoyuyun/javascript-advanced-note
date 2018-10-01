# javascript-advanced-note
This repository is used to record the note of learning advanced javascript.
## 1. 数据类型
### 1.1 课程介绍
#### 课程大纲如下：  
1. 数据类型
2. 表达式和运算符
3. 语句
4. 对象
5. 数组
6. 函数
7. this
8. 闭包和作用域
9. OOP
10. 正则与模式匹配
#### 学习方法：  
##### 学习资料：  
1. 《JavaScript权威指南》
2. [MDN](https://developer.mozilla.org/zh-CN/learn/javascript)
3. 多动手实践+参与讨论

### 1.2 JavaScript六种数据类型
JavaScript被称为是一种弱类型的语言，原因就是数据类型居然能够随意转换而不报错，而且在定义变量的时候不用指定其类型，示例代码如下：  
![代码](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrvdx38cij30s40gq3ys.jpg)  
JavaScript拥有五种原始类型和一种对象类型：  
![类型](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrvdyhug2j30ra0er3yv.jpg)  

### 1.3 JavaScript隐式转换
#### 加号（+）和减号（-）
在数字与字符串做运算的时候，加号做拼接，减号就做减法  
![+-](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrvjosrtdj30tm0higm5.jpg)  
#### 等于号（==）
等于号判断原始类型的时候会有自动类型转换的特点，在判断引用类型的时候会从引用地址上进行判断。  
![==](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrvjp5si9j30s90frt8z.jpg)  
![==](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrvjpxqdlj30xl0h4mxw.jpg)  
#### 严格等于（===）
相比于等于号，严格等于不会进行类型转换，它会一开始判断两者之间的类型，如果类型不同，直接返回false。类型相同且内容相同才返回true。  
![===](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrvjpjq0ej30qk0ghglw.jpg)  

### 1.4 JavaScript包装对象
JavaScript是面向对象的语言，使用”`.`”操作符可以访问对象的属性和方法，而对于基本类型（`null`,`undefined`, `bool`, `number`, `string`）应该是值类型，没有属性和方法，然而：  
```js
var str = "this is a string";
console.log(str.length); //16
console.log(str.indexOf("is")); //2
```
结果很简单，但是仔细想想还真奇怪，`string`不是值类型吗！怎么又有属性又有方法的！其实只要是引用了字符串的属性和方法，JavaScript就会将字符串值通过`new String(s)`的方式转为内置对象String，一旦引用结束，这个对象就会销毁。所以上面代码在使用的实际上是String对象的`length`属性和`indexOf`方法。  
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrvr4u7a2j30ly0fedg5.jpg)  

同样的道理，数字和布尔值的处理也类似。`null`和`undefined`没有对应对象。既然有对象生成，能不能这样：  
```js
var str = "this is a string";
str.b = 10;
console.log(str.b);  //undefined
```
结果并没有返回10，而是`undefined`！不是说好了是个对象吗！正如刚才提到第二行代码只是创建了一个临时的String对象，随即销毁，第三行代码又会创建一个新的临时对象（这就是低版本IE频繁处理字符串效率低的一个原因），自然没有b属性，这个创建的临时对象就成为包装对象。  
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrvr6ftdvj30qu0i674z.jpg)  

### 1.5 JavaScript类型检测
在JavaScript中，有很多种检测数据的类型，主要是有以下几种：  
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrw10yr16j30sz0f9glu.jpg)  
#### typeof：一般用于检测原始数据类型，引用数据类型无法具体的检测出来
```js
console.log(typeof ""); //string
console.log(typeof 1); //number
console.log(typeof true); //boolean
console.log(typeof null); //object
console.log(typeof undefined); //undefined
console.log(typeof []); //object
console.log(typeof function() {}); //function
console.log(typeof {}); //object
```
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrw11drp0j30ix0fzgm1.jpg)  
其实`null`是js设计的一个败笔，早期准备更改`null`的类型为`null`，由于当时已经有大量网站使用了`null`，如果更改，将导致很多网站的逻辑出现漏洞问题，就没有更改过来，于是一直遗留到现在。  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrw11qrwcj30jx05w747.jpg)  
#### instanceof：检测引用数据类型
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrw122u0pj30eg02pq2s.jpg)  
```js
console.log("1" instanceof String);  //false
console.log(1 instanceof Number);  //false
console.log(true instanceof Boolean);  //false
console.log([] instanceof Array);  //true
console.log(function() {} instanceof Function);  //true
console.log({} instanceof Object);  //true
```
可以看到前三个都是以对象字面量创建的基本数据类型，但是却不是所属类的实例，这个就有点怪了。后面三个是引用数据类型，可以得到正确的结果。如果我们通过`new`关键字去创建基本数据类型，你会发现，这时就会输出`true`,如下：  
```js
console.log(new String("1") instanceof String); //true
console.log(new Number(1) instanceof Number); //true
console.log(new Boolean(true) instanceof Boolean); //true
console.log([] instanceof Array); //true
console.log(function() {} instanceof Function); //true
console.log({} instanceof Object); //true
```
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrw12lzkrj30to0inaba.jpg)  
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrw132zabj30xv0j10u9.jpg)  
#### constructor：似乎完全可以应对基本数据类型和引用数据类型，都能检测出数据类型
```js
console.log(("1").constructor === String); //true
console.log((1).constructor === Number); //true
console.log((true).constructor === Boolean); //true
console.log(([]).constructor === Array); //true
console.log((function() {}).constructor === Function); //true
console.log(({}).constructor === Object); //true
```
事实上并不是如此，来看看为什么：  
```js
function Fn(){};
Fn.prototype=new Array();
var f=new Fn();

console.log(f.constructor===Fn);  //false
console.log(f.constructor===Array);  //true
```
声明了一个构造函数，并且把他的原型指向了Array的原型，所以这种情况下，`constructor`也显得力不从心了。  
#### Object.prototype.toString：终极数据检测方式
```js
var a = Object.prototype.toString;

console.log(a.call("aaa"));  //[object String]
console.log(a.call(1));  //[object Number]
console.log(a.call(true));  //[object Boolean]
console.log(a.call(null));  //[object Null]
console.log(a.call(undefined));  //[object Undefined]
console.log(a.call([]));  //[object Array]
console.log(a.call(function() {}));  //[object Function]
console.log(a.call({}));  //[object Object]
```
![7](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrw13iospj30xe0f70tn.jpg)  
#### 类型检测小结
![8](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrw13z7ffj30rd0gmgmo.jpg)  

## 2. 表达式和运算符
### 2.1 JavaScript表达式
#### 概念
表达式是指能计算出值的任何可用程序单元，或者可以这么说：表达式是一种JavaScript短语，可以使JavaScript解释器用来产生一个值。  
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwdsss9oj30vh0c1t92.jpg)  
#### 原始表达式
1. 常量、直接量：比如3.14、“test”等这些；
2. 关键字：比如`null`，`this`，`true`等这些；
3. 变量：比如i，k，j等这些。  
![2](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwdt79y7j30qg0cw0t0.jpg)  
#### 复合表达式
比如：10\*20  
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwdtinsfj30kg07a0sq.jpg)  
#### 数组和对象的初始化表达式
比如：`[1,2]`、`{x:1,y:2}`等这些。  
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwdtwn69j30q00f6q3c.jpg)  
#### 函数表达式
比如：  
```js
var fe = function(){}
```
或者  
```js
(functiong(){console.log('hello world');})()
```
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwdui114j30ki08fq30.jpg)  
#### 属性访问表达式
比如：  
```js
var o = {x:1}
```
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwduu5tsj30e909dwef.jpg)  
访问属性的方式有`o.x`或者`o['x']`。  
#### 调用表达式
比如：`funName()`  
![7](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwdv75j5j308p05aglg.jpg)  
#### 对象创建表达式
比如：`new Func(1,2)`或者`new Object`  
![8](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwdvl1dbj30bj06saa0.jpg)  
#### 表达式小结
![9](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwdvx61sj30ms0gcdg5.jpg)  

### 2.2 JavaScript运算符
#### 运算符
![1](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwotb044j30p20c1mx9.jpg)  
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrwotphm8j30sp0gkmxt.jpg)  
#### 运算符`?`
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwou32xhj30mp0bqjre.jpg)  
#### 运算符`,`
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwoufkagj30ml0c1jrd.jpg)  
#### 运算符`delete`
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwov0hb9j30s30est8y.jpg)  
![6](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrwovgay8j30qj0ew3yx.jpg)  
#### 运算符`in`
![7](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrwovwgdej30pb0btaa3.jpg)  
#### 运算符`instanceof`,`typeof`
![8](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrwowkyz1j30st0db0sz.jpg)  
#### 运算符`new`
![9](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwox7qzuj30s60ekwf1.jpg)  
#### 运算符`this`
![10](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwoxwmsbj30ok0cx3ys.jpg)  
#### 运算符`void`
![11](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwoyqumcj30ny0cojrh.jpg)  
#### 特殊运算符总结
![12](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrwozdmgyj30u60e2mxy.jpg)  
#### 运算符优先级
![13](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrwp00lvlj30w90hl3z6.jpg)  

## 3. 语句
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrx2vnsiqj30ti0c70t2.jpg)  
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrx2wt9lqj30r00fszko.jpg)  
### 3.1 JavaScript block语句、var语句
#### `block`语句
块语句常用于组合0~N个语句，往往用一对花括号定义。  
```js
{
  var str = 'hi';
  console.log(str);
}

if(true) {
  console.log('hi');
}
```
在ES6出来之前，JavaScript是没有块级作用域的，具体看以下两段代码：  
```js
for (var i=0; i<10; i++) {                
  var str = 'hi';  
  console.log(str); 
}  
等同于
 var i = 0;
for (; i<10; i++) {                
  var str = 'hi';  
  console.log(str); 
} 

{
  var x = 1;
}
等同于
var x = 1;
{

}

function foo() {
  var a = 1;
  console.log(a);  //1
}
foo();
console.log(typeof a);  //undefined
```
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrx2xdzv5j30wk0hn3zd.jpg)  
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrx2yj16zj30tx0h7aal.jpg)  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrx2z1xozj30r40ezwev.jpg)  
#### `var`语句
```js
function foo() {
  //隐式的将b定义为全局变量
  var a = b = 1;
}
foo();
console.log(typeof a);  //undefined
console.log(typeof b);  //number

function foo() {
  //同时定义多个变量的正确方式
  var a =1,b = 1;
}
foo();
console.log(typeof a);  //undefined
console.log(typeof b);  //undefined
```
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrx30ciqij30ti0g8aaf.jpg)  

### 3.2 JavaScript try-catch语句
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrxfpe6hmj30o50d0t8s.jpg)  
#### 说明
`try`跟`catch`搭配使用可以检测`try`里边的代码有没有抛出`error`，如果有`error`就会跳转到`catch`里执行`catch`里的程序。执行顺序为：先`try`捕获异常，执行`catch`里面的内容，最后执行`finally`里面的内容，当然也可以只写`catch`或者`finally`两个中的一个，但是必须写`try`语句块。  
```js
try {
  throw 'test';
} catch(e) {
  console.log(e); //test
} finally {
  console.log('必须执行的');
}
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrxfovqp4j30re0eut90.jpg)  
#### 嵌套使用
```js
try {
  try {
    throw new Error('oops');
  } finally {
    console.log('finally');
  }
} catch(e) {
  console.error('outer', e.message);
}
```
由于在嵌套层中并没有`catch`语句，因此输出结果的顺序为：`finally`、`outer`、`oops`。  
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrxfps3xtj30tr0f0wex.jpg)  
#### 下面请看含有`catch`语句的
```js
try {
  try {
    throw new Error('oops');
  } catch(ex) {
    console.log('inner', ex.message);
  } finally {
    console.log('finally');
  }
} catch(ex) {
  console.error('outer', ex.message);
}
```
以上代码输出结果的顺序为：`inner`、`oops`、`finally`。原因是异常已经在内部处理过了，因此不会再到外部去处理。  
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrxfq64suj30r30f6dgc.jpg)  
#### 更复杂的嵌套
```js
try {
  try {
    throw new Error('oops');
  } catch(ex) {
    console.log('inner', ex.message);
    throw ex;
  } finally {
    console.log('finally');
  }
} catch(ex) {
  console.error('outer', ex.message);
}
```
以上代码输出结果的顺序为：`inner`、`oops`、`finally`、`outer`、`oops`。原因在内部的`catch`语句重新向外抛出了`ex`这个异常。  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrxfqonopj30rm0eldga.jpg)  

### 3.3 JavaScript函数、switch、循环
#### 函数
function语句用于定义函数对象。称作函数声明。  
与之对应的另一种称为函数表达式。  
二者最显著的区别就是函数声明会被预先处理（或者叫函数前置），可以在函数声明前调用。  
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrxkl8jlfj30t00czt90.jpg)  
除此之外还有函数构造器`new`创建函数对象。之后章节会详细展开说明。  
#### `for in`语句
有以下的特点：  
1. 顺序不确定；
2. `enumerable`为`false`时不会出现；
3. `fon in`对象属性时会受到原型链的影响  
```js
var p;
var obj = { x: 1, y: 2 }
for(p in obj) {
  console.log(p);  //x  y
}
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrxklnks5j30w60ci0t7.jpg)  
#### `switch`
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrxkm1wu7j30u00ghq3r.jpg)  
#### 循环
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrxkmh180j30xm0bhaac.jpg)  
#### `with`
`with`语句可以修改当前作用域，但不推荐使用。  
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrxkmzu4cj30ss0f6dgf.jpg)  

### 3.4 JavaScript严格模式
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrxzxl8ssj30t90f3gmd.jpg)  
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrxzxy47hj30ta0ftgm2.jpg)  
![3](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrxzyim3nj30u00ftwf0.jpg)  
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrxzyv4esj30vu0gzwfa.jpg)  
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrxzzel2ej30s40e3t96.jpg)  
![6](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrxzzxw9lj30uo0fvmy6.jpg)  
![7](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvry011hwsj30sy0fpmxk.jpg)   
![8](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvry01kew0j30t00fy74z.jpg)  
![9](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvry00dg9qj30uj0ge0t9.jpg) 
![10](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvry0801qej30vb0eyjs2.jpg)  
![11](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvry08qbuuj30w20h2408.jpg)  

## 4. 对象
### 4.1 JavaScript对象概述
对象中包含一系列的属性，这些属性是无序的，每一个属性都有一个字符串`key`和对应的`value`
```js
var obj = {x:1,y:2};
obj.x;  //1
obj.y; //2
```
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvryatqy3aj30rs0eudga.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvryau3xw7j30r70e5dg4.jpg)  
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvryauibimj30tj0ek74o.jpg)  
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvryauxossj30wx0ipq45.jpg)  

### 4.2 JavaScript创建对象、原型链
#### 创建对象方式1—字面量
```js
var obj1 = {x:1,y:2};
var obj2 = {
  x:1,
  y:2,
  o:{
    z:3,
    n:4
  }
};
```
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrygffbavj30oq0gbmxa.jpg)  
#### 创建对象方式2—通过构造函数
```js
function foo() {}
foo.prototype.z = 3;
var obj = new foo();
obj.x = 1;
obj.y = 2;
console.log(obj.x); //1
console.log(obj.y); //2
console.log(obj.z); //3
console.log(typeof obj.toString()); //string
console.log(typeof obj.toString);  //function
console.log('z' in obj); //true
console.log(obj.hasOwnProperty('z')); //false
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrygg0rmkj30ui0ifmy7.jpg)  
![3](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvryggfp4aj30v20iwt9l.jpg)  
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrygguqwij30ty0iwmy2.jpg)  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvryghfuz6j30ug0ipab2.jpg)  
#### 创建对象方式3—`Object.create`
```js
var obj = Object.create({ x: 1 });
console.log(obj.x);  //1
console.log(obj.toString);  //function
console.log(obj.hasOwnProperty('x'));  //false

// null对象的原型链少了Object这一层
var obj1 = Object.create(null);
console.log(obj1.toString);  //undefined
```
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrygii9ppj30vx0gut9g.jpg)  

### 4.3 JavaScript属性操作
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrzdskq0lj30q00eemxd.jpg)  
#### 属性读写
```js
var obj = {x:1,y:2};
obj.x;  //1
obj['y'];  //2

//使用[]取得属性值一般场景
var obj = {x1:1,x2:2};
for(var i=1; i<=2; i++){
  console.log(obj['x' + i];
}
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzdv34x6j30t10gf74s.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrze1l0dmj30v10h1t9a.jpg)  
#### 属性删除
```js
var person = {age:28,title:'test'};
delete person.age;  //true
delete preson['title'];  //true
person.age;  //undefined
delete person.age;  //true

//无法删除原型
delete Object.prototype;  //false
//因为原型的configurable不可配置
var descriptor = Object.getOwnPropertyDescriptor(Object,'prototype');
descriptor.configurable;  //false
```
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrze21861j30u40faaat.jpg)  
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrze2jhmmj30ws0eo74y.jpg)  
#### 属性检测
```js
var cat = new Object();
cat.legs = 4;
cat.name = 'tom';

//for in检测
'legs' in cat;  //true
'abc' in cat;  //false
'toString' in cat;  //true

//检测对象本身的属性
cat.hasOwnProperty('legs');  //true
cat.hasOwnProperty('toString');  //false

//检测对象的属性是否可枚举，包括原型上的
cat.propertyIsEnumerable('legs');  //true
cat.propertyIsEnumerable('toString');  //false
```
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrze3ejv3j30um0hfmy0.jpg)  
![7](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrze30lqaj30s40gqt9k.jpg)  
#### 属性枚举
```js
var o = { x: 1, y: 2, z: 3 };
'toString' in o; //true
o.propertyIsEnumerable('toString'); //false
var key;
for(key in o) {
  console.log(key); //x,y,z
}

var obj = Object.create(o);
obj.a = 4;
var key;
for(key in obj) {
  console.log(key); //a,x,y,z
}

var obj = Object.create(o);
obj.a = 4;
var key;
for(key in obj) {
  if(obj.hasOwnProperty(key)) {
    console.log(key); //a
  }
}
```
![8](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrze3te4zj30we0hl75b.jpg)  

### 4.4 JavaScript get/set方法
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrze46543j30r60ck0ss.jpg)  
#### getter()和setter()方法
```js
var man = {
  name: 'tom',
  weibo: 'haha',
  get age() {
    return new Date().getFullYear() - 1988;
  },
  set age(val) {
    console.log('你没有权限设置年龄为：' + val);
  }
}
console.log(man.age);  //30
man.age = 100;  //你没有权限设置年龄为：100
console.log(man.age);  //30

//修改一下，使上面的代码更复杂
var man = {
  name: 'tom',
  weibo: 'haha',
  $age: null,
  get age() {
    if(this.$age == undefined) {
      return new Date().getFullYear() - 1988;
    } else {
      return this.$age;
    }
  },
  set age(val) {
    val = +val;
    if(!isNaN(val) && val > 0 && val < 150) {
      this.$age = +val;
    } else {
      console.log('年龄无法设置为：' + val);
    }
  }
}
console.log(man.age); //30
man.age = 100;
console.log(man.age); //100
man.age = 'abc'; //年龄无法设置为：NaN
```
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzk1atrbj30tq0hfwf9.jpg)  
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvrzk1otu5j30x40huwfd.jpg)  
#### get/set与原型链
```js
//get和set设置的值不会被直接更改
function foo() {}
Object.defineProperty(foo.prototype, 'z', {
  get: function() {
    return 1;
  }
})
var obj = new foo();
console.log(obj.z); //1
obj.z = 10;
console.log(obj.z); //1

//defineProperty设置的值不会被直接更改
var o = {}
Object.defineProperty(o, 'x', { value: 5 });
var obj = Object.create(o);
console.log(obj.x); //5
obj.x = 200;
console.log(obj.x); //5

//更改defineProperty设置的值
var o = {}
Object.defineProperty(o, 'x', { writable: true, configurable: true, value: 5 });
var obj = Object.create(o);
console.log(obj.x); //5
obj.x = 200;
console.log(obj.x); //200
```
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzk23djaj30xh0inab9.jpg)  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzk2jyj6j30xc0fzdgm.jpg)  

### 4.5 JavaScript 属性标签
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrzqsqi11j30mo0bfq2x.jpg)  
```js
var person = {};
Object.defineProperty(person, 'name', {
  configurable: false,
  writable: false,
  enumerable: true,
  value: 'tom'
})
console.log(person.name);  //tom
person.name = 'jack';
console.log(person.name);  //tom
console.log(delete person.name);  //false
//获得属性的标签值
console.log(Object.getOwnPropertyDescriptor(person,'name'));

//属性标签的应用
var person = {};
Object.defineProperties(person, {
  title: {
    value: 'JavaScript',
    enumerable: true
  },
  corp: {
    value: 'BAT',
    enumerable: true
  },
  salary: {
    value: 50000,
    enumerable: true,
    writable: true
  },
  luck: {
    get: function() {
      return Math.random() > 0.5 ? 'good' : 'bad';
    }
  },
  promote: {
    set: function(level) {
      this.salary *= 1 + level * 0.1;
    }
  }
});
console.log(Object.getOwnPropertyDescriptor(person, 'salary'));
console.log(person.salary); //50000
person.promote = 2;
console.log(person.salary); //60000
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzqt4mohj30vu0fvwfo.jpg)  
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzqtl3txj30on0dhmxi.jpg)  
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzqu37dfj30w20f60tx.jpg)  
![5](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzqungjbj30va0hwgml.jpg)  
![6](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzqv1fcjj30z20jc75h.jpg)  

### 4.6 JavaScript对象标签、对象序列化
#### 对象标签
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrzwuo37oj30oj0dk3yj.jpg)  
##### 原型标签`__proto__`
![2](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrzwv0p5nj30qw0j8mxi.jpg)  
##### `class`标签
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzwvfe7wj30pt0frt9m.jpg)  
##### `extensible`标签
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzwvtxioj30wt0hpdgz.jpg)  
#### 序列化、其他对象方法
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzwwcd0yj30pp0csjrf.jpg)  
##### 序列化
```js
//将属性和值序列化为字符串
var obj = { x: 1, y: true, z: [1, 2, 3], nullVal: null };
//{"x":1,"y":true,"z":[1,2,3],"nullVal":null}
console.log(JSON.stringify(obj));

//有点小坑，得注意
var obj1 = { val: undefined, a: NaN, b: Infinity, c: new Date() };
//{"a":null,"b":null,"c":"2018-06-09T10:46:01.929Z"}
console.log(JSON.stringify(obj1));

//把字符串反序列化为属性和值
var obj2 = JSON.parse('{"x":2333}');
//2333
console.log(obj2.x);
```
![6](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzwwrempj30vg0ejjs0.jpg)  
##### 序列化 - 自定义
![7](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvrzwxefmnj30pc0fmt8z.jpg)  
##### 其他对象方法
![8](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvrzwy8obij30qr0f8mxw.jpg)  
#### 小结
![9](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvrzwylw8rj30mr0g5jrp.jpg)  

## 5.数组
### 5.1 JavaScript创建数组、数组操作
#### 数组概述
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvszzh3zj5j30tz0ghgme.jpg)  
#### 创建数组 - 字面量
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvszzi8sapj30yf0i53zp.jpg)  
#### 创建数组 - new Array
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvszzincc5j30uq0det93.jpg)  
#### 数组元素读写
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvszzj2dlnj30sl0g90sy.jpg)  
#### 数组元素增删
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvszzjuw9mj30vd0gb0tf.jpg)  
#### 数组迭代
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvszzkdmr2j30vg0hddgk.jpg)  

### 5.2 JavaScript二维数组、稀疏数组
#### 二维数组
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt02zsi1lj30se0hagm8.jpg)  
#### 稀疏数组
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt030od3rj30vs0gcgm8.jpg)  

### 5.3 JavaScript数组方法（上）
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt08a9uu0j30u20got9l.jpg)  
#### 将数组转为字符串
```js
var arr = [1, 2, 3];
console.log(arr.join());  //1,2,3
console.log(arr.join('-'));  //1-2-3

function repeatString(str, n) {
  return new Array(n + 1).join(str);
}
console.log(repeatString('a', 3));  //aaa
console.log(repeatString('hi', 2));  //hihi
```
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt08anhu1j30x80fzwf3.jpg)  
#### 将数组逆序输出
```js
var arr = [1, 2, 3];
console.log(arr.reverse());  //3,2,1
console.log(arr);  //3,2,1
```
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt08b3w2gj30y40eamxe.jpg)  
#### 将数组进行排序
```js
var arr = [13, 24, 52, 3];
//13,24,3,54
console.log(arr.sort());
//13,24,3,54
console.log(arr);
var arrSort = arr.sort(function(a, b) { return a - b; });
//3,13,24,52
console.log(arrSort);

var arr1 = [
  { age: 25 }, 
  { age: 39 }, 
  { age: 22 }
];
arr1.sort(function(a, b) {
  return a.age - b.age;
})
arr1.forEach(function(item) {
  console.log('age:', item.age);
})
```
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt08bn9blj30wk0g43z8.jpg)  
#### 将数组合并
```js
var arr = [1, 2, 3];
//[1,2,3,4,5]
console.log(arr.concat(4, 5));
//[1,2,3]
console.log(arr);
//[1,2,3,10,11,13]
console.log(arr.concat([10, 11], 13));
//[1,2,3,1,[2,3]]
console.log(arr.concat([1, [2, 3]]));
```
![5](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt08dxei0j30xv0evjru.jpg)  
#### 返回部分数组
```js
var arr = [1, 2, 3, 4, 5];
//[2,3]
console.log(arr.slice(1, 3));
//[2,3,4,5]
console.log(arr.slice(1));
//[2,3,4]
console.log(arr.slice(1, -1));
//[2]
console.log(arr.slice(-4, -3));
//[1,2,3,4,5]
console.log(arr);
```
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt08ejpejj30wb0do0t4.jpg)  
#### 将数组进行拼接
```js
var arr = [1, 2, 3, 4, 5];
//[3,4,5]，从下标为2开始删除
console.log(arr.splice(2));
//[1,2]
console.log(arr);

var arr1 = [1, 2, 3, 4, 5];
//[3,4]，从下标为2开始删除，删除两个元素后停止
console.log(arr1.splice(2, 2));
//[1,2,5]
console.log(arr1);

var arr2 = [1, 2, 3, 4, 5];
//[2]，从下标为1开始删除，删除一个元素后停止，并且添加a和b这两个元素
console.log(arr2.splice(1, 1, 'a', 'b'));
//[1,"a","b",3,4,5]
console.log(arr2);
```
![7](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt08f80eij30xo0gqt9b.jpg)  

### 5.4 JavaScript数组方法（下）
#### 将数组进行遍历
```js
var arr = ['a', 'b', 'c', 'd', 'e'];
arr.forEach(function(x, index, a) {
  console.log(x + '-' + index + '-' + (a === arr));
});
```
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0hg98mrj30vq0f9mxh.jpg)  
#### 将数组进行映射
```js
var arr = [1, 2, 3];
arr.map(function(x) {
  //11,12,13
  console.log(x + 10);
});
//[1,2,3]
console.log(arr);
```
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt0hgr4v3j30wh0dt74k.jpg)  
#### 将数组进行过滤
```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
arr.filter(function(x, index) {
  //1,2,3,4,5,6,7,8,9,10
  console.log(x);
  //0,1,2,3,4,5,6,7,8,9
  console.log(index);
  return index % 3 === 0 || x >= 8;
});
//[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
console.log(arr);
```
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt0hh7g67j30wm0dtmxm.jpg)  
#### 将数组进行判断
```js
var arr = [1, 2, 3, 4, 5];
//判断每一个元素是否小于10
arr.every(function(x) {
  return x < 10;
});
//判断是否有一个元素等于3
arr.some(function(x) {
  return x === 3;
});
```
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0hhotwuj30xs0f9wf6.jpg)  
#### 将数组元素进行累加操作
```js
var arr = [1, 2, 3];
var sum = arr.reduce(function(x, y) {
  return x + y;
});
//6
console.log(sum);
//[1,2,3]
console.log(arr);

var arr1 = [3, 9, 6];
var max = arr1.reduce(function(x, y) {
  //3-9
  //9-6
  console.log(x + '-' + y);
  return x > y ? x : y;
});
//9
console.log(max);
//[3,9,6]
console.log(arr1);

var arr2 = [3, 9, 6];
var max2 = arr2.reduceRight(function(x, y) {
  //6-9
  //9-3
  console.log(x + '-' + y);
  return x > y ? x : y;
});
//9
console.log(max2);
//[3,9,6]
console.log(arr2);
```
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt0hi62ytj30yj0i63zg.jpg)  
#### 将数组进行检索
```js
var arr = ['a', 'b', 'c', 'd', 'e', 'a'];
//2,在数组下标为2的位置找到
console.log(arr.indexOf('c'));
//-1,找不到该元素
console.log(arr.indexOf('f'));
//0,在数组下标为0的位置找到，默认是从左到右查找
console.log(arr.indexOf('a'));
//5,在数组下标为5的位置找到，设置是从数组下标为1的位置开始向右查找
console.log(arr.indexOf('a', 1));
//5,在数组下标为5的位置找到，-3表示在d的位置开始向右查找
console.log(arr.indexOf('a', -3));
//0,在数组下标为0的位置找到，-6表示在最左a的位置开始向右查找
console.log(arr.indexOf('a', -6));
//-1,找不到该元素，-3表示在d的位置开始向右查找
console.log(arr.indexOf('b', -3));
//3,在数组下标为3的位置找到
console.log(arr.lastIndexOf('d'));
//-1,找不到该元素，2表示的是在e的位置向右查找
console.log(arr.lastIndexOf('d', 2));
//3,在数组下标为3的位置找到，5表示的是在b的位置向右查找
console.log(arr.lastIndexOf('d', 5));
```
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt0hikmo2j30xs0gbdgj.jpg)  
#### 判断是否为数组
```js
//true
console.log(Array.isArray([]));
//true
console.log(({}).toString.apply([]) === '[object Array]');
//true
console.log([] instanceof Array);
//true
console.log([].constructor === Array);
```
![7](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt0hiz53pj30ve0gawew.jpg)  

### 5.5 JavaScript数组小结
#### 数组 VS 对象
1. 相同点：都可以继承，数组是对象，对象不一定是数组，都可以当做对象一般添加删除属性。
2. 不同点：数组自动更新`length`，按索引访问数组常常比访问一般对象属性明显迅速，数组对象继承`Array.prototype`上的大量数组操作方法。  

![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0mm7kjhj30wc0cp3z1.jpg)  
#### 数组 VS 字符串
字符串是一种类数组  
```js
var str = 'hello world';
str.charAt(0);  //h
str[1];  //e

Array.prototype.join.call(str,'-');  //h-e-l-l-o--w-o-r-l-d
```
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0mmla0dj30q60dnaa9.jpg)  
#### 小结
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0mmycwzj30pa0fh74p.jpg)  

## 6. 函数和作用域（函数、this）
### 6.1 JavaScript函数概述
#### 函数概念
函数是一块JavaScript代码，被定义一次，但可以执行和调用多次。JavaScript中的函数也是对象，所以函数可以像其他对象那样操作和传递，所以我们也常叫JavaScript中的函数为函数对象。  
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt0sq9vfaj30x80ij0ti.jpg)  
#### 重点
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt0sr249bj30tf0ga0t7.jpg)  
#### 函数调用方式
1. 直接调用
2. 对象方法调用
3. 构造器调用
4. `call`/`apply`/`bind`调用  

![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt0srfuqsj30x60d7dg2.jpg)  

### 6.2 JavaScript函数声明与表达式
#### 函数声明
```js
function add(a, b) {
  var x = a;
  var y = b;
  return x + b;
}
```
#### 函数表达式
````js
//一般表达式
var add = function(a, b) {
    
}
//立即调用表达式
(function() {
    
})();
//返回表达式
return function() {
    
};
//命名函数表达式
var add = function foo(a, b) {
    
}
````
#### 函数声明和函数表达式的区别
他们最重要的区别就是函数声明能够提前，而函数表达式不行  
```js
var num = add(1, 3);
//4
console.log(num);
function add(a, b) {
  var x = a;
  var y = b;
  return x + b;
}

var num1 = add1(1, 3);
//add1 is not a function
console.log(num1);
var add1 = function(a, b) {
  var x = a;
  var y = b;
  return x + b;
}
```
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt0xp00yhj30vd0ilwf5.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt0xpg9emj30wj0haab1.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0xpttxnj30tt0igdgq.jpg)  
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt0xqbzjhj30sn0cxmxg.jpg)  
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt0xqxd2rj30ts0gcjsa.jpg)  
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt0xray2rj30rz0gs74p.jpg)  

### 6.3 JavaScript this
#### 全局的this（浏览器）
```js
console.log(this.document == document);  //true
console.log(this === window);  //true

this.a = 37;
console.log(window.a);  //37
```
![1](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt14poneij30rq0dzglw.jpg)  
#### 一般函数中的this
```js
function f1(){
  return this;
}
f1() === window;  //true

function f2(){
'use strict';
return this;
}
f2() === undefined;  //true
```
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt14q1vsgj30wd0e2mxj.jpg)  
#### 作为对象方法中函数的this
```js
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};
console.log(o.f()); //37

var o = { prop: 37 };
function independent() {
  console.log(this.prop); //37
  return this.prop;
}
o.f = independent;
console.log(o.f()); //37
```
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt14qhvn7j30v00fk3z1.jpg)  
#### 对象原型链上的this
```js
var o = {
  fn: function() {
    return this.a + this.b;
  }
};
var p = Object.create(o);
p.a = 1;
p.b = 4;
//5
console.log(p.fn());
```
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt14r6df8j30sn0eaweq.jpg)  
#### get/set方法与this
```js
function modulus() {
  //abs()返回数的绝对值
  return Math.abs(this.re + this.im);
}
var o = {
  re: 4,
  im: -9,
  get phase() {
    //sqrt()返回数的平方根
    return Math.sqrt(this.re);
  }
};
Object.defineProperty(o, 'modulus', {
  get: modulus,
  enumerable: true,
  configurable: true
});
console.log(o.phase); //2
console.log(o.modulus); //5
```
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt14rsdz2j30sr0hft98.jpg)  
#### 构造器中的this
```js
function Abc() {
  this.a = 33;
}
var oo = new Abc();
console.log(oo.a); //33

function Bcd() {
  this.b = 55;
  return { b: 66 };
}
var pp = new Bcd();
console.log(pp.b); //66
```
![6](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt14slf2yj30ss0ifmxk.jpg)  
#### call/apply方法与this
```js
function add(c, d) {
  return this.a + this.b + c + d;
}
var o = { a: 1, b: 3 };
//16
console.log(add.call(o, 5, 7));
//34
console.log(add.apply(o, [10, 20]));

function bar() {
  //[object Window]
  console.log(Object.prototype.toString.call(this));
}
bar();
//[object Number]
console.log(bar.call(10));
```
![7](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt14t1grbj30tk0iddgh.jpg)  
#### bind方法与this
```js
function bar() {
  return this.a;
}
var ger = bar.bind({
  a: 'test'
});
//f bar(){ return this.a; }
console.log(ger);
//test
console.log(ger());

var odr = {
  a: 33,
  bn: bar,
  gn: ger
};
//33
console.log(odr.bn());
//test
console.log(odr.gn());
```
![8](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt14tnzgaj30qk0fm74l.jpg)  

### 6.4 JavaScript函数属性arguments
```js
function foo(x, y, z) {
  //2
  console.log(arguments.length);
  //3
  console.log(arguments[0]);
  arguments[0] = 100;
  //100
  console.log(x)
  arguments[2] = 200;
  //undefined
  console.log(z);
  //200
  console.log(arguments[2]);
  //true
  console.log(arguments.callee === foo);
}
foo(3, 4);
//3
console.log(foo.length);
//foo
console.log(foo.name);
```
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1drikukj30xh0imdgy.jpg)  
#### apply/call方法（浏览器）
```js
function foo(x, y) {
  console.log(x, y, this);
}
//1,2,Number(100)
foo.call(100, 1, 2);
//3,4,Boolean(true)
foo.apply(true, [3, 4]);
//undefined,undefined,window
foo.apply(null);
//undefined,undefined,window
foo.apply(undefined);

function foo(x, y) {
  'use strict';
  console.log(x, y, this);
}
//undefined,undefined,null
foo.apply(null);
//undefined,undefined,undefined
foo.apply(undefined);
```
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1dsb9exj30x40fcjrs.jpg)  
![3](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1drwd15j30wx0got9b.jpg)  
#### bind方法
```js
this.x = 9;
var module = {
  x: 33,
  getX: function() {
    return this.x;
  }
};
console.log(module.getX()); //33

var gn = module.getX;
console.log(gn()); //9

var gn1 = gn.bind(module);
console.log(gn1()); //33
```
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1dsupl1j30s30hu0t6.jpg)  
#### bind与currying
函数有柯里化（currying）的特性，意思函数能拆分成多个单元。  
```js
function add(a, b, c) {
  return a + b + c;
}
var fn = add.bind(undefined, 100);
console.log(fn(1, 2)); //103

var fn1 = fn.bind(undefined, 200);
console.log(fn1(10)); //310
```
柯里化的实际运用  
```js
function getConfig(colors, size, otherOptions) {
  console.log(colors, size, otherOptions);
}
var defaultConfig = getConfig.bind(null, '#cc0000', '1024*768');
//#cc0000 1024*768 123
console.log(defaultConfig('123'));
//#cc0000 1024*768 456
console.log(defaultConfig('456'));
```
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1dthaaij30r70f80t2.jpg)  
![6](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1du3tubj30vx0f0dgi.jpg)  
#### bind与new
```js
function foo() {
  this.b = 100;
  return this.a;
}
var fn = foo.bind({ a: 1 });
//1
console.log(fn());
//{b:100}
console.log(new fn());
```
![7](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt1dur9duj30oi0flt8w.jpg)  
#### bind方法模拟
```js
if(!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if(typeof this !== 'function') {
      throw new TypeError('what is trying to be bound is not callable');
    }
    var aArgs = Array.prototype.slice.call(arguments, 1);
    var fToBind = this;
    var fNOP = function() {};
    var fBound = function() {
      return fToBind.apply(this instanceof fNOP ? this : oThis, aArgs.concat(Array.prototype.slice.call(arguments)));
    };
    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
  };
}
```
![8](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1e1pu5fj30xz0hymyk.jpg)  
![9](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1e2bt02j30y70i6400.jpg)  

## 7. 函数和作用域（闭包、作用域）
### 7.1 JavaScript理解闭包
#### 闭包的栗子
```js
function outer() {
  var localVal = 30;
  return localVal;
}
console.log(outer());

function outer1() {
  var localVal2 = 33;
  return function() {
    return localVal2;
  }
}
var fn = outer1();
console.log(fn())
```
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt1mpticsj30so0frgm0.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1mq8kdwj30xl0fzdgl.jpg)  
#### 常见错误之循环闭包
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt1mqp2b2j30ur0j1wff.jpg)  
#### 封装
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1mxr6yrj30sw0icwf7.jpg)  
#### 闭包的概念
在计算机科学中，闭包（也成为词法闭包或者函数闭包）是指一个函数或者函数的引用，与一个引用环境绑定在一起，这个引用环境是一个存储该函数每个非局部变量（也叫自由变量）的表。  

闭包，不同于一般的函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量。  
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1myegloj30vs0gnt9h.jpg)  
#### 小结
1. 优点：灵活、方便、封装；
2. 缺点：空间浪费、内存泄露、性能消耗。  

![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1n0vgu1j30sp0f30sv.jpg)  

### 7.2 作用域
#### 全局作用域、函数作用域和eval
```js
//全局变量
var a = 10;
(function() {
  //局部变量
  var b = 20;
})();
//10
console.log(a);
//undefined
console.log(b);

for(var item in { a: 1, b: 2 }) {
  //a  b
  console.log(item);
}
//b
console.log(item);

eval('var c = 100');
//100
console.log(c);
```
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt1rfktfsj30ry0hn0tc.jpg)  
#### 作用域链
```js
function outer1() {
  var local1 = 11;
  function inter() {
    var local2 = 22;
    //11
    console.log(local1);
    //22
    console.log(local2);
    //33
    console.log(global1);
  }
  inter();
}
var global1 = 33;
outer1();

function outer2() {
  var local3 = 44;
  var fn = new Function('console.log(typeof local3);');
  //undefined
  fn();
}
outer2();
```
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt1rg6txtj30qd0h2mxj.jpg)  
#### 利用函数作用域封装
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt1rgmfoyj30qu0h50sw.jpg)  

### 7.3 JavaScript ES3执行上下文
![1](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt21bu4m8j30ky03qq31.jpg)  
![2](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt21citafj30ut0hl0tm.jpg)  
![3](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt21cx5dij30tt0g2aaj.jpg)  
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt21dawy8j30wy0hw3z3.jpg)  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt21dtejlj30sw0hgmxt.jpg)  
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt21e5ehaj30po0fmwen.jpg)  
![7](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt21ejar8j30v50g4jrz.jpg)  
![8](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt21fp5eij30um0gugm9.jpg)  
![9](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt21g4d72j30xs0gmq3z.jpg)  
![10](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt21giy71j30y70i3my7.jpg)  
![11](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt21gwv22j30xu0h2aat.jpg)  
![12](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt21hbrftj30wc0h774w.jpg)  
![13](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt21huk4bj30mx0hemxc.jpg)  

### 7.4 小结
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt289yme7j30m30bv74m.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt28aleftj30rh0gvjrt.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt28b3la1j30it0egmxr.jpg)  
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt28bfnt2j30pt0higm2.jpg)  
![5](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt28c1r3cj30ws0ikdgz.jpg)  
![6](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt28cepwaj30rq0h5wez.jpg)  
![7](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt28ct8kaj30ty0i0js7.jpg)  
![8](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt28dcgumj30uy0ghgmd.jpg)  
![9](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt28e4mehj30xz0gsgmd.jpg)  

## 8. OOP（上）
### 8.1 概念与继承
面向对象程序设计是一种程序设计规范，同时也是一种程序开发的方法。对象指的是类的实例。它将对象作为程序的基本单元，将程序和数据封装其中，以提高软件的重用性、灵活性和扩展性。  
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt2hruq38j30xx0gngmj.jpg)  
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2hs809xj30y90gk3z5.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2hsnri0j30tl0hawfd.jpg)  
![4](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt2ht1w90j30x80ht755.jpg)  
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt2hto531j30yx0hrdhf.jpg)  

### 8.2 再谈原型链
![1](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt2mm3shjj30yy0jk0tw.jpg)  
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2mmj6oaj30yw0hnmyr.jpg)  
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt2p12j10j30oc0fhwf3.jpg)  
![4](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt2mor3c2j30oa0fbmxz.jpg)  
![5](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt2mp8uz5j30p80f1aap.jpg)  

### 8.3 prototype属性
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2sxcoz5j30yq0gdjs5.jpg)  
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt2mm3shjj30yy0jk0tw.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2syf7fhj30xx0fu3zb.jpg)  
![4](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2syw5hqj30v70j775b.jpg)  

### 8.4 instanceof
![1](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2vm7lkbj30sy0izq42.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt2vmm0nhj30tp0ihmya.jpg)  
![3](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt2vn3utaj30xg0iu75o.jpg)  

### 8.5 实现继承的方式
![1](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt2y1frlpj30wl0gsgma.jpg)  
![2](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt2y1tu46j30xv0fkt9i.jpg)  
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt2y293abj30yu0jmwfn.jpg)  

## 9. OOP（下）
### 9.1 OOP（模拟重载、链式调用、模块化）
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt31mvv8xj30w80gv3zc.jpg)  
![2](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt31navf8j30uk0iw0tt.jpg)  
![3](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt31nwe7sj30t90hct9j.jpg)  
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt31oabi9j30uo0h2abf.jpg)  
![5](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt31ozhbwj30xn0gy75k.jpg)  
![6](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt31pfd21j30w80ft0ta.jpg)  

### 9.3 实践（探测器）
![1](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt357l0dej30xt0hnmyx.jpg)  
![2](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt35807tfj30yh0ht764.jpg)  
![3](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt3598bwzj30w20eu0t9.jpg)  

## 10. 正则与模式匹配
### 10.1 JavaScript正则表达式
![1](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt3b9he11j30wd0fwt9l.jpg)  
![2](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt3b9x64dj30uw0f8aaf.jpg)  
![3](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt3baecdoj30y90i70tr.jpg)  
![4](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt3baqxymj30xo0i7q3b.jpg)  
![5](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt3bb8y0xj30sf0fjaa2.jpg)  
![6](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt3bbmpeqj30x20g6wev.jpg)  
![7](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt3bc0ixjj30ws0i0wfg.jpg)  
![8](http://wx4.sinaimg.cn/mw690/006epDUlgy1fvt3bck7nmj30tm0cfglx.jpg)  
![9](http://wx3.sinaimg.cn/mw690/006epDUlgy1fvt3bcy49fj30w60fs3z4.jpg)  
![10](http://wx1.sinaimg.cn/mw690/006epDUlgy1fvt3bmeanjj30v70fzgmd.jpg)  
![11](http://wx2.sinaimg.cn/mw690/006epDUlgy1fvt3bn1595j30yq0ekt9t.jpg)  

## 参考来源：
[慕课网JavaScript深入浅出](https://www.imooc.com/learn/277)  
[CruxF'Blog](https://github.com/CruxF/Blog/issues/10)  