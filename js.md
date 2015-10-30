

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
		- [高阶函数](#高阶函数)

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
- 为了避免参数收`undefined`使用`if(typeof arg !== 'number')进行检查`; 少传或不传参数情况，收到参数是undefined，计算结果是NaN.
- arguments 关键字，它只在函数内部起作用，并且永远指向当前函数的调用者传入的所有参数。arguments类似Array但它不是一个Array,可以使用`arguments.length` 参数个数，`arguments[0]`获取第一个参数
- 内部函数可以访问外部函数定义的变量，反过来则不行
- JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部，但是不会提升变量的赋值
- 请严格遵守“在函数内部首先申明所有变量”这一规则，最常见的做法是用一个var申明函数内部用到的所有变量
- 全局作用域 ,JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性,例如修改`window.alert = function(){}` alert就不能运行了,`window.alert = old_alert;`回复alert。
>这说明JavaScript实际上只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，则报ReferenceError错误。

- 名字空间
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
