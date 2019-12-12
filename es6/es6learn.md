# 2.let和const命令

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

