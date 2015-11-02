

<!-- MarkdownTOC -->

- [ javascript常用操作](#-javascript常用操作)
	- [类型](#类型)
	- [字符串操作](#字符串操作)
	- [数组(array)](#数组array)
	- [对象](#对象)
	- [循环 for in的例子](#循环-for-in的例子)
	- [map和set(要支持ES6规范浏览器可以使用)](#map和set要支持es6规范浏览器可以使用)
	- [iterable (ES6规范引入)](#iterable-es6规范引入)
	- [函数](#函数)
		- [函数定义和调用](#函数定义和调用)
		- [ 变量的作用域](#-变量的作用域)
		- [高阶函数](#高阶函数)
		- [闭包](#闭包)
		- [箭头函数](#箭头函数)
		- [generator](#generator)
			- [标准对象](#标准对象)
				- [Date](#date)
				- [RegExp](#regexp)
				- [JSON](#json)
			- [面向对象](#面向对象)
				- [创建对象](#创建对象)
				- [继承](#继承)
					- [构造函数绑定](#构造函数绑定)
					- [prototype模式](#prototype模式)

<!-- /MarkdownTOC -->


<a name="-javascript常用操作"></a>
#  javascript常用操作

<a name="类型"></a>
#### 类型
- `NaN === NaN; // false`唯一能判断NaN的方法是通过isNaN()函数：`isNaN(NaN); // true`
- null和undefined: null表示一个“空”的值，而undefined表示值未定义,区分两者的意义不大。大多数情况下，我们都应该用null。undefined仅仅在判断函数参数是否传递的情况下有用。

<a name="字符串操作"></a>
#### 字符串操作
1. `toUpperCase()` 把一个字符串全部变为大写
2. `toLowerCase()` 把一个字符串全部变为小写
3. `indexOf()` 会搜索指定字符串出现的位置 
`var s = 'hello'; s.indexof('e') //返回1,没有到返回-1`
4. `substring()` 截取字符串,包含头不包含尾

<a name="数组array"></a>
#### 数组(array)
1. `indexOf()` 与String类似，Array也可以通过indexOf()来搜索一个指定的元素的位置
2. `slice()` slice()就是对应String的substring()版本，它截取Array的部分元素，然后返回一个新的Array
3. `push和pop` push()向Array的末尾添加若干元素，pop()则把Array的最后一个元素删除掉：
4. `unshift和shift` 如果要往Array的头部添加若干元素，使用unshift()方法，shift()方法则把Array的第一个元素删掉
5. `sort()` 可以对当前Array进行排序，它会直接修改当前Array的元素位置，直接调用时，按照默认顺序排序
6. `reverse()` 把整个Array的元素给掉个个，也就是反转
7. `splice` 方法是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

8. `concat()`方法把当前的Array和另一个Array连接起来，并返回一个新的Array,并没有改变原来的Array;concat()方法可以接收任意个元素和Array，并且自动把Array拆开，然后全部添加到新的Array里
9. `join()`方法是一个非常实用的方法，它把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串

<a name="对象"></a>
#### 对象
- 访问对象用`user.name` 或 `user['name']`,最好在属性上面加上`''`
- 用`delete`动态删除属性，`delete user.name`,添加属性就直接添加
- 要检查否拥有某一属性，可以用`in`操作符 `'toString' in user //true`;如果in判断一个属性存在，这个属性不一定是user的，它可能是user继承得到的
- 要判断一个属性是否是`user`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法

<a name="循环-for-in的例子"></a>
#### 循环 for in的例子
```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) { //过滤父类继承的属性
        alert(key); // 'name', 'age', 'city'
    }
}
```

<a name="map和set要支持es6规范浏览器可以使用"></a>
#### map和set(要支持ES6规范浏览器可以使用) 
- map

```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95

var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```
- set 

```javascript
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
s1.add(1);
s2.delete(3);  
```

<a name="iterable-es6规范引入"></a>
#### iterable (ES6规范引入)
- `for ... of`循环遍历集合

```javascript
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    alert(x);
}
for (var x of s) { // 遍历Set
    alert(x);
}
for (var x of m) { // 遍历Map
    alert(x[0] + '=' + x[1]);
}
```
> for ... of循环和for ... in循环有何区别？
> for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性。
当我们手动给Array对象添加了额外的属性后，for ... in循环将带来意想不到的意外效果

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    alert(x); // '0', '1', '2', 'name'
}
```
>for ... in循环将把name包括在内，但Array的length属性却不包括在内。

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    alert(x); //'A', 'B', 'C'
}
```
> for ... of循环则完全修复了这些问题，它只循环集合本身的元素

- `forEach()` 
>更好的方式是直接使用iterable内置的forEach方法，它接收一个函数，每次迭代就自动回调该函数,ES5.1标准引入的

```javascript
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    alert(element);
});

//set没有索引,因此回调函数的前两个参数都是元素本身：
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    alert(element);
});

//Map的回调函数参数依次为value、key和map本身：
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    alert(value);
});

```

<a name="函数"></a>
#### 函数
<a name="函数定义和调用"></a>
##### 函数定义和调用
- 为了避免参数收`undefined`使用`if(typeof arg !== 'number')进行检查`; 少传或不传参数情况，收到参数是undefined，计算结果是NaN.
- arguments 关键字，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。arguments类似Array但它不是一个Array,可以使用`arguments.length` 参数个数，`arguments[0]`获取第一个参数
- 内部函数可以访问外部函数定义的变量，反过来则不行
- JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部，但是不会提升变量的赋值
- 请严格遵守“在函数内部首先申明所有变量”这一规则，最常见的做法是用一个var申明函数内部用到的所有变量
- 全局作用域 ,JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性,例如修改`window.alert = function(){}` alert就不能运行了,`window.alert = old_alert;`回复alert。
>这说明JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报ReferenceError错误。

<a name="-变量的作用域"></a>
#####  变量的作用域
>全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。
减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：

```javascript
// 唯一的全局变量MYAPP:
var MYAPP = {};

// 其他变量:
MYAPP.name = 'myapp';
MYAPP.version = 1.0;

// 其他函数:
MYAPP.foo = function () {
    return 'foo';
};
```
>把自己的代码全部放入唯一的名字空间MYAPP中，会大大减少全局变量冲突的可能。许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。

- 局部作用域

```javascript
//用let取代var的申明，ES6引入
for (let i=0; i<100; i++) {}
```

- 常量

```javascript
//原来的 var PI = 3.14;
const PI = 3.14;
```
- apply
> apply可以改变this值的指向,可以用函数本身的apply方法，它接收两个参数，第一个参数就是需要绑定的this变量，第二个参数是Array。
> `getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空`
- 另一个与apply()类似的方法是call()，唯一区别是：
	- apply()把参数打包成Array再传入；
	- call()把参数按顺序传入。
	- 比如调用Math.max(3, 5, 4)，分别用apply()和call()实现如下（对普通函数调用，我们通常把this绑定为null。）

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```
- 装饰器， 使用apply的特性改成装饰器

```javascript
//假定我们想统计一下代码一共调用了多少次parseInt()
var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};

// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
count; // 3
```
<a name="高阶函数"></a>
##### 高阶函数
> 一个函数可以接受另一个函数当成参数传递进来，叫高阶函数

- map和reduce
	1. map接受的arr当前元素作为参数进行处理(只有一个参数);返回还是一个数组
	2. reduce第一个参数接受之前元素处理后的结果，第二参数接受当前元素;返回是一个计算结果

```javascript
function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
//用map把arr中的元素变成字符串
arr.map(str);

//使用reduce 求和
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25

```
- fileter

```javascript
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

- sort
>sort默认的排序，是按照ASCII码大小写进行排序，如果是int类型的数据，会转换成string类型的进行排序,sort()方法会直接对Array进行修改，它返回的结果仍是当前Array

```javascript
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
}); // [1, 2, 10, 20]
```

<a name="闭包"></a>
##### 闭包
>当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易,返回的函数并没有立刻执行，而是直到调用了f()才执行

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count();
var f1 = results[0];
var f2 = results[1];
var f3 = results[2];
//能认为调用f1()，f2()和f3()结果应该是1，4，9,但结果是都是16
f1(); //16
f2(); //16
f3(); //16
```
>原因：返回的函数引用了变量i，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量i已经变成了4，因此最终结果为16。
__返回函数不要引用任何循环变量，或者后续会发生变化的变量__;如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变

```javascript
function count() {
    var arr = [];
    for (var i=1; i<=3; i++) {
        arr.push((function (n) {
            return function () {
                return n * n;
            }
        })(i));
    }
    return arr;
}
//(function (n) { return n * n }) (3); 定义一个匿名函数并调用
```

```javascript
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
//调用,该闭包携带了局部变量x，并且，从外部代码根本无法访问到变量x, 闭包就是携带状态的函数，并且它的状态可以完全对外隐藏起来。
var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13

//闭包还可以把多参数的函数变成单参数的函数
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n); // x^n
    }
}

// 创建两个新函数:
var pow2 = make_pow(2);
var pow3 = make_pow(3);

pow2(5); // 25
pow3(7); // 343
```
<a name="箭头函数"></a>
##### 箭头函数
ES6标准新增了一种新的函数：Arraw Function（箭头函数）。箭头函数相当于匿名函数，并且简化了函数定义,有的浏览器并不支持。
对this的绑定根据上下文决定

```javascript
var fn = (x) => x * x; //一个参数,省略了 {...} 和return ,多条语句时不能省略
(x, y) => x * x + y * y // 两个参数
() => 3.14 //无参数
//可变参数
(x,y,...rest) => { 
	 var i, sum = x + y;
    for (i=0; i<rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}
```
<a name="generator"></a>
##### generator 
>generator（生成器）是ES6标准引入的新的数据类型。一个generator看上去像一个函数，但可以返回多次;
generator和函数不同的是，generator由function*定义（注意多出的*号），并且，除了return语句，还可以用yield返回多次。

```javascript
//斐波那契数列，使用生成器
function* fib(max){
	var t,a=0,b=1,n=0;
	while(n<max){
		yield a;  //yield 关键字修饰,用next()调用可以得到返回值,done为false
		t = a+b;
		a = b;
		b = t;
		n++;
	}
	return a; //最后一个值 {value: 3, done: true} done值为true
}
fib(5); // fib {[[GeneratorStatus]]: "suspended", [[GeneratorReceiver]]: Window},仅仅是创建了一个generator对象，还没有去执行它。
var f = fib(5);
f.next(); // {value: 0, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 1, done: false}
f.next(); // {value: 2, done: false}
f.next(); // {value: 3, done: true}

//还可以使用for...of来调用,不用自己来判断done值
for(var x of fib(5)){
	console.info(x);
}

```
因为generator可以在执行过程中多次返回，所以它看上去就像一个可以记住执行状态的函数，利用这一点，写一个generator就可以实现需要用面向对象才能实现的功能
generator还有另一个巨大的好处，就是把异步回调代码变成“同步”代码，比如 ajax可以使用。


<a name="标准对象"></a>
#### 标准对象

- 有这么几条规则需要遵守：
	- 不要使用`new Number()`、`new Boolean()`、`new String()`创建包装对象；
	- 用`parseInt()`或`parseFloat()`来转换任意类型到`number`；
	- 用`String()`来转换任意类型到string，或者直接调用某个对象的`toString()`方法；
	- 通常不必把任意类型转换为boolean再判断，因为可以直接写`if (myVar) {...}；`
	- `typeof`操作符可以判断出`number`、`boolean`、`string`、`function`和`undefined`
	- 判断`Array`要使用`Array.isArray(arr)`
	- 判断`null`请使用`myVar === null`；
	- 判断某个全局变量是否存在用`typeof window.myVar === 'undefined'`
	- 函数内部判断某个变量是否存在用`typeof myVar === 'undefined'`
	- `null`和`undefined`是没有`toString()`方法的,虽然`null`还伪装成了`object`类型.
	- `number`对象调用`toString()`报SyntaxError 
		- `123.toString(); // SyntaxError`
		- `123..toString(); // '123', 注意是两个点！(123).toString(); // '123'`

<a name="date"></a>
##### Date
- 当前时间是浏览器从本机操作系统获取的时间，所以不一定准确，因为用户可以把当前时间设定为任何值

```javascript
var now = new Date();
now; // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
now.getFullYear(); // 2015, 年份
now.getMonth(); // 5, 月份，注意月份范围是0~11，5表示六月
now.getDate(); // 24, 表示24号
now.getDay(); // 3, 表示星期三
now.getHours(); // 19, 24小时制
now.getMinutes(); // 49, 分钟
now.getSeconds(); // 22, 秒
now.getMilliseconds(); // 875, 毫秒数
now.getTime(); // 1435146562875, 以number形式表示的时间戳
```

- 指定一个`Date`对象

```javascript
var d = new Date(2015, 5, 19, 20, 15, 30, 123); //月份是从0开始的
d; // Fri Jun 19 2015 20:15:30 GMT+0800 (CST)

//通过解析符合ISO 8601格式的字符串
var d = Date.parse('2015-06-24T19:49:22.875+08:00'); 
d; // 1435146562875 //返回的是个时间戳
var date = new Date(d);
date;  // Wed Jun 24 2015 19:49:22 GMT+0800 (CST)
```

<a name="regexp"></a>
##### RegExp

- JavaScript有两种方式创建一个正则表达式：
	- 第一种方式是直接通过`/正则表达式/`写出来 `var re1 = /ABC\-001/;`
	- 第二种方式是通过`new RegExp('正则表达式')`创建一个RegExp对象  `var re2 = new RegExp('ABC\\-001');`
- 是用`test()`方法进行测试是否符合条件 `re1.test('ABC-001') //true`
- 用`split()`进行字符串分割，空格可以用`/\s+/`来匹配
- 分组,使用`exec()`来获取分组内容,匹配成功返回`Array`,不成返回`null`

```javascript
var re = /^(\d{3})-(\d{3,8})$/ //匹配电话
re.exec('027-12456'); //['027-12456','027','12456']
re.exec('027 12456'); // null
```
- 贪婪匹配,正则匹配默认是贪婪匹配，也就是匹配尽可能多的字符

```javascript
var re = /^(\d+)(0*)$/;
re.exec('102300'); // ['102300', '102300', ''] 
//由于\d+采用贪婪匹配，直接把后面的0全部匹配了，结果0*只能匹配空字符串了。

//必须让\d+采用非贪婪匹配（也就是尽可能少匹配），才能把后面的0匹配出来，加个?就可以让\d+采用非贪婪匹配：
var re = /^(\d+?)(0*)$/;
re.exec('102300'); // ['102300', '1023', '00']
```
- 全局搜索,最常用的是`g`，表示全局匹配 ; `i`表示忽略大小写 ; `m`表示执行多行匹配
>全局匹配可以多次执行exec()方法来搜索一个匹配的字符串。当我们指定g标志后，每次运行exec()，正则表达式本身会更新lastIndex属性，表示上次匹配到的最后索引

```javascript
var s = 'JavaScript, VBScript, JScript and ECMAScript';
var re=/[a-zA-Z]+Script/g; //等价于 var re = new RegExp('[a-zA-Z]+Script','g')

// 使用全局匹配:
re.exec(s); // ['JavaScript']
re.lastIndex; // 10

re.exec(s); // ['VBScript']
re.lastIndex; // 20

re.exec(s); // ['JScript']
re.lastIndex; // 29

re.exec(s); // ['ECMAScript']
re.lastIndex; // 44

re.exec(s); // null，直到结束仍没有匹配到
```

<a name="json"></a>
##### JSON

- 序列化 使用`JSON.stringify()`方法
	- 格式化可以使用 `JSON.stringify(obj,null,' ');`
	- 第二个参数用于控制如何筛选对象的键值，如果我们只想输出指定的属性,可以传入Array`JSON.stringify(obj,['name','age'],' ');`
	- 第二参数还可以传入函数,这样对象的每个键值对都会被函数先处理;`JSON.stringify(obj,function(key,value){... return value;},' ');`
	- 还可以在对象上添加`toJSON`方法,直接返回JSON应该序列化的数据

- 反序列化 使用`JSON.parse()`,转换成javascript对象


<a name="面向对象"></a>
#### 面向对象
>JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。
Object.create()方法可以传入一个原型对象，并创建一个基于该原型的新对象，但是新对象什么属性都没有;不要直接用obj.__proto__去改变一个对象的原型	

<a name="创建对象"></a>
##### 创建对象
>JavaScript对每个创建的对象都会设置一个原型，指向它的原型对象。
当我们用obj.xxx访问一个对象的属性时，JavaScript引擎先在当前对象上查找该属性，如果没有找到，就到其原型对象上找，如果还没有找到，就一直上溯到Object.prototype对象，最后，如果还没有找到，就只能返回undefined

- 创建一个`Array`对象，`var arr=[1,2]` 其原型链是 `arr ----> Array.prototype ----> Object.prototype ----> null`
- 函数也是一个对象，`function foo(){return 0;}` 其原型链是 `foo ----> Function.prototype ----> Object.prototype ----> null`
	- 如果原型链很长，那么访问一个对象的属性就会因为花更多的时间查找而变得更慢，因此要注意不要把原型链搞得太长。
- 构造器方式创建，除了直接用`{ ... }`(原始的创建)创建一个对象外，JavaScript还可以用一种构造函数的方法来创建对象：

```javascript
function Student(name) { //构造函数首字母大写
    this.name = name;
    this.hello = function () { //不共享方法,占用内存多，每个student都有自己的hello方法
        alert('Hello, ' + this.name + '!');
    }
}
//如果不写new，这就是一个普通函数，它返回undefined。但是，如果写了new，它就变成了一个构造函数，
//它绑定的this指向新创建的对象，并默认返回this，也就是说，不需要在最后写return this;。
var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明!
```
- `xiaoming`的原型为：`xiaoming ----> Student.prototype ----> Object.prototype ----> null`
- 用`new Student()`创建的对象还从原型上获得了一个`constructor`属性，它指向函数`Student`本身：

```javascript
xiaoming.constructor === Student.prototype.constructor; // true xiaoming对象是没有prototype属性的，不过可以用__proto__这个非标准用法来查看
Student.prototype.constructor === Student; // true
Object.getPrototypeOf(xiaoming) === Student.prototype; // true
xiaoming instanceof Student; // true
xiaoming.hello === xiaohong.hello; // false
```
- prototype模式

```javascript
function Student(props) {
    this.name = props.name || '匿名'; // 默认值为'匿名'
    this.grade = props.grade || 1; // 默认值为1
}

Student.prototype.hello = function () { //prototype模式共享方法
    alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}

//创建
var xiaoming = createStudent({
    name: '小明'
});
var xiaohong = createStudent({
    name: '小红'
});

xiaoming.hello === xiaohong.hello; // true
Student.prototype.isPrototypeOf(xiaoming);//true 判断某个proptotype对象和某个实例之间的关系
xiaoming.hasOwnProperty("name");//true  每个实例对象都有一个hasOwnProperty()方法，用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性
"name" in xiaoming; //true in运算符可以用来判断，某个实例是否含有某个属性，不管是不是本地属性,还可以用于遍历某个对象的所有属性
```

<a name="继承"></a>
##### 继承 
<a name="构造函数绑定"></a>
[继承详解](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)

<a name="构造函数绑定"></a>
###### 构造函数绑定
第一种方法也是最简单的方法，使用`call`或`apply`方法，将父对象的构造函数绑定在子对象上，即在子对象构造函数中加一行

```javascript
function Animal(){
　　this.species = "动物";
}

function Cat(name,color){
	Animal.apply(this, arguments);
	this.name = name;
	this.color = color;
}
var cat1 = new Cat("大毛","黄色");
alert(cat1.species); // 动物
```


<a name="prototype模式"></a>
###### prototype模式
