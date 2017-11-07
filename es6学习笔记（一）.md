# es6学习笔记（一）

```
2017.7.25

es6:第一章

let命令和const命令
let命令：
   1.let 只在所在的代码块内有效。

   2.let 不像 var 那样会发生“变量提升”现象。所以，变量一定要在声明后使用，否则报错。

   3.只要块级作用域内存在 let 命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影
     响。ES6 明确规定，如果区块中存在 let 和 const 命令，这个区块对这些命令声明的变量，从一开始就形成
     了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

   4.let 不允许在相同作用域内，重复声明同一个变量。

   ps:(ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明)
   5. •  允许在块级作用域内声明函数。
      •  函数声明类似于 var ，即会提升到全局作用域或函数作用域的头部。
      •  同时，函数声明还会提升到所在的块级作用域的头部。
       注意，上面三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作
       let 处理。


const 命令：
1.const 声明一个只读的常量。一旦声明，常量的值就不能改变。

2.对于 const 来说，只声明不赋值，就会报错。

3.const 的作用域与 let 命令相同：只在声明所在的块级作用域内有效。

4.const 命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

5.const 声明的常量，也与 let 一样不可重复声明。

6.对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。 const 命令只是保证变量名指

向的地址不变，并不保证该地址的数据不变，所以将一个对象声明为常量必须非常小心。
7.如果真的想将对象冻结，应该使用 Object.freeze 方法。
8.为了保持兼容性， var 命令和 function 命令声明的全局变量，依旧是全
局对象的属性；另一方面规定， let 命令、 const 命令、 class 命令声明的全局变量，不属于全局对象的属性。
也就是说，从 ES6 开始，全局变量将逐步与全局对象的属性脱钩。


2017.8.7
es6:第二章
1. 数组的解构赋值

var [a, b, c] = [1, 2, 3];
上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]
let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
如果解构不成功，变量的值就等于 undefined 。
等号右边如果不是数组形式，则会报错。
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};

2.对象的解构赋值
// 错误的写法
var x;
{x} = {x: 1};
// SyntaxError: syntax error

// 正确的写法
({x} = {x: 1});

3. 字符串的解构赋值
字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

类似数组的对象都有一个 length 属性，因此还可以对这个属性解构赋值。
let {length : len} = 'hello';
len // 5

4. 数值和布尔值的解构赋值
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
let {toString: s} = 123;
s === Number.prototype.toString // true
let {toString: s} = true;
s === Boolean.prototype.toString // true
上面代码中，数值和布尔值的包装对象都有 toString 属性，因此变量 s 都能取到值。
解构赋值的规则是，只要等号右边的值不是对象，就先将其转为对象。由于 undefined 和 null 无法转为对象，
所以对它们进行解构赋值，都会报错。
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError

ps:=> = function（）{} 匿名函数的意思，
data.map(items,function(){
  
})
遍历数组的函数，有回调函数


6. 圆括号问题

解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办
法从一开始就知道，必须解析到（或解析不到）等号才能知道。
由此带来的问题是，如果模式中出现圆括号怎么处理。ES6的规则是，只要有可能导致解构的歧义，就不得使
用圆括号。
但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆
括号。

以下三种解构赋值不得使用圆括号。
（1）变量声明语句中，模式不能带有圆括号。
// 全部报错
var [(a)] = [1];
var { x: (c) } = {};
var { o: ({ p: p }) } = { o: { p: 2 } };
上面三个语句都会报错，因为它们都是变量声明语句，模式不能使用圆括号。
（2）函数参数中，模式不能带有圆括号。
函数参数也属于变量声明，因此不能带有圆括号。
// 报错
function f([(z)]) { return z; }
（3）不能将整个模式，或嵌套模式中的一层，放在圆括号之中。
// 全部报错
({ p: a }) = { p: 42 };
([a]) = [5];
上面代码将整个模式放在模式之中，导致报错。
// 报错
[({ p: a }), { x: c }] = [{}, {}];
上面代码将嵌套模式的一层，放在圆括号之中，导致报错。

可以使用圆括号的情况
可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号。
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
上面三行语句都可以正确执行，因为首先它们都是赋值语句，而不是声明语句；其次它们的圆括号都不属于模
式的一部分。第一行语句中，模式是取数组的第一个成员，跟圆括号无关；第二行语句中，模式是p，而不是
d；第三行语句与第一行语句的性质一致。

第三章：字符串的扩展

1. 字符的Unicode 表示法
js允许采用\uxxxx形式表示一个字符，但是只限于\u0000--\uFFFF之间的字符。超出
这个范围的字符，必须用两个双字节的形式表达。但是es6则可用大括号表示法。只要
将只要将码点放入大括号，就能正确解读该字符。
2.codePointAt()
，能够正确处理4个字节储存的字符，返回一个字符的码点。

var s = '₻a';
s.codePointAt(0) // 134071
s.codePointAt(1) // 57271
s.charCodeAt(2) // 97
codePointAt 方法会正确返回32位的UTF-16字符的码点。对于那些两个字节储存的常规字符，它的返
回结果与 charCodeAt 方法相同。

codePointAt 方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。

3. String.fromCodePoint()

ES5提供 String.fromCharCode 方法，用于从码点返回对应字符，但是这个方法不能识别32位的UTF-16字符
（Unicode编号大于 0xFFFF ）。
 String.fromCodePoint 方法，可以识别 0xFFFF 的字符，弥补了 String.fromCharCode 方法的不
足。在作用上，正好与 codePointAt 方法相反。
注意， fromCodePoint 方法定义在 String 对象上，而 codePointAt 方法定义在字符串的实例对象上。

4. 字符串的遍历器接口 for...of

除了遍历字符串，这个遍历器最大的优点是可以识别大于 0xFFFF 的码点，传统的 for 循环无法识别这样的码
点。
var text = String.fromCodePoint(0x20BB7);
for (let i = 0; i < text.length; i++) {
console.log(text[i]);
字符串的扩展 - ECMAScript 6入门
http://es6.ruanyifeng.com/#docs/string
}
// " "
// " "
for (let i of text) {
console.log(i);
}
// "吉"

5. at()
ES5对字符串对象提供 charAt 方法，返回字符串给定位置的字符。该方法不能识别码点大于 0xFFFF 的字符。
'abc'.charAt(0) // "a"
'₻'.charAt(0) // "\uD842"
上面代码中， charAt 方法返回的是UTF-16编码的第一个字节，实际上是无法显示的。
ES7提供了字符串实例的 at 方法，可以识别Unicode编号大于 0xFFFF 的字符，返回正确的字符。Chrome浏览
器已经支持该方法。
'abc'.at(0) // "a"
'₻'.at(0) // "₻"
6. normalize()
ES6提供字符串实例的 normalize() 方法，用来将字符的不同表示方法统一为同样的形式，这称为Unicode正
规化。
'\u01D1'.normalize() === '\u004F\u030C'.normalize()
// true
字符串的扩展 - ECMAScript 6入门
http://es6.ruanyifeng.com/#docs/string
normalize 方法可以接受四个参数。
NFC ，默认参数，表示“标准等价合成”（Normalization Form Canonical Composition），返回多
个简单字符的合成字符。所谓“标准等价”指的是视觉和语义上的等价。
NFD ，表示“标准等价分解”（Normalization Form Canonical Decomposition），即在标准等价的
前提下，返回合成字符分解的多个简单字符。
NFKC ，表示“兼容等价合成”（Normalization Form Compatibility Composition），返回合成字
符。所谓“兼容等价”指的是语义上存在等价，但视觉上不等价，比如“囍”和“喜喜”。（这只是用来举
例， normalize 方法不能识别中文。）
NFKD ，表示“兼容等价分解”（Normalization Form Compatibility Decomposition），即在兼容等
价的前提下，返回合成字符分解的多个简单字符。
'\u004F\u030C'.normalize('NFC').length // 1
'\u004F\u030C'.normalize('NFD').length // 2
上面代码表示， NFC 参数返回字符的合成形式， NFD 参数返回字符的分解形式。
不过， normalize 方法目前不能识别三个或三个以上字符的合成。这种情况下，还是只能使用正则表达式，通
过Unicode编号区间判断。

7. includes(), startsWith(), endsWith()
传统上，JavaScript只有 indexOf 方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了
三种新方法。
includes()：返回布尔值，表示是否找到了参数字符串。
startsWith()：返回布尔值，表示参数字符串是否在源字符串的头部。
endsWith()：返回布尔值，表示参数字符串是否在源字符串的尾部。

这三个方法都支持第二个参数，表示开始搜索的位置。
var s = 'Hello world!';
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
上面代码表示，使用第二个参数 n 时， endsWith 的行为与其他两个方法有所不同。它针对前 n 个字符，而其他
两个方法针对从第 n 个位置直到字符串结束。

8. repeat()
repeat 方法返回一个新字符串，表示将原字符串重复 n 次。
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
参数如果是小数，会被取整。
'na'.repeat(2.9) // "nana"
如果 repeat 的参数是负数或者 Infinity ，会报错。

9. padStart()，padEnd()
ES7推出了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。 padStart 用于头部
补全， padEnd 用于尾部补全。
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
上面代码中， padStart 和 padEnd 一共接受两个参数，第一个参数用来指定字符串的最小长度，第二个参数是
用来补全的字符串。
如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。

10. 模板字符串
$("#result").append(`
There are <b>${basket.count}</b> items
in your basket, <em>${basket.onSale}</em>
are on sale!
`);
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可
以用来定义多行字符串，或者在字符串中嵌入变量。

如果需要引用模板字符串本身，可以像下面这样写。
// 写法一
let str = 'return ' + '`Hello ${name}!`';
let func = new Function('name', str);
func('Jack') // "Hello Jack!"
// 写法二
let str = '(name) => `Hello ${name}!`';
let func = eval.call(null, str);
func('Jack') // "Hello Jack!"

11. 实例：模板编译
下面，我们来看一个通过模板字符串，生成正式模板的实例。
var template = `
<ul>
<% for(var i=0; i < data.supplies.length; i++) {%>
<li><%= data.supplies[i] %></li>
<% } %>
</ul>
`;
上面代码在模板字符串之中，放置了一个常规模板。该模板使用 <%...%> 放置JavaScript代码，使用 <%= ...
%> 输出JavaScript表达式。

12. 标签模板
模板字符串的功能，不仅仅是上面这些。它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符
字符串的扩展 - ECMAScript 6入门
http://es6.ruanyifeng.com/#docs/string
串。这被称为“标签模板”功能（tagged template）。
标签模板其实不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数，紧跟在后面的模板字符串就是
它的参数。
var a = 5;
var b = 10;
tag`Hello ${ a + b } world ${ a * b }`;
上面代码中，模板字符串前面有一个标识名 tag ，它是一个函数。整个表达式的返回值，就是 tag 函数处理模
板字符串后的返回值。
函数 tag 依次会接收到多个参数。

13. String.raw()
ES6还为原生的String对象，提供了一个 raw 方法。
String.raw 方法，往往用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）
的字符串，对应于替换变量后的模板字符串。
String.raw`Hi\n${2+3}!`;
// "Hi\\n5!"
String.raw`Hi\u000A!`;
// 'Hi\\u000A!'
如果原字符串的斜杠已经转义，那么 String.raw 不会做任何处理。
```