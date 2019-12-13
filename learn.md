#   2.let和const命令

## 2.1.let命令

### 基本用法

1.用于声明变量，用法类似于var，但是声明的变量只能在其代码块有效，例如｛｝

2.所声明的变量一定要在声明后使用，否则会报错

3.全局变量var lmp，块级作用域在let lmp会报错，为暂时性死区

4.不允许重复声明

5.do表达式

```javascript
let x=do{
    let t=f();
    t*t+1;
}
//变量x会得到整个块级作用域的返回值
```

## 2.2.const命令

### 基本用法

1.const声明一个常量

2.不可重复声明

3.可以将一个对象声明为常量，常量储存的是地址，地址指向一个对象

4.冻结一个对象，用Object.freeze

5.彻底将一个对象冻结的函数

```javascript
var constantize=(obj)=>{
    Object.freeze(obj);
    Object.keys(obj).forEach((key,i)=>{
        if(typeof obj[key]==='object'){
            constantize(obj[key]);
        }
    })
}
```



## 2.4 顶层对象的属性

1.window

```javascript
var a=1;
window.a //1
```

```javascript
let a=1;
window.a //undefiend
```

2.浏览器中，顶层对象是window；浏览器和web worker中，self指向顶层对象，但是node没有self；node中，顶层对象是global，其他环境不支持；全局环境中，this会返回顶层对象，在node和es6中， this返回当前模块；

# 3.变量的解构赋值

## 3.1.数组的解构赋值

### 基本用法

1.解构的定义：按照一定模式从数组和对象中提取值，然后对变量进行赋值。

2.模式匹配：

```javascript
let [foo,[[bar],baz]=[1,[[2],3]];
```

```javascript
let [head,...tail]=[1,2,3,4];
head //1
tail //[2,3,4]
```

3.对于set解构，有可以使用数组的解构赋值

```javascript
let [x,y,z]=new Set(['a','b','c']);
x // 'a'
```

### 默认值

1.允许指定默认值

```javascript
let [x,y='b']=['a'];
x //a
y //b
```

2.null不严格等于undefined

```javascript
let [x=1]=[undefined];
x //1
let [y=1]=[null];
y  //null
```

## 3.2.对象的解构赋值

1.可以用于对象

```javascript
let {foo,bar}={foo:'a',bar:"b"};
foo //a
bar //b
```

2.变量名和属性名不一样时,baz才是被赋值，foo知识匹配的模式

```javascript
let {foo:baz}={foo:'aaa'};
baz //aaa
foo //error
```

## 3.3.字符串的解构赋值

1.数组的对象都有一个length属性

```javascript
let {length:len}='hello';
len //5
```

## 3.5.函数参数的解构赋值

```javascript
function move({x=0,y=0}={}){
    return [x,y];
}
move({}); //[0,0]
```

1.move的参数是一个对象，对这个对象进行解构，解构失败有默认值

```javascript
function move({x,y}={x:0,y:0}){
    return [x,y];
}
move({}); //[undefined,undefined]
```

2.是为参数指定默认值，而不是为变量x和y指定默认值。

# 5.正则的扩展

## 5.3.U修饰符

1.ES6对正则表达式添加了u修饰符

```javascript
var s="哈"
/^.$/.test(s) //false
/^.$/u.test(s) //true
```

2.es6新增了使用大括号表示Unicode字符的表示法，必须加上u修饰符才能识别当中的大括号，否则会被解读为量词。

```javascript
/\u{61}/.test("a")  //false
/\u{61}/u.test("a")  //true
/哈{2}/u.test("哈哈")  //true
```

```javascript
function codePointLength(text){
    var result = text.match(/[\s\S]/gu);
    return result ? result.length :0;
}
var s="吉吉";
s.length //4
codePointLength(s)  //2
```

## 5.4.y修饰符

1.y修饰符作用和g修饰符类似，后一次匹配从上一次匹配成功的下一个位置开始。不同之处，g只要剩余位置中存在匹配就行，而y修饰符必须从剩余的第一个位置开始。粘连 sticky

