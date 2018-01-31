---
title: JavaScript-Modules
date: 2018-02-01 00:04:05
tags:
---
JavaScript-Modules
==================

1.  [模块化](#模块化)
2.  [为什么要进行模块化](#为什么要进行模块化)
3.  [JS模块化的发展过程（从function，IIFE到Commonjs）](#JS模块化的发展过程（从function，IIFE到Commonjs）)
    1.  [Function](#Function)
    2.  [IIFE](#IIFE)
4.  [放大模式](#放大模式)
5.  [宽放大模式（Loose augmentation）](#宽放大模式（Loose-augmentation）)
6.  [紧扩充模式](#紧扩充模式)
7.  [输入全局变量](#输入全局变量)
8.  [Commonjs](#Commonjs)
9.  [在浏览器中使用 CommonJS](#在浏览器中使用-CommonJS)
10.  [AMD](#AMD)
11.  [总结：](#总结：)
    1.  [模块接口的设设计原则：](#模块接口的设设计原则：)
12.  [ECMAScript 6 Modules](#ECMAScript-6-Modules)
    1.  [export multiply;](#export-multiply)
    2.  [import:](#import)

### [](#模块化 "模块化")模块化

当我们称一个应用程序是模块化的的时候，我们通常是指它由一组高度解耦的、存放在不同模块中的独特功能构成。你可能已经知道，松散耦合通过尽可能地去除依赖性来让可维护性更加简单易得。当这一点被有效实现的时候，系统中某一部分的变化将如何影响其它部分就会变得显而易见。

JavaScript模块化是将JS代码按照功能的不同分成高度解耦的代码块，存放在模块中。与一些更传统的编程语言不同的是，JavaScript 6以前的版本并没有为开发者们提供以一种简洁、有条理地的方式来引入模块的方法。

### [](#为什么要进行模块化 "为什么要进行模块化")为什么要进行模块化

1.解决命名冲突问题  
在传统的JavaScript语言中，位于全局作用域中的变量可以在任何地方被访问，然而随着网站逐渐变成”互联网应用程序”，嵌入网页的Javascript代码越来越庞大，越来越复杂。JS这种共享全局作用域的做法慢慢显现出可能带来命名冲突的弊端。

2.代码复用  
在一个大型的互联网应用程序中，很多不同功能会用到相同的代码，在这样的情况下，如果不使用模块化，我们的解决方案将是复制这段代码，然后粘贴到需要这段代码的地方。如果我们的这段代码出现了错误或者我们想要修改这段代码，必须找到所有使用这段代码的地方逐一修改。然而一旦我们引入了模块思想，这种问题就会迎刃而解。我们只需要将这段代码封装成一个模块，在需要使用它的地方引入这个模块，并且只用在模块中修改。  
代码复用的另一个方面是指你的代码也可以被别人使用，或者你也可以使用别人的代码。理想情况下，开发者只需要实现核心的业务逻辑，其他都可以加载别人已经写好的模块。

3.去耦合  
模块化的一个很明显的，它可以将一段有特定功能的代码独立出来。这样可以一定程度上减小代码之间的耦合性，当一个页面应用需要的某些功能需要更新时，只要更新相关的模块就可以了，这些改动不会对其他的模块带来影响。同样，一个程序中的某一个模块的改动也不会影响这个模块的主要功能。

4.保持模块内部的私有性  
模块化需要给模块定义一个接口，主程序或者其它模块通过这个接口来调用模块，模块的接口是对外开放的，但是模块内部的代码不能被模块外部访问。这就保证了模块内部变量的私有性。

### [](#JS模块化的发展过程（从function，IIFE到Commonjs） "JS模块化的发展过程（从function，IIFE到Commonjs）")JS模块化的发展过程（从function，IIFE到Commonjs）

虽然Javascript模块化编程，已经成为一个迫切的需求。但是，ECMAScript标准第六版出现之前，Javascript不是一种模块化编程语言，它不支持”类”（class），更遑论”模块”（module）了。（ECMAScript标准第六版，正式支持”类”和”模块”，但还需要很长时间才能投入实用。）

Javascript社区做了很多努力，在现有的运行环境中，实现”模块”的效果。这一部分将总结当前＂Javascript模块化编程＂的最佳实践。

#### [](#Function "Function")Function

模块就是实现特定功能的一组方法。

只要把不同的函数（以及记录状态的变量）简单地放在一起，就算是一个模块。

函数是JavaScript中唯一可以创建新的作用域的类型，我们可以利用函数来为模块创建作用域。  
例如：

1

2

3

4

5

6

7

var dayName=function(){

    var names=\["Sunday","Monday","Tuesday","Wedensday","Thursday","Friday","Saturday"\];

    return function(number){

        return names\[number\];

    };

}();

console.log(dayName(1));

这个例子通过将一个匿名函数的返回值赋给一个变量（dayName），返回值也是一个函数，也是这个模块想暴露到模块外的部分，这样变量dayName函数就变成了模块的接口。

现在假如我们需要在这个模块中再加入一个函数，并且想把这个函数也作为模块接口，此时我们需要返回两个函数，解决方案是将两个函数封装在一个对象中，然后返回这个对象作为模块的接口。  
代码如下：

1

2

3

4

5

6

7

8

9

10

11

12

var weekDay = function() {

        var names = \["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"\];

        return {

            name: function(number){

                return names\[number\];

            },

            number: function(name){

                return names.indexOf(name);

            }

        }

    }();

console.log(weekDay.number("Sunday"));

但是这种做法的一个很明显显的缺点是当返回部分很大时return语句要包裹很多代码，我们可以使用一个立即执行函数来解决这个问题。下面将对此进行介绍。

#### [](#IIFE "IIFE")IIFE

JavaScript语法有一个怪癖，如果一个函数以function开始，它将会是一个函数声明，函数声明必须有一个函数名。而如果一个函数不以function开始，它将是一个函数表达式。我们可以使用一个括号强制将函数转换为  
函数表达式。

上面的函数可以改写成如下形式：

1

2

3

4

5

6

7

8

9

10

(function(exports){

    var names = \["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"\];

    exports.name = function(number) {

        return names\[number\];

    }

    exports.number = function(name) {

        return names.indexOf\[name\];

    };

})(this.weekDay = {});

console.log(weekDay.name(1));

上面的代码块中，我们使用一个对象作为函数的参数，将要输出的接口作为对象的属性，这样我们就可以使用这个对象调用接口。

### [](#放大模式 "放大模式")放大模式

如果一个模块很大，必须分成几个部分，或者一个模块需要继承另一个模块，这时就有必要采用”放大模式”（augmentation）。

1

2

3

4

5

6

　　var module1 = (function (mod){

　　　　mod.m3 = function () {

　　　　　　//...

　　　　};

　　　　return mod;

　　})(module1);

### [](#宽放大模式（Loose-augmentation） "宽放大模式（Loose augmentation）")宽放大模式（Loose augmentation）

在浏览器环境中，模块的各个部分通常都是从网上获取的，有时无法知道哪个部分会先加载。如果采用上一节的写法，第一个执行的部分有可能加载一个不存在空对象，这时就要采用”宽放大模式”。

1

2

3

4

　　var module1 = ( function (mod){

　　　　//...

　　　　return mod;

　　})(window.module1 || {});

与”放大模式”相比，＂宽放大模式＂就是”立即执行函数”的参数可以是空对象。

上面的代码为module1模块添加了一个新方法m3()，然后返回新的module1模块。

### [](#紧扩充模式 "紧扩充模式")紧扩充模式

宽放大模式非常棒，但是有一个缺点是对无法安全地处理方法属性的重载。紧扩充模式保持对旧有方法的的引用，在定义的新方法中可以灵活地重用旧有方法的功能。

1

2

3

4

5

6

7

var MODULE = (function (my) {

    var old_moduleMethod = my.moduleMethod;

    my.moduleMethod = function () {

        // method override, has access to old through old_moduleMethod...

    };

    return my;

}(MODULE));

### [](#输入全局变量 "输入全局变量")输入全局变量

独立性是模块的重要特点，模块内部最好不与程序的其他部分直接交互。  
为了在模块内部调用全局变量，必须显式地将其他变量输入模块。

1

2

3

　　var module1 = (function ($) {

　　　　//...

　　})(jQuery);

但是上面的这种做法依然有不足之处，我们在使用这种方式定义的模块时，依然要通过全局作用域来访问模块，并且模块之间的依赖关系也不够明显。

并且这种模式需要通过script标签将模块文件插入到文档中，被依赖的模块必须放在依赖它的模块之前，如果模块之间的依赖很复杂，那就悲剧了。为此，社区产生了下面更好的解决方法。

### [](#Commonjs "Commonjs")Commonjs

上面介绍JavaScript中如何现实模块化的一些常见的模式，但是仅仅有这些模式是不够的，对于整个生态圈来说，JS 模块化缺失带来的一个严重问题是各社区开发一套组件都需要实现自己的模块化机制，不同社区重复制造轮子，导致组件与组件无法兼容、相互割裂，严重阻碍生态系统的发展，模块化规范的制定和遵守更加重要。

为此， Mozilla 工程师 Kevin Dangoor 在 2009 年 1 月发起了 ServerJS，目标是为非浏览器（比如服务端、本地桌面应用、命令行应用）构建 JavaScript 生态系统，同年 8 月改名为 CommonJS，其目标也扩展到浏览器。CommonJS 的规范包括模块(Modules)、包(Package)、Promises 等多个方面，详情可查阅 CommonJS Wiki。CommonJS 规范有很多的实现，其中最著名的实现就是 Node.js，接下来就以它作为例子介绍 CommonJS 的模块规范。

从结构的角度来看，一个CJS模块是一段可重用的JavaScript，它导出一系列特定的对象给依赖它的代码调用。

简单来说来看，一个CJS模块主要包含两个部分：一个名叫exports的自由变量，它包含模块希望提供给其它模块的对象；以及一个 require 函数，让模块用来导入其它模块的导出。

下面通过一个简单的例子来理解exports和require：

1

2

3

4

5

6

7

8

// package/lib 是我们须的一个依赖项

var lib = require('package/lib');

// 我们的模块的一些行为

function foo(){

    lib.log('hello world!');

}

// 把 foo 导出（暴露）给其它模块

exports.foo = foo;

exports 的基本用法

1

2

3

4

5

6

7

8

9

10

11

// 定义我们希望暴露的更多行为

function foobar(){

        this.foo = function(){

                console.log('Hello foo');

        }

        this.bar = function(){

                console.log('Hello bar');

        }

}

// 把 foobar 暴露给其它模块

exports.foobar = foobar;

require 的基本用法

1

2

3

4

5

// 一个使用了 'foobar' 的应用

// 相对于使用文件与模块文件所在的同一目录路径获取模块

var foobar = require('./foobar').foobar,

    test = new foobar();

test.bar(); // 'Hello bar'

### [](#在浏览器中使用-CommonJS "在浏览器中使用 CommonJS")在浏览器中使用 CommonJS

浏览器并不不兼容CommonJS，要想在浏览器中加载 CommonJS ，我们需要使用模块加载器：Browserify 是目前最常用的 CommonJS 模块加载器。

但是有些开发者觉得 CommonJS 更适合于服务器端开发，因为很多处理面向服务器端特性的 CommonJS API 根本无法用 JavaScript 在浏览器级别实现。并且，CommonJS 则采用了服务器优先的策略，采取同步行为、服务器端的资源可以保存在服务器本地的硬盘里面，从硬盘里读取资源的速度是很快的。但是当把这种模式运用到浏览器端之后，浏览器必须从web上下载需要的资源，而下载的速度会受到网速的限制，所以一旦我们所需加载的资源较多，或者用户的网速较慢，就会造成浏览器加载全部资源以前处于假死状态，严重影响我们的用户体验。

所以，浏览器端的模块，不能采用”同步加载”（synchronous），最好采用”异步加载”（asynchronous）。AMD应运而生。

### [](#AMD "AMD")AMD

AMD是”Asynchronous Module Definition”的缩写，意思就是”异步模块定义”。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

AMD帮助消除对全局变量的需求。 每个模块都通过局部变量引用或者返回对象来定义其依赖模块以及输出功能。因此，模块不需要引入全局变量就能够定义其功能并实现与其他模块的交互。AMD同时是“匿名的”，意味着模块不需要硬编码指向其路径的引用， 模块名仅依赖其文件名和目录路径。

这里你须要先对下面这两个重要的概念有一定的了解：一个用来进行模块定义的 define 方法以及一个用来处理依赖项加载的 require 方法。define 根据如下的方法定义具名或匿名的模块：

1

2

3

4

5

define(

    module_id /*可选*/,

    \[dependencies\] /*可选*/,

    definition function /*用来初始化模块或对象的函数*/

);

正如上面展示的，define函数接受三个参数，第一个参数是模块名，可选。第二个参数定义了模块的依赖项，可选。第三个参数是一个初始化模块的函数，是必须的。  
下面通过一个具体的例子来了解如何使用define定义一个模块：

1

2

3

4

5

6

7

8

9

10

11

12

define('myModule',

    \['foo', 'bar'\],

    function ( foo, bar ) {

        // 在这里创建模块

        var myModule = {

            //这里可以使用foo模块和bar模块

            doSomething:function(){

                console.log('Hello world!');

            }

        }

        return myModule;

});

AMD也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：

1

　　require(\[module\], callback);

第一个参数\[module\]，是一个数组，里面的成员就是要加载的模块；第二个参数callback，则是加载成功之后的回调函数。如果将前面的代码改写成AMD形式，就是下面这样：

1

2

3

　　require(\['foobar'\], function (foobar) {

　　　　foobar.bar(); // 'Hello bar'

　　});

AMD模块也不能在浏览器中直接加载，同样可以使用模块加载器来完成。webpack可以用来加载AMD模块。

### [](#总结： "总结：")总结：

#### [](#模块接口的设设计原则： "模块接口的设设计原则：")模块接口的设设计原则：

为一个模块设计一个接口是一个非常精妙的工作，接口设计可以采用多种方法，但是我们在设计接口时一定要有远见性，要考虑到以后的使用。为此，模块接口的设计必须遵循以下原则：

可预见原则：一个网页应用可能是由多个开发人员参与开发的，其他的开发人员可能要使用你的接口，为了不让他花费太多精力查看你的接口的使用方式，你应该尽量保持接口的可预测性。

可共用原则：一个模块有时不仅仅只用到一个程序中，模块的一大优点就是可以进行代码复用。所以我们要尽量让我们的模块内部的数据结构和实现方法简单明了，让我们的接口可以被不同的模块使用。

层次接口原则：一个模块应该提供不同层次的接口，如果我们要写一个复杂的程序，这个程序面向不同层次的使用者，我们应该给不同层次的使用者提供不同层次的使用接口。

### [](#ECMAScript-6-Modules "ECMAScript 6 Modules")ECMAScript 6 Modules

令人振奋的消息是 2015 年 6 月正式发布的 ECMAScript 6 包含了模块规范，采用申明式的语法，使用 import、export 这两个关键字，同时照顾到 Common.JS 社区和 AMD 社区的使用习惯，方便地实现模块的定义和导入。

新的标准规定使用两个关键字：

export： 声明了某个模块的本地绑定是外部可见的，这样其它模块就能够读取它们但却无法进行修改。有趣的是，模块可以导出子模块，却无法导出已经在别处定义过的模块。你同样可以给导出重命名来让它们不同于本地的名字。

import：声明把某个模块的导出绑定为本地变量，并可以重命名来避免命名冲突。

下面来分别讲解：

export：

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

// export data

export var color = "red";

export let name = "Nicholas";

export const magicNumber = 7;

// export function

export function sum(num1, num2) {

    return num1 + num1;

}

// export class

export class Rectangle {

    constructor(length, width) {

        this.length = length;

        this.width = width;

    }

}

// this function is private to the module

function subtract(num1, num2) {

    return num1 - num2;

}

// define a function

function multiply(num1, num2) {

    return num1 * num2;

}

// export later

#### [](#export-multiply "export multiply;")export multiply;

我们只需在在原JS的声明前加上export关键字就可以导出数据、类、函数。也可以先定义后导出。而模块中未加export关键字的数据、类和函数都将为模块所私有。

但是需要注意的是该关键字只能在顶级作用域中使用，类似下面的用法是明显的语法错误：

1

2

3

if (flag) {

    export flag;    // syntax error

}

#### [](#import "import:")import:

一旦我们把一个模块导出，这个模块就可以使用关键字import导入，语法如下：

1

import { identifier1, identifier2 } from "module";

identifier1, identifier2是要导入的模块，module是被导入的模块的来源。

identifiers的具体写法还没有确定下来，可能要等到浏览器和node.js开始原生的使用模块时才会确定identifiers的具体格式。

可以使用import关键字从一个文件中导入多个模块。

1

import { module1, module2 \[,module3,...\]} from "example";

如果要从一个文件中导入所有的模块：

1

import * as example from "example";

源文件中的所有模块都被导入，并且被命名为example，源文件中所有的模块都变成example的属性，调用方式为：

1

2

3

4

5

// import everything

import * as example from "example";

console.log(example.sum(1,

        example.magicNumber));          // 8

console.log(example.multiply(1, 2));    // 2

被导入的模块还可以重命名：

1

2

3

import { add as sum } from "example";

console.log(typeof add);            // "undefined"

console.log(sum(1, 2));             // 3

从一个模块中导入一个标示符后，这个标示符将表现的和使用const关键字定义的标示符一样，不能被重新赋值：

1

2

3

import { sum } from "example";

console.log(sum(1, 2));     // 3

sum = 1;        // error

每一个模块都可以且仅可以定义一个默认值，语法如下：

1

2

3

4

//默认值可以是函数、变量或者类，这里以函数为例

export default function(num1, num2) {

    return num1 + num2;

}

导入时的语法：

1

2

3

// import the default

import sum from "example";

console.log(sum(1, 2));     // 3

或者：

1

2

import { default as sum} from "example";

console.log(sum(1, 2));     // 3

再次导出：

我们可以把已经导入的模块再次导出，并且可以在此过程中进行重命名：

1

2

3

4

5

6

import { sum } from "example";

export { sum }

//or

export { sum as add } from "example";

//export everything from another module

export * from "example";

需要注意的是，import关键字会将导入的变量、函数和类绑定到本地，你依然可以在本地通过导入的模块去修改它们。

1

2

3

4

5

6

7

8

export var name = "Nicholas";

export function setName(newName) {

    name = newName;

}

import { name, setName } from "example";

console.log(name);       // "Nicholas"

setName("Greg");

console.log(name);       // "Greg"

参考资料：

1.[Eloquent JavaScript-Chapter 10. Modules](https://www.safaribooksonline.com/library/view/eloquent-javascript-2nd/9781457189821/ch10.html)

2.[Understanding ECMAScript 6-Modules](https://leanpub.com/understandinges6/read/#leanpub-auto-modules)

3.[使用 AMD、CommonJS 及 ES Harmony 编写模块化的 JavaScript](http://justineo.github.io/singles/writing-modular-js/)

4.[Javascript模块化编程（一）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

5.[Javascript模块化编程（二）：模块的写法](http://www.ruanyifeng.com/blog/2012/10/asynchronous_module_definition.html)

6.[Javascript模块化编程（三）：模块的写法](http://www.ruanyifeng.com/blog/2012/11/require_js.html)

7.[后端程序员的 JavaScript 之旅 - 模块化（一） | 简书](http://lishaopeng.com/2016/02/05/js-module/)

8.[后端程序员的 JavaScript 之旅 - 模块化（二） | 简书](http://lishaopeng.com/2016/02/11/js-module2/)

9.[后端程序员的 JavaScript 之旅 - 模块化（三） | 简书](http://lishaopeng.com/2016/02/19/js-module3/)
