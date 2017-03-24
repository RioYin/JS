<h1>ES6常用新特性</h1>

<h2>let, const声明变量</h2>

* ES6之前只有全局作用域和函数作用域，没有块级作用域，带来很多不合理的场景。

* let为JS新增了块级作用域，用它所声明的变量，只在let命令所在的代码块内有效。

  * 下面代码中，变量i是var声明的，在全局范围内都有效。所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮的i的值

```
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

* 
  * 而使用let则不会出现这个问题。
  
```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

* const用来声明常量，一旦声明，常量的值就不能改变，当尝试改变用const声明的常量时，浏览器会报错，如：`const PI = Math.PI;  PI = 23`会报错

* const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug，如：`const monent = require('moment')`

<h2>class, extends, super实现继承</h2>

```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello
```

* 上面代码首先用class定义了一个类，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实例对象可以共享的。

* 类之间可以通过extends关键字实现继承，上面定义了一个Cat类，该类通过extends关键字，继承了Animal类的所有属性和方法。

* super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

* ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。

<h2>箭头“=>”简写函数</h2>

* 简单情况

```
function(i){ return i + 1; } //ES5
(i) => i + 1 //ES6
```

* 复杂情况,需要用`{}`把代码包起来

```
function(x, y) { 
    x++;
    y--;
    return x + y;
}  //ES5
(x, y) => {x++; y--; return x+y}  //ES6
```

* 特殊举例

```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout(function(){
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

 var animal = new Animal()
 animal.says('hi')  //undefined says hi
```

运行上面的代码会报错，这是因为setTimeout中的this指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：

* 第一种是将this传给self,再用self来指代this

```
 says(say){
     var self = this;
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }, 1000)
```

* 第二种方法是用`bind(this)`,即

```
 says(say){
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }.bind(this), 1000)
```

* 用箭头函数`=>`

```
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi
 
 //当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。
```

<h2>用一对反引号``和${}插入html内容</h2>

* 传统方法

```
$("#result").append(
  "There are <b>" + basket.count + "</b> " +
  "items in your basket, " +
  "<em>" + basket.onSale +
  "</em> are on sale!"
);
```

* 用反引号``和${}

```
$("#result").append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);

//用反引号`来标识起始，用${}来引用变量，而且所有的空格和缩进都会被保留在输出之中
```

<h2>扩展运算符`...`操作数组</h2>

* 拷贝数组

```
//传统方法：
const len = items.length;
const itemsCopy = [];

for (let i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}


//ES6写法：
const itemsCopy = [...items];
```

* 取代伪数组

```
//传统方法：
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}


//ES6写法：
function concatenateAll(...args) {
  return args.join('');
}
```

http://www.jianshu.com/p/ebfeb687eb70
