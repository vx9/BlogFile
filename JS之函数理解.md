[TOC]

# JS之函数理解#

## 前言##

知识需要总结，形成属于自己的知识架构，这样才算是真正的理解，即时很长时间没有看，微微温习也会立即拾起。

### 函数是对象，函数名是指向对象的指针​

## 1. 函数定义

### 1.1 函数声明

```javascript
sum(2,3);
function sum(a,b){
  alert(a+b);
}
//输出5
```

解析器会提前对函数声明进行解析，不是按行解析，所以以上代码无误。这就是<u>**函数声明提升**</u>(function declaration hoisting)。

> tip:变量提升呢？就是变量声明提升。

```javascript
var v = 'Hello World';

(function() {
	alert(v);
	var v='I love you';//如果没有这句，会是“Hello World”,因为没有v的定义，所以回去全局作用域查找v。
})(); 

//undefined
//2个知识点：1.变量声明提升；2.临时作用域（仿块级作用域）
```

变量提升：函数里`alert(v)`相当于`var v; alert(v);` ,因为没有定义，所以是undefined

临时作用域：`(function() {statement})()`,虽然JS没有块级作用域，但这样就仿块级作用域，目的是防止全局污染，防止多人开发时出现命名冲突。

### 1.2 函数表达式

```javascript
sum(2,3);

var sum = function(a,b) {
  alert(a+b);
};

//出现错误：函数不存在。函数表达式没有提升，必须先赋值才能使用。
```

```javascript
if(condition) {
  var say = function() {
    alert("hi");
  };
} else {
  var say = function(){
    alert("hello");
  };
}

say();
//函数表达式声明再使用，以上代码不能用函数声明代替
```

```javascript
//不能这样用！
if(condition) {
  function say() {
    alert("hi");
  }
} else {
  function say() {
    alert("hello");
  }
}

say();
//在ES中是无效语法，JS引擎会尝试修正，但是浏览器修正方式不一致，会得到不同结果。
```

函数调用执行时，参数不一定就要和定义时的参数个数一样，不一样解析器不会出错。

```javascript
function doSth() { 
    alert(arguments[0]+arguments[1]);
}

doSth(1);	//NaN
doSth(2,3);	//5
```

### 1.3 Function构造函数###

不推荐这种定义，但有助于理解函数是对象，函数名是指向对象的指针。

```javascript
//不推荐这种定义，最后一个参数是函数的方法内容。
var sum = new Function(a,b,"return a+b;");
```

## 2. 函数没有重载:star:

后定义的函数会覆盖先定义的函数，函数的参数不同也不会出现重载，而是覆盖。

> tip:可以用函数内部的arguments对象的属性来模仿重载，如下代码

```javascript
function doSth() {
  if(arguments.length == 1) {
    alert("这个函数有一个参数,是"+arguments[0]);
  } else if (arguments.length == 2) {
    alert("这个函数有两个参数，是"+arguments[0]+"和"+arguments[1]);
  }
}

doSth(1);	//这个函数有一个参数,是1
doSth(2,3);	//这个函数有两个个参数,是2和3
```

## 3.函数可以作为值使用

3.1 函数名就是指针变量，可以作为参数传递。

> tip:此时的函数名不加括号，若加括号就是调用函数，那么传递的就是函数的执行结果。

> tip:是否会想到深拷贝和浅拷贝？​

3.2 函数也可以作为另一个函数的返回值（闭包概念）。

## 4.函数内部对象

### 4.1 arguments对象

arguments对象是函数内部的特殊对象，主要用途是访问函数 **实际运行中 **的参数数组。

注意的是：

1. 他 **不是** 函数参数数组，而与参数数组保持同步。

		   2. 他访问的是实际运行中的参数数组。arguments.length与函数名.length不一定相等。
		   3. 只能在函数内部使用，不能放在外部。

#### arguments.callee指针

指向所在函数，用在递归函数中，为了避免耦合。

```javascript
function factorial(num) {
  if (num <=1 ) {
    return 1;
  } else {
    return arguments.callee(num-1);
  } 
}

//递归计算阶乘，arguments.callee指向的是factorial本身。
```





### 4.2 this对象

this指向直接调用者。

| 在什么情景下？    | this指向谁？      |
| :--------- | :------------ |
| 全局函数中      | this = window |
| 对象方法调用中—正常 | this =对象      |
| 对象方法调用—闭包  | this =window  |

```javascript
var name = "this window";

var obj = {
  name : "my obj",
  
  getNameFunc : function() {
    return function() {
      return this.name;
    };
  }
};

alert(obj.getNameFunc()());	//this window
```

以上代码obj内存在闭包，this指向window。obj.getNameFunc()()立即调用执行函数。

## 5.函数的属性和方法

### 5.1 .length属性

函数**定义**时参数的个数（运行时就不一定有几个参数了）

```javascript
function doSth(a) { 
    alert(arguments.length);
}
	
doSth(2,3,3);	//3
alert(doSth.length);//1
```

### 5.2 .prototype原型属性

这个很重要，是JS创建对象中原型模式创建的关键。原型模式就是利用prototype来创建共享的属性和方法。详细请看另一篇文章。

### 5.3 call()和apply()方法

用于改变函数的作用域。关于作用域问题请看另一篇文章。

`call(作用域，参数)`:参数必须枚举出来。

`apply(作用域，参数数组)`:参数是数组。

```javascript
var color = "red";
var temp = {color:"blue"};

function sayColor() { 
    alert(this.color);
}
	
sayColor();             //red
sayColor.call();        //red
sayColor.call(this);    //red
sayColor.call(window);  //red
sayColor.call(temp);    //blue
sayColor.apply(temp);   //blue
```

ES5还有bind()方法。跟call和apply功能类似，但是他返回该函数的实例。`var sy = sayColor.bind(temp);sy();`

### 5.4 其他方法

toString(), toLocalString()





