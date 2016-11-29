# <img src="http://i.imgur.com/yy1sACZ.png" width="100px"> ECMAScript 6 与 ES5 的等价


*请注意文档还在处理中，欢迎你的贡献.*

**内容:**

1. [箭头函数](#arrow-functions)
1. [块级作用域](#block-scoping-functions)
1. [模板字符串](#template-literals)
1. [属性名称的计算](#computed-property-names)
1. [解构赋值](#destructuring-assignment)
1. [默认参数](#default-parameters)
1. [迭代器和Foro-of](#iterators-and-for-of)
1. [Classes类](#classes)
1. [模块](#modules)
1. [数字字面量](#numeric-literals)
1. [属性方法](#property-method-assignment)
1. [对象快速初始化](#object-initializer-shorthand)
1. [rest参数](#rest-parameters)
1. [延展操作符](#spread-operator)
1. [代理函数对象](#proxying-a-function-object)
1. [关于](#about)
1. [License](#license)

## 箭头函数

与函数表达式相比，箭头函数表达式（也称为胖箭头函数）具有更短的语法，并且以词法的方式绑定此值。 箭头功能总是匿名的。

ES6:

```js
[1, 2, 3].map(n => n * 2);
// -> [ 2, 4, 6 ]
```

ES5 等价:

```js
[1, 2, 3].map(function(n) { return n * 2; }, this);
// -> [ 2, 4, 6 ]
```

ES6:

```js
var evens = [2, 4, 6, 8, 10];

// bodies 表达式
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);

console.log(odds);
// -> [3, 5, 7, 9, 11]

console.log(nums);
// -> [2, 5, 8, 11, 14]

// 声明body
var fives = [];
nums = [1, 2, 5, 15, 25, 32];
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

console.log(fives);
// -> [5, 15, 25]

// this 关键字
var bob = {
  _name: 'Bob',
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + ' knows ' + f));
  }
}
```

ES5:

```js
'use strict';

var evens = [2, 4, 6, 8, 10];

// bodies 表达式 
var odds = evens.map(function (v) {
  return v + 1;
}, this);
var nums = evens.map(function (v, i) {
  return v + i;
}, this);

console.log(odds);
// -> [3, 5, 7, 9, 11]

console.log(nums);
// -> [2, 5, 8, 11, 14]

var fives = [];
nums = [1, 2, 5, 15, 25, 32];

// bodies 声明 
nums.forEach(function (v) {
  if (v % 5 === 0) {
    fives.push(v);
  }
}, this);

console.log(fives);
// -> [5, 15, 25]

// this 关键字
var bob = {
  _name: 'Bob',
  _friends: [],
  printFriends: function printFriends() {
    this._friends.forEach(function (f) {
      return console.log(this._name + ' knows ' + f);
    }, this);
  }
};
```

## 块级作用域

块级作用域绑定提供函数和顶级作用域之外的作用域。 这确保您的变量不会超出它们定义的范围:

ES6:

```js
// let 声明块级作用域的本地变量,
// 在ES6中优先选择它声明变量

'use strict';

var a = 5;
var b = 10;

if (a === 5) {
  let a = 4; // The scope is inside the if-block
  var b = 1; // The scope is inside the function

  console.log(a);  // 4
  console.log(b);  // 1
}

console.log(a); // 5
console.log(b); // 1
```

ES5:

```js
'use strict';

var a = 5;
var b = 10;

if (a === 5) {
  // 在技术上层面上更像是下面
  (function () {
    var a = 4;
    b = 1;

    console.log(a); // 4
    console.log(b); // 1
  })();
}

console.log(a); // 5
console.log(b); // 1
```


ES6:

```js
// const关键字初始化只读常量.
'use strict';
// define favorite as a constant and give it the value 7
// 定义favorite作为常量并赋值为7
const favorite = 7;
// Attempt to overwrite the constant
// 尝试重写常量
try {
  favorite = 15;
} catch (err) {
  console.log('my favorite number is still: ' + favorite);
  // will still print 7
  // 将会打印7
}
```

ES5:

```js
'use strict';
// 定义favorite作为不可写的`常量`并赋值为7
Object.defineProperties(window, {
  favorite: {
    value: 7,
    enumerable: true
  }
});
// ^ 描述符默认是false，常量是可枚举的
var favorite = 7;
// 尝试重写常量
favorite = 15;
// 将会打印7
console.log('my favorite number is still: ' + favorite);
```

## 模板字符串

ES6模板字符串是可以包含<strong>嵌入表达式</ strong>的字符串。 这有时被称为字符串插值。

ES6:

```js
// 基本用法
var person = 'Addy Osmani';
console.log(`Yo! My name is ${person}!`);

// 表达式也可以是对象的属性
var user = {name: 'Caitlin Potter'};
console.log(`Thanks for getting this into V8, ${user.name}.`);

// 表达式插值
var a = 50;
var b = 100;
console.log(`The number of JS frameworks is ${a + b} and not ${2 * a + b}.`);


// 多行不需要换行符 \n\
console.log(`string text line 1
string text line 2`);

// 表达式中的函数
function fn() { return 'result'; }
console.log(`foo ${fn()} bar`);
```

ES5:

```js
'use strict';

// 基本用法
var person = 'Addy Osmani';
console.log('Yo! My name is ' + person + '!');

// 表达式也可以是对象的属性
var user = { name: 'Caitlin Potter' };
console.log('Thanks for getting this into V8, ' + user.name + '.');

// 表达式插值， 一个用途是可读的内联数学
var a = 50;
var b = 100;
console.log('The number of JS frameworks is ' + (a + b) + ' and not ' + (2 * a + b) + '.');

// 多行字符串:
console.log('string text line 1\nstring text line 2');
// 或者:
console.log('string text line 1\n\
string text line 2');

// 表达式中的函数
function fn() {
  return 'result';
}
console.log('foo ' + fn() + ' bar');
```


## 计算属性名

计算属性名允许你指定表达式指定作为属性的名称

ES6:

```js
var prefix = 'foo';
var myObject = {
  [prefix + 'bar']: 'hello',
  [prefix + 'baz']: 'world'
};

console.log(myObject['foobar']);
// -> hello
console.log(myObject['foobaz']);
// -> world
```

ES5:

```js
'use strict';

var prefix = 'foo';
var myObject = {};

myObject[prefix + 'bar'] = 'hello';
myObject[prefix + 'baz'] = 'world';

console.log(myObject['foobar']);
// -> hello
console.log(myObject['foobaz']);
// -> world
```

## 解构赋值

解构赋值语法是一个JavaScript表达式，可以使用镜像数组和对象属性值构造的语法从数组或对象中提取数据。


ES6:

```js
var {foo, bar} = {foo: 'lorem', bar: 'ipsum'};
// foo => lorem and bar => ipsum
```

ES5:

```js
'use strict';

var _ref = { foo: 'lorem', bar: 'ipsum' };

// foo => lorem and bar => ipsum
var foo = _ref.foo;
var bar = _ref.bar;
```

ES3:

```js
with({foo: 'lorem', bar: 'ipsum'}) {
  // foo => lorem and bar => ipsum
}
```

ES6:

```js
var [a, , b] = [1,2,3];
```

ES6 (shimming using `Symbol.iterator`):

```js
'use strict';

var _slicedToArray = function (arr, i) {
  if (Array.isArray(arr)) {
    return arr;
  } else {
    var _arr = [];

    for (var _iterator = arr[Symbol.iterator](), _step; !(_step = _iterator.next()).done;) {
      _arr.push(_step.value);

      if (i && _arr.length === i) {
        break;
      }
    }

    return _arr;
  }
};

var _ref = [1, 2, 3];

var _ref2 = _slicedToArray(_ref, 3);

var a = _ref2[0];
var b = _ref2[2];
```

ES5:

```js
String.prototype.asNamedList = function () {
  return this.split(/\s*,\s*/).map(function (name, i) {
    return name ? ('var ' + name + '=slice(' + i + ', ' + (i + 1) + ')[0]') : '';
  }).join(';');
};

with([1,2,3]) {
  eval('a, , b'.asNamedList());
}
```


## 默认参数

默认参数允许你的函数有可选的参数，而不需要检查arguments.length或检查undefined。

ES6:

```js
function greet(msg='hello', name='world') {
  console.log(msg,name);
}

greet();
// -> hello world
greet('hey');
// -> hey world
```

ES5:

```js
'use strict';

function greet() {
  // 如果你访问arguments[0]，像这样你可以很简单的代替使用变量名访问 
  var msg = arguments[0] === undefined ? 'hello' : arguments[0];
  var name = arguments[1] === undefined ? 'world' : arguments[1];
  console.log(msg, name);
}

function greet(msg, name) {
  (msg === undefined) && (msg = 'hello');
  (name === undefined) && (name = 'world');
  console.log(msg,name);
}

// 使用检查未定义的基本实用程序
function greet(msg, name) {
  console.log(
    defaults(msg, 'hello'),
    defaults(name, 'world')
  );
}

greet();
// -> hello world
greet('hey');
// -> hey world
```

ES6:

```js
function f(x, y=12) {
  // y 是 12 如果没有传递 (将会作为undefined传递)
  return x + y;
}

f(3) === 15;
```

ES5:

```js
'use strict';

function f(x, y) {
  if (y === undefined) {
    y = 12;
  }

  return x + y;
}

f(3) === 15;
```

## 迭代器与For-of

迭代器是可以遍历容器的对象，它是使一个类工作在一个for循环很有用的方式。
接口类似于iterators-interface，迭代器与 `for..of`看起来很相像 ：

ES6:

```js
// 下面，这将会从数组中获取迭代器并循环遍历取值。
for (let element of [1, 2, 3]) {
  console.log(element);
}
// => 1 2 3
```

ES6 (如果支持`Symbol`，可以不使用`for-of`):

```js
'use strict';

for (var _iterator = [1, 2, 3][Symbol.iterator](), _step; !(_step = _iterator.next()).done;) {
  var element = _step.value;
  console.log(element);
}

// => 1 2 3
```

ES5 (approximates):

```js

// 使用 forEach（）不需要在作用域范围内声明索引和元素变量，它们作为参数传递给迭代器，并且作用域只是这次迭代。

var a = [1,2,3];
a.forEach(function (element) {
    console.log(element);
});

// => 1 2 3

// 使用for遍历
var a = [1,2,3];
for (var i = 0; i < a.length; ++i) {
    console.log(a[i]);
}
// => 1 2 3
```

注意ES5中使用`Symbol`， 需要Symbol polyfill才能正确运行。

## Classes

This implements class syntax and semantics as described in the ES6 draft spec. Classes are a great way to reuse code.
Several JS libraries provide classes and inheritance, but they aren't mutually compatible.

ES6草案规范中描述的类语法和语义，class是实现重用代码的好方法，有几个JS库提供类和继承，但它们不是相互兼容的。

ES6:

```js
class Hello {
  constructor(name) {
    this.name = name;
  }

  hello() {
    return 'Hello ' + this.name + '!';
  }

  static sayHelloAll() {
    return 'Hello everyone!';
  }
}

class HelloWorld extends Hello {
  constructor() {
    super('World');
  }

  echo() {
    alert(super.hello());
  }
}

var hw = new HelloWorld();
hw.echo();

alert(Hello.sayHelloAll());
```

ES5 (approximate):

```js
function Hello(name) {
  this.name = name;
}

Hello.prototype.hello = function hello() {
  return 'Hello ' + this.name + '!';
};

Hello.sayHelloAll = function () {
  return 'Hello everyone!';
};

function HelloWorld() {
  Hello.call(this, 'World');
}

HelloWorld.prototype = Object.create(Hello.prototype);

HelloWorld.prototype.echo = function echo() {
  alert(Hello.prototype.hello.call(this));
};

var hw = new HelloWorld();
hw.echo();

alert(Hello.sayHelloAll());
```

可以在[Babel](http://goo.gl/573aDy)中找到更加详尽的解释
## Modules

Modules are mostly implemented, with some parts of the Loader API still to be corrected. Modules try to solve many issues in dependencies and deployment, allowing users to create modules with explicit exports, import specific exported names from those modules, and keep these names separate.

*Assumes an environment using CommonJS*

模块已经实现大部分功能，Loader API的一些部分仍需要更正。 模块尝试解决依赖和部署中的问题，允许用户使用显式导出已创建模块

*假设在使用CommonJS 环境*


app.js - ES6

```js
import math from 'lib/math';
console.log('2π = ' + math.sum(math.pi, math.pi));
```

app.js - ES5

```js
var math = require('lib/math');
console.log('2π = ' + math.sum(math.pi, math.pi));
```

lib/math.js - ES6

```js
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;
```

lib/math/js - ES5

```js
exports.sum = sum;
function sum(x, y) {
  return x + y;
}
var pi = exports.pi = 3.141593;
```

lib/mathplusplus.js - ES6

```js
export * from 'lib/math';
export var e = 2.71828182846;
export default function(x) {
  return Math.exp(x);
}
```

lib/mathplusplus.js - ES5

```js
var Math = require('lib/math');

var _extends = function (target) {
  for (var i = 1; i < arguments.length; i++) {
    var source = arguments[i];
    for (var key in source) {
      target[key] = source[key];
    }
  }

  return target;
};

var e = exports.e = 2.71828182846;
exports['default'] = function (x) {
  return Math.exp(x);
};

module.exports = _extends(exports['default'], exports);
```

## 数字字面量

ES6:

```js
var binary = [
  0b0,
  0b1,
  0b11
];
console.assert(binary === [0, 1, 3]);

var octal = [
  0o0,
  0o1,
  0o10,
  0o77
];
console.assert(octal === [0, 1, 8, 63]);
```

ES5:

```js
'use strict';

var binary = [0, 1, 3];
console.assert(binary === [0, 1, 3]);

var octal = [0, 1, 8, 63];
console.assert(octal === [0, 1, 8, 63]);
```

## 方法属性
对象初始化器中支持Method语法，例如toString（）：

ES6:

```js
var object = {
  value: 42,
  toString() {
    return this.value;
  }
};

console.log(object.toString() === 42);
// -> true
```

ES5:

```js
'use strict';

var object = {
  value: 42,
  toString: function toString() {
    return this.value;
  }
};

console.log(object.toString() === 42);
// -> true
```

## 对象初始化快捷方式

省略重复的属性名和属性值在字面量的对象中
ES6:

```js
function getPoint() {
  var x = 1;
  var y = 10;

  return {x, y};
}

console.log(getPoint() === {
  x: 1,
  y: 10
});
```

ES5:

```js
'use strict';

function getPoint() {
  var x = 1;
  var y = 10;

  return { x: x, y: y };
}

console.log(getPoint() === {
  x: 1,
  y: 10
});
```

## Rest 参数

Rest参数让函数具有可变数量的参数，在不使用arguments对象情况下。
rest参数是一个Array的实例，因此在rest参数使用数组函数也可以正常工作。

ES6:

```js
function f(x, ...y) {
  // y是数组
  return x * y.length;
}

console.log(f(3, 'hello', true) === 6);
// -> true
```

ES5:

```js
'use strict';

function f(x) {
  var y = [];
  y.push.apply(y, arguments) && y.shift();

  // y是数组
  return x * y.length;
}

console.log(f(3, 'hello', true) === 6);
// -> true
```

## Spread Operator

延展操作符与Rest参数相反，它允许您将数组扩展为多个参数形式。

ES6:

```js
function add(a, b) {
  return a + b;
}

let nums = [5, 4];

console.log(add(...nums));
```

ES5:

```js
'use strict';

var _toArray = function (arr) {
  return Array.isArray(arr) ? arr : [].slice.call(arr);
};

function add(a, b) {
  return a + b;
}

var nums = [5, 4];
console.log(add.apply(null, _toArray(nums)));
```

ES6:

```js
function f(x, y, z) {
  return x + y + z;
}
// 作为参数传递每个数组元素
f(...[1,2,3]) === 6;
```

ES5:

```js
'use strict';

function f(x, y, z) {
  return x + y + z;
}

// 作为参数传递每个数组元素
f.apply(null, [1, 2, 3]) === 6;
```

## 函数对象代理

ES6:

```js
var target = function () {
  return 'I am the target';
};

var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
console.log(p() === 'I am the proxy');
// -> true
```

ES5:

在ES5没有proxy

## 关于

灵感来自:

* [ES6 Feature Proposals](http://tc39wiki.calculist.org/es6/)
* [ES6 Features](https://github.com/lukehoban/es6features)
* [ECMAScript 6 support in Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla)
* [Babel](https://babeljs.io)
* [JS Rocks](http://jsrocks.org/)


## License

This work is licensed under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0/) License.