```javascript
var a = "aaa_aa_a";
var r1= /a+/g;
var r2=/a+/y;

r1.exec(s); // ["aaa"]
r2.exec(s);// ["aaa"]
 
r1.exec(s); //["aa"]
r2.exec(s); // null 
```

2.g修饰符的lastIndex 属性指定每次搜索的开始位置，g修饰符从这个位置开始向后搜索，直到发现匹配为止。y修饰符的lastIndex属性要求必须在lastIndex指定的位置发现匹配。



3.在split方法中使用y修饰符 （！！！与书本不符，后续待研究）

```javascript
'x##'.split(/#/y) //['x','','']
'##x'.split(/#/y) //['','','x']
'#x#'.split(/#/y) //['','x','']
'##'.split(/#/y)  //['','','']
```

4.g修饰符会漏掉非法字符，而y不会

## 5.6.flags属性

```javascript
/abc/ig.source //'abs'
/abc/ig.flags //'gi'
```

## 5.7.s修饰符：dotAll模式

1./s可以匹配任意单个字符  例如/foo.bar/s.test("foo\nbar");

## 5.9.Unicode属性类

1.\p{...}\P{...}，允许正则表达式匹配符合Unicode某种属性的所有字符，例如希腊文字母箭头等，但是使用的时候一定要加/u修饰符，否则会报错

## 5.10.具名组匹配

1.正则表达式使用圆括号进行组匹配，使用exec方法就可以将这3组匹配结果提取出来。

```javascript
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
'2015-02-04'.replace(re,'$<day>/$<month>/$<year>');  //04/02/2015
```

# 6.数值的扩展

## 6.1.二进制和八进制表示法

1.es6提供了二进制和八进制数值的新写法，分别用前缀0b（0B）或0o（0O）表示

## 6.2.Number.isFinite()、Number.isNaN()

1.第一个新方法只对数值有效，对于非数值一律返回false ；第二个新方法只对NaN有效，其他返回false

## 6.3.Number.parseInt()、Number.parseFloat() 

1.与parseInt还有parseFloat行为完全保持不变

## 6.4.Number.isInterger()

1.用来判断一个值是否为整数，3和3.0被视为同一个值

## 6.5.Number.EPSILON

1.极小的常量

## 6.6.安全整数和Number.isSafeInteger()

1.能够准确表示的整数范围在-2的53此房和2的53此房之间（不包含两个端点）

## 6.7.Math对象的扩展

### 1.Math.trunc()

用于去除一个数的小数部分，返回整数部分

对于非数值，内部使用Number方法将其先转为数值

### 2.Math.sign()

1.用来判断一个数到底是正数、负数，还是零

2.对于非数值，会先将其转换为数值

3.正数，返回+1；负数返回-1；0返回为0 ；-0返回-0；其他值返回NaN

### 3.Math.cbrt()

用于计算一个数的立方根

对于非数值，先转换为数值；字符串返回NaN

### 4.Math.clz32()

1.返回一个数的32位无符号整数形式有多少个前导0   例如Math.clz32(0)   32

2.对于小数，只考虑整数部分

3.对于空值或其他类型的值，会将他们先转为数组，再计算   例如Math.clz32(NaN)   //32  Math.clz32(true)   //32

### 5.Math.imul()  相乘

返回两个数以32位带符号整数形式相乘的结果，返回的也是一个32位的带符号整数

对于很大的数的乘法，低位数值是不精确的，js无法保存额外的精度，就把地位的值都变成了0.imul可以返回正确的1

### 6.Math.fround()

返回一个数的单精度浮点数形式

### 7.Math.hypot()

返回所有参数的平方和的平方根

参数不是数值，方法会将其转化为数值

### 8.对数方法

#### 1.Math.expm1()

Math.expm1(x)返回e^x-1  即Math.exp(x)-1

#### 2.Math.log1p()

Math.log1p(x)返回ln（1+x）  即Math.log(1+x) 如果x<-1则返回NaN

#### 3.Math.log10()

Math.log10(x)返回以10为底的x的对数。如果x小于0，则返回NaN

