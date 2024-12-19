JavaScript 中的 => 是箭头函数 (arrow function) 的语法，提供了一种更简洁的方式来定义函数。它与传统的 function 关键字定义的函数存在一些重要的区别。
1. 基本语法
箭头函数语法：


`const add = (a, b) => a + b;`
传统函数语法：

javascript
复制代码
function add(a, b) {
  return a + b;
}
2. 区别
(1) this 绑定行为
箭头函数中的 this 是从外部上下文继承的，并不会根据调用方式动态绑定。
普通函数的 this 是动态绑定的，取决于函数的调用方式。
示例：

javascript
复制代码
const obj = {
  value: 10,
  arrowFn: () => console.log(this.value), // `this` 指向定义时的上下文
  normalFn: function () {
    console.log(this.value); // `this` 指向调用时的上下文
  },
};

obj.arrowFn(); // undefined，因为 `this` 在定义时是全局对象（或 undefined 在严格模式下）
obj.normalFn(); // 10，因为 `this` 指向 obj
(2) 语法简洁
箭头函数对于简单的表达式和无代码块的函数更加简洁：
javascript
复制代码
const square = (x) => x * x; // 单行返回
const greet = () => console.log("Hello"); // 无参数
普通函数需要明确使用 return 和花括号：
javascript
复制代码
function square(x) {
  return x * x;
}
function greet() {
  console.log("Hello");
}
(3) 没有 arguments 对象
箭头函数没有自己的 arguments 对象。如果需要访问参数，可以使用剩余参数 ...args。
普通函数可以直接访问 arguments。
示例：

javascript
复制代码
function normalFn() {
  console.log(arguments);
}

const arrowFn = (...args) => {
  console.log(args); // 使用剩余参数代替 arguments
};

normalFn(1, 2, 3); // [1, 2, 3]
arrowFn(1, 2, 3); // [1, 2, 3]
(4) 不能作为构造函数
箭头函数不能被用作构造函数，尝试用 new 调用会报错。
普通函数可以通过 new 操作符实例化对象。
示例：

javascript
复制代码
function NormalFn() {
  this.value = 10;
}
const ArrowFn = () => {
  this.value = 10;
};

const obj1 = new NormalFn(); // 正常
const obj2 = new ArrowFn(); // TypeError: ArrowFn is not a constructor
(5) 没有原型
箭头函数没有 prototype 属性。
普通函数有 prototype 属性。
示例：

javascript
复制代码
function normalFn() {}
const arrowFn = () => {};

console.log(normalFn.prototype); // { constructor: f }
console.log(arrowFn.prototype); // undefined
(6) 适合回调函数
箭头函数适用于需要保持外部 this 的场景，例如回调函数。
示例：

javascript
复制代码
class Timer {
  start() {
    this.time = 0;
    setInterval(() => {
      this.time++; // 箭头函数继承了 Timer 的 `this`
      console.log(this.time);
    }, 1000);
  }
}

const timer = new Timer();
timer.start();
普通函数在这种场景中需要手动绑定 this：

javascript
复制代码
class Timer {
  start() {
    this.time = 0;
    setInterval(function () {
      this.time++; // 错误的 `this`
      console.log(this.time);
    }.bind(this), 1000); // 手动绑定 `this`
  }
}

const timer = new Timer();
timer.start();
