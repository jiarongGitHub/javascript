#### ����
##### js�е������б����������Լ�����������
#### ����������4������
#### 1.���������������������������������������������
#### 2.���������������ڱ���������
#### 3.������ָ��������������������
#### 4.��if(){}else{}�����Ŀ��ڲ���Ҫ���庯���������������򲻻����if else������������ʾ
#### ���������ǿ��������ӣ�
```
��1:
test();
var a=2;
function test(){
  console.log(a);
}
������Ϊ��
undefined
```
#### ��1��������֮��Ϊ��
```
function test(){
  console.log(a);
}
var a;
test();
a=2;
���ʱ�����aΪundefined�ͺ���������
```
```
��2:
function foo() {
		console.log( 1 );
	}	
    var foo=2;
    foo(); 
��������
Uncaught TypeError: foo is not a function
```
#### ��2���������
#### ����������  Ȼ���������  Ȼ�������ֵ Ȼ����� 
```
������֮��������ִ�У�
function foo(){
console.log(1);
}
var foo;
foo=2;
foo();
foo������2  ��������Ȼ��������Ϊ������һ����������
```
```
��3:
var foo=2;
		test();
		function test(){
			foo=5;
			function foo(){
				console.log(1);
			}
		}
		console.log(foo);
������Ϊ��2
```
#### ��3���������
#### ���������������ǰ���б���������
```
����֮��
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
�����е�foo��ȫ���е�foo����ͬһ�������Ժ����еĸı䲻��Ӱ��ȫ�ֵġ�
```

```
��4��
var foo=2;
		test();
		function test(){
			foo=function (){
				console.log(1);
			}
			foo=5;
		}
		console.log(foo);
������Ϊ��5
```
#### ��4���������
```
����֮��
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
test�����е�foo=function(){
    console.log(1);
   }
   foo=5;���ǽ������ѯ����������������Ѱ��foo����������Ȼȫ�����У�����һ���İѿ�ʼ��2���ǵ�������Ϊ5.
```
```
��5��
        console.log(a);
		console.log(b);
		if(true){
			var a=2;
		}else{
			var b=3;
		}
������Ϊundefined undefined
```
#### ��5��������������������������if else���������жϵİ�ʾ