#### 4.Math.log2()

Math.log2(x)返回以2为底的x的对数。如果x小于0，则返回NaN

### 9.双曲函数方法

Math.sinh(x)

Math.cosh(x)

Math.tanh(x)

Math.asinh(x)

Math.acosh(x)

Math.atanh(x)

## 6.8.Math.signbit()

Math.sign()用来判断一个值的正负

Math.signbit()判断符号位是否设置，返回true或者false；

## 6.9.指数运算符

1.es6新增了一个指数运算符（**）

```javascript
2**2  //4
a**=2  //等同于a=a*a;
```

## 6.10 Integer数据类型

Integer只能用来表示整数，没有位数的限制。

与number类型区别，数据必须使用后缀n来表示

# 7.函数的扩展

## 7.1函数参数的默认值

### 1.基本用法

1.es6允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function log(x,y='World'){
  console.log(x,y);
}
log('Hello');  //Hello World
log('Hello','China'); //Hello World
log('Hello',''); //Hello
```

### 2.与解构赋值默认值结合使用

```javascript
function log({x,y='World'}){
  console.log(x,y);
}
log({}); //undefined World
log({x:'Hello'});//Hello World
log({x:'Hello',y:'China'});//Hello World
log();error


function m1({x=0,y=0}={}){
  return [x,y];
}
m1({}) //[0,0]

function m2({x,y}={x:0,y:0}){
  return [x,y];
}
m2({}) //[undefined,undefined]
```

### 3.参数默认值的位置

参数对应位置传值undefine，结果触发了默认值，传值null没有触发默认值

### 4.函数的length属性

指定了默认值以后，函数的length属性将返回没有指定默认值的参数个数。

```javascript
(function (a) {}).length  //1
(function (a = 5) {}).length  //0
```

### 5.作用域

```javascript
 var x=1;
 function f(x,y=x){
   console.log(y);
 }
 f(2);  //2

let x=1;
function f(y=x){
  let x=2;
  console.log(y);
}
f();   //1 调用函数f时，参数y=x形成一个单独的作用域，x没有定义，于是会指向全局变量x
```

## 7.2.rest参数

...变量名

rest参数之后不能再有其他参数（即只能是最后一个参数）

函数的length属性不包括rest参数

## 7.4.name属性

name属性返回该函数的函数名

```javascript
(new Function).name //"anonymous"
functiton foo(){};
foo.bind({}).name  //bound foo
```

## 7.5.箭头函数

### 1.基本用法

1.如果箭头函数不需要参数或需要多个，就是用圆括号代表参数部分  var a=(x,y)=>x+y;

```javascript
var f=v=>v;
f(3)  //3
//相当于
var f=function(v){
    return v;
}
var f=()=>5;
f() //5
//相当于
var f=function(){
    return 5;
}
```

2.如果箭头函数直接返回一个对象，必须在对象外面加括号

```javascript
var getItem=id=>({id:'id',name:"name"});
getItem().id   //"id"
```

3.箭头函数可以与变量结构结合使用

```javascript
const full=({first,last})=>first+'aaa'+last;
full({first:'gg',last:'dd'})  //ggaaadd;
//相当于
function full(person){
    return person.first+'aaa'+person.last;
}
```

**箭头函数的一个作用是简化回调函数**

### 注意事项

1.函数体内的this对象就是定义时所在的对象，而不是使用时所在的对象。

2.不可以当做构造函数，不可以使用new命令

3.不可以舒勇arguments对象，可以用rest对象代替

4.不可以使用yield命令

## 7.6.尾调用优化

### 1.尾调用优化

函数里的尾部return一个函数，后面不可以跟算法

```javascript
//以下不属于尾调
function a(x){
  let y = b(x);
  return y;
}
function a(x){
  return b(x) + 1;
}
//以下属于尾调
function a(x){
  if (x > 0) return b(x)
  return c(x);
}
function a(x){
  return b(x-1);
}
```



尾调的好处可以省内存，只有不再用刀外层函数的内部变量，内层函数的调用帧才会取代外层的调用帧，从而进行尾调用优化

### 2.尾递归

```javascript
function factorial(n,total){
    if(n===1) return total;
    return  factorial(n-1,n*total)
}
```

只保留一个调用记录，复杂度O（1）

### 3.递归函数的改写

循环可以用递归代替，递归最好用尾递归

### 4.尾递归优化的实现



# 8.数组的扩展

## 8.1 扩展运算符

#### 定义：将一个数组变成一个参数序列

```javascript
function f(x,y,z,v,w){}
var args=[0,1];
f(-1,...args,2,...[3]);
```

#### 作用：替代数组的apply方法

#### 应用：

##### 1、合并数组

```javascript
//es5
[1,2].concat(more)
//es6
[1,2,...more]
```

##### 2、与解构赋值结合

扩展运算符用于数组赋值，只能放在参数的最后一位

##### 3.函数的返回值

没看懂

##### 4.字符串

```javascript
[...'hello']
//["h","e","l","l","o"]
```

能够正确识别32位的Unicode字符

```javascript
'x\uD83D\uDE80y'.length //4
[...'x\uD83D\uDE80y'].length //3
```

##### 5.实现了Iterator接口的对象

能把对象转化为数组

```javascript
var nodeList=document.querySelectorAll("div");
var array = [...nodeList];
```

 对于没有部署Iterator接口的类似数组的对象，用Array.from

## 8.2 Array.from()

Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）对象，包括set和map

```javascript
let arrayLike={'0':'a',
              '1':'b',
               '2':'c',
               length:3
              }
