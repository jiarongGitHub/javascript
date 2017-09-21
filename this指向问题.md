### this
#### 在Js中，this的指向分为四种绑定形式，要想弄明白this到底指向谁，需要抓住一条主线：**函数的调用位置上的调用形式**
#### 调用形式不同，this就指向不同的对象
### 首先我们了解一下this的4种绑定形式：
 1. 独立调用，叫做默认绑定规则，this绑定给window对象
    独立调用在严格模式“use strict”下，this绑定给undefined
 2. 对象**.** 的形式调用，叫做隐式绑定规则，this绑定给最近的那个调用对象
 3. call、apply、bind调用，叫做显示绑定规则，this指给绑定的对象
 4. new 调用，叫做new绑定，this指向构造出来的实例化对象

----------
### 下面我们详细说一下4种绑定形式：
#### 什么是独立调用默认绑定呢？
```
function foo() {
	var a = 2;
	console.log( this.a );
   }
foo();
输出结果：undefined
```
#### 以上foo函数在调用时就是独立调用，独立函数调用,如果使用了非严格模式,this 会绑定到全局对象(window)
#### 上面提到了严格模式，严格模式分为函数在严格模式中运行以及在严格模式下调用函数,**那么在严格模式下调用函数的话独立模式还是会指向全局对象window，只有函数运行在严格模式下，才回影响到this，this会指向undefined**，下面我们看一个严格模式下的例子：
```
function foo() {
	   "use strict";//函数运行在严格模式下
		console.log( this.a );
	}
var a = 2;
foo();
输出结果：Uncaught TypeError: Cannot read property 'a' of undefined

"use strict"; //函数运行在严格模式下
function foo() {
	    console.log( this.a );
	}
		var a = 2;
		foo();
输出结果：Uncaught TypeError: Cannot read property 'a' of undefined


var a = 2;
function foo() {
		console.log( this.a );
	}
(function(){
		"use strict";//函数在严格模式下调用
		foo();
	})()
输出结果：2
```
#### 什么是对象.的形式隐式绑定呢？
```
var a=3;
function foo() {
		console.log( this.a );
	}
var obj = {
	    a: 2,
		fun: foo
	};
	obj.fun(); 
输出结果：2
```
```
function foo() {
		console.log( this.a );
	}
var obj2 = {
		a: 42,
		foo: foo
	};
var obj1 = {
		a: 2,
		obj2: obj2
	};
obj1.obj2.foo(); 
输出结果：42
```
#### 对象**.** 的形式调用，this绑定给最近的那个调用对象

#### 隐式绑定中有个很重要的点叫做隐式丢失，那么什么是隐式丢失呢？
#### 隐式丢失就是以隐式绑定的规则去赋值或传参，最终使用独立调用的形式去调用，this就使用了独立调用的默认绑定规则，导致this绑定给了全局对象window，这样就违背了开发人员在赋值和传参时候的意图。
### 常见的隐式丢失：赋值、参数传递 请看以下例子：
```
//赋值导致隐式丢失
function foo(){
    console.log(this.a);
}
var obj={
 a:2,
 fun:foo
}
var b=obj.fun;
b();
输出结果：undefined
```
```
//参数传递导致隐式丢失
//参数传递其实就是一种隐式赋值，因此我们传入函数时也会被隐式赋值
function foo() {
		console.log( this.a );
	}
function doFoo(fn) {
		fn(); 
	}
var a = "oops, global"; 
var obj = {
		a: 2,
		foo: foo
	};
doFoo( obj.foo ); 
输出结果：oops, global
```
#### 什么是call，apply，bind调用，显示绑定呢？
#### 我们可以call、apply强制把this绑定给一个指定的对象，这种绑定叫做硬绑定。
```
function fun(a,b){
    console.log(this.a,a,b);
}
var obj={
    a:2
}
fun.call(obj,'a','b');
输出结果：2,'a','b'
```
```
function fun(a,b){
    console.log(this.a,a,b);
}
var obj={
    a:2
}
fun.apply(obj,['a','b']);
输出结果：2,'a','b'
```
![](leanote://file/getImage?fileId=59c32a9aa781a213f5000000)
```
//apply()方法可以将一个伪数组对象转为真正的数组
var obj={
    length:3,
    0:"Tom",
    1:"Sala",
    2:"Jreey"
}
var arr=Array.apply(null,obj);
console.log(arr);
输出结果：["Tom", "Sala", "Jreey"]
```
#### 硬绑定
```
//	硬绑定的典型应用场景就是创建一个包裹函数，传入所有的参数并返回接收到的所有值
	function foo(arg1,arg2) {
		console.log( this.a,arg1,arg2);
		return this.a + arg1;
	}
	var obj = {
		a:2
	};
	var bar = function() {
		return foo.apply( obj, arguments);
	};
	
	
	var b = bar(3,2); // 2 3 2
	console.log( b ); // 5
```
#### 硬绑定函数bind
#### 简单的辅助绑定函数    bind函数的作用：返回一个新的函数，并且指定该新函数的this指向
```
document.write('test');
var altwrite=document.write;
altwrite('hello');
//1.以上代码有什么问题:
//对于上面这道题目，答案并不是太难，主要考点就是this指向的问题，
//altwrite()函数改变了write的this的指向，让它指向global或window对象，
//导致执行时提示非法调用异常
//解决办法就是bind硬绑定函数
var altwrite=document.write.bind(document);
altwrite('hello');
```
### 硬绑定函数可以解决隐式丢失问题
#### 什么是new调用，new绑定呢？
```
function P(name,age){
    this.name=name;
    this.age=age;
}
var tom=new P("tom",18);
console.log(tom.name,tom.age);
输出结果：tom 18
//这就是new绑定，this指向的是new出来的实例化对象
```
----------
#### this绑定规则的优先级
#### new绑定 > 显示绑定 > 隐式绑定 > 默认绑定
#### 被忽略的this：如果你把 null 或者 undefined 作为 this 的绑定对象传入 call 、 apply 或者 bind，这些值在调用时会被忽略，实际应用的是默认绑定规则
#### 柯里化：给目标函数预绑定一些参数
```
function foo(a,b) {
		console.log( "a:" + a + ", b:" + b );
	}
var bar=foo.bind(null,8);
bar(7);
输出结果：a:8, b:7
//bind函数传入的参数8已经给foo函数进行了参数预绑定，已经占了一位的参数a,所以再传入7的时候只能传给b
```
```
//我们还可以创建一个特别干净的空对象，不会污染全局进行预绑定参数，实现柯里化
function foo(a,b) {
		console.log( "a:" + a + ", b:" + b );
	}
var ø = Object.create( null );//{}
var bar=foo.bind(ø,2);
bar(3);a:2, b:3
```
### 以上为Js中this指向以及通过this指向涉及到其他的一些知识点的小总结



