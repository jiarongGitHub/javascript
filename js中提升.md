#### 提升
##### js中的提升有变量的提升以及函数的提升
#### 提升有以下4条规则：
#### 1.函数的提升是整体的提升，变量的提升是声明的提升
#### 2.函数的提升优先于变量的提升
#### 3.提升是指提升到本层作用域的最顶层
#### 4.在if(){}else{}这样的块内部不要定义函数，变量的提升则不会理会if else这样的条件暗示
#### 接下来我们看几个例子：
```
例1:
test();
var a=2;
function test(){
  console.log(a);
}
输出结果为：
undefined
```
#### 例1代码提升之后为：
```
function test(){
  console.log(a);
}
var a;
test();
a=2;
这个时候输出a为undefined就很明了啦！
```
```
例2:
function foo() {
		console.log( 1 );
	}	
    var foo=2;
    foo(); 
输出结果：
Uncaught TypeError: foo is not a function
```
#### 例2代码解析：
#### 函数先提升  然后变量提升  然后变量赋值 然后调用 
```
提升完之后是这样执行：
function foo(){
console.log(1);
}
var foo;
foo=2;
foo();
foo现在是2  调用它当然报错啊，因为它不是一个函数啊。
```
```
例3:
var foo=2;
		test();
		function test(){
			foo=5;
			function foo(){
				console.log(1);
			}
		}
		console.log(foo);
输出结果为：2
```
#### 例3代码解析：
#### 首先浏览器在运行前进行变量提升：
```
提升之后：
function test(){
    function foo(){
        console,log(1);
    }
    foo=5;

}
var foo;
foo=2;
test();
console.log(foo);
函数中的foo与全局中的foo不是同一个，所以函数中的改变不会影响全局的。
```

```
例4：
var foo=2;
		test();
		function test(){
			foo=function (){
				console.log(1);
			}
			foo=5;
		}
		console.log(foo);
输出结果为：5
```
#### 例4代码解析：
```
提升之后：
function test(){
   	foo=function (){
		console.log(1);
	}
		foo=5;
}
var foo;
foo=2;
test();
console.log(foo);
test函数中的foo=function(){
    console.log(1);
   }
   foo=5;都是进行左查询，在整个作用域中寻找foo的声明，显然全局中有，所以一层层的把开始的2覆盖掉，最终为5.
```
```
例5：
        console.log(a);
		console.log(b);
		if(true){
			var a=2;
		}else{
			var b=3;
		}
输出结果为undefined undefined
```
#### 例5代码解析：变量的提升不会理会if else这种条件判断的暗示