//es5
var arr1 = [].slice.call(arrayLike);
//es6
var arr2 = Array.from(arrayLike);


//nodelist对象
let ps=document.querySelectorAll('p');
Array.from(ps).forEach(function(p){
    console.log(p)
})

//argument对象
function foo(){
    var args= Array.from(arguments);
}
```

字符串和Set结构都具有Iterator接口，因此可以被Array.from转化为真正的数组。

对于没有部署该方法的浏览器，可以用Array.prototype.slice方法代替。

还可以接受第二个参数，作用类似于数组的map方法，对每个元素进行处理，将处理后的值放回返回的数组

```javascript
Array.from(arrayLike,x=>x*x);
//等同于
Array.from(arrayLike).map(x=>x*x);


Array.from([1,,2,,3],(n)=>n||0)l
//[1,0,2,0,3]

```



## 8.3 Array.of()

定义：用于将一组值转换为数组

## 8.4 数组实例的copyWithin（）

Array.prototype.copyWidth(target, start=0,end=this.length)

target(必选)：从该位置开始替换数据

start（可选）：从该位置开始读取数据，默认为0，如果为负值，表示倒数。

end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数

```javascript
//将3号位复制到0号位
[1,2,3,4,5].copyWithin(0,3,4)
//[4,2,3,4,5]


//-2相当于倒二位，-1是倒一位
[1,2,3,4,5].copyWithin(0,-2,-1)
//[4,2,3,4,5]
```

对于没有部署TypeArray的copyWidth方法的平台需要用

```javascript
[].copyWithin.call(new Int32Array([1,2,3,4,5]),0,3,4);
//Int32Array  [4,2,3,4,5]

```

## 8.5  数组实例的find（）和findIndex（）



数组实例的find方法用于找出第一个符合条件的数组成员，参数是一个回调函数，所有数组成员依次执行该回调函数，知道找到为true的成员。

```javascript
[1,4,-5,10].find((n)=>n<0) //-5
```

## 8.6 数组实例的fill（）

fill方法使用给定值填充一个数组

```javascript
['a','b','c'].fill(7) ///[7,7,7]
```

## 8.7 数组实例的entries（），keys（）和values（）

用于遍历数组 

keys（是对键名的遍历），values（）是对键值的遍历，entries是对键值队的遍历

## 8.8 数组实例的includes（）

foreach（），filteer（），evety(),some()都会跳过空值

map（）会跳过空位

join（）和tostring（）会将空位视为undefined

