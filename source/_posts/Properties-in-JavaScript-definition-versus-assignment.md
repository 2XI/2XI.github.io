---
title: Properties in JavaScript/ definition versus assignment
date: 2018-02-01 00:05:28
tags:
---

Properties in JavaScript/ definition versus assignment
======================================================

1.  [属性定义和属性赋值](#属性定义和属性赋值)
2.  [概述：属性特性和内部属性](#概述：属性特性和内部属性)

原文链接:Properties in JavaScript: definition versus assignment;

在JavaScript，属性的定义和赋值两个不同的操作，这篇博客致力于解释这两种操作的差异和造成这些差异的原因。

### [](#属性定义和属性赋值 "属性定义和属性赋值")属性定义和属性赋值

1.属性定义：定义属性将会使用一个如下的属性定义函数：、

1

Object.defineProperty(obj, propName, propDesc)

这个函数的主要功能是直接在obj对象上添加一个属性，通过propDesc配置属性特性（例如writable）。  
这个函数的另一个功能是用来改变一个属性的特性，包括属性的值。

2.属性赋值：为了给一个属性赋值，使用如下的赋值表达式：

1

obj.prop = value

这个表达式的主要功能是改变一个属性的值，为了完成这个操作，JavaScript将会查询obj对象的原型链。如果obj对象或者它的原型链上定义了setter方法，赋值操作将会通过调用这个setter方法来完成。如果被赋值的这个属性在obj对象上不存在，这个赋值操作将会产生另一个结果——在obj对象上定义一个拥有默认特性（writable等）的prop属性。

接下来的两个部分将会探讨更多关于属性定义和赋值是如何工作的细节。及时你跳过它们，你应该依然能够理解第四部分以及后续段落。

### [](#概述：属性特性和内部属性 "概述：属性特性和内部属性")概述：属性特性和内部属性

在我们解释属性定义和赋值是如何工作之前，让我们快速回顾一下什么事属性特性和内部属性。

1.属性的种类：

JavaScript中存在3种属性

命名访问器属性：一个通过getter或者setter定义的属性。

命名数据属性：拥有一个确定值得属性，这是最常见的属性，它们有自己的方法。

内部属性：由JavaScript引擎内部使用的属性，不能通过直接JavaScript代码操作。然而，然而我们可以间接的方式来操作它们。例如，每一个对象都有一个叫做\[\[Prototype\]\]的内部属性，你不可以直接的读取到它，但是依然可以通过Object.getPrototypeOf()来获取它的值。尽管内部属性通常由双中括号包围的名称来表示，但这并不是它们的名字， 这是一种抽象的表示，它们并没有字符串值表示的属性名。

2.属性特性：

每个属性都拥有以下的属性特性，并且被它们所影响。

所有属性都有的特性：

\[Enumerable\]\]：如果一个属性是不可枚举的,则在一些操作下,这个属性是不可见的,比如for…in和Object.keys().

\[\[Configurable\]\]: 如果一个属性是不可配置的,则该属性的除了\[\[Value\]\]的所有特性都不可改变.

命名数据特性：

\[\[Value\]\]: 属性的值.

\[\[Writable\]\]:决定属性值是否可变.

命名访问器属性:

\[\[Get\]\]:拥有getter方法.

\[\[Set\]\]:拥有setter方法.

3.属性描述符：

属性操作符可以写成一个包含一系列属性特性的对象。例如：

1

2

3

4

{

    value: 123,

    writable: false

}

如你所见，对象的属性名对应着\[\[Value\]\]和\[\[Writable\]\]这样的属性特性名。属性操作符使用在Object.defineProperty, Object.getOwnPropertyDescriptor, Object.create这样的改变或者返回属性特性的函数中.如果某个属性特性缺失，属性操作符将使用下表对应的默认值.

属性 默认值  
value undefined  
get undefined  
set undefined  
writable false  
enumerable false  
configurable false

4.内部属性:

每个对象都包含如下四个属性在内的一些内部属性：

\[\[Prototype\]\]: 对象的原型.

\[\[Extensible\]\]: 对象是否可扩展，即是否可添加新的属性.

\[\[DefineOwnProperty\]\]: 定义一个属性的内部方法.

\[\[Put\]\]: 为一个属性赋值的内部方法.

属性定义和属性赋值详解

1.属性定义

属性定义是通过如下的内部方法来操作的：

1

\[\[DefineOwnProperty\]\] (P, Desc, Throw)

p是将要定义的属性名，Desc是属性描述符，Throw明确了如何处理操作异常:如果Throw为true,则抛出异常.否则,操作只会静默失败.当调用\[\[DefineOwnProperty\]\]时,会下面的操作步骤执行操作.

*   如果对象上没有名为P的属性：
    
    *   如果属性可扩展，创建一个新的属性P.
        
    *   如果对象不可扩展，拒绝操作.
        
*   如果对象上已经有一个名为P的属性:
    
    *   如果属性可配置,则重新配置属性.
        
    *   如果属性不可配置,如下操作将会被拒绝（不在如下范围内的操作可以进行）:
        
        *   将一个数据属性转换成访问器属性,反之亦然.
            
        *   改变\[\[Configurable\]\]或\[\[Enumerable\]\].
            
        *   该变\[\[Writable\]\].
            
        *   在\[\[Writable\]\]为false时改变\[\[Value\]\].
            
        *   改变\[\[Get\]\]或\[\[Set\]\].
            

如果P的属性操作符和和当前属性操作符一致，操作可以进行.

使用Object.defineProperty和Object.defineProperties这两个函数用来定义属性，例如：

1

Object.defineProperty(obj, propName, desc)

JavaScript引擎内部将会将其转换成如下的方法调用:

1

obj.\[\[DefineOwnProperty\]\](propName, desc, true)

2.属性赋值:

为一个属性赋值操作时JavaScript引擎会调用如下的内部方法:

1

\[\[Put\]\] (P, Value, Throw)

P 和 Throw 和 \[\[DefineOwnProperty\]\]中的对应参数作用相同.\[\[Put\]\]方法调用时会按照如下情况执行操作:

*   如果在原型链上存在一个名为P的只读属性(只读的数据属性或者没有setter的访问器属性),拒绝操作.
    
*   如果在原型链上存在一个名为P的且拥有setter的访问器属性:调用这个setter.
    
*   如果没有名为P的自身属性:
    
    *   如果对象不可扩展：拒绝操作.
        
    *   如果这个对象是可扩展的,就使用下面的操作创建一个新属性:
        

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

this.\[\[DefineOwnProperty\]\](

    P,

    {

        value: Value,

        writable: true,

        enumerable: true,

        configurable: true

    },

    Throw

)

如果已经存在一个可写的名为P的自身属性.则调用:

1

this.\[\[DefineOwnProperty\]\](P, { value: Value }, Throw)

这会改变P的值，其它特性值保持不变。

如下赋值操作将会调用\[\[Put\]\].

1

obj.prop = v;

在浏览器引擎内部将其会转换成如下的方法调用:

1

obj.\[\[Put\]\]("prop", v, isStrictModeOn)

这个赋值操作只会在严格模式下抛出异常，并且\[\[Put\]\]方法没有返回值（赋值操作会产生返回值）.

结论

这部分我们将会探讨关于属性定义和属性赋值的一些结论.

1.赋值可能会调用原型链上的setter方法，定义会创建一个自身属性.

给定一个原型proto上有名为foo访问器属性的对象.

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

var proto = {

    get foo() {

        console.log("Getter");

        return "a";

    },

    set foo(x) {

        console.log("Setter: "+x);

    },

};

var obj = Object.create(proto);

在对象obj上定义一个名为foo的属性或者为名为foo的属性赋值会有什么区别呢？

如果是定义操作，你将会创建一个新的属性.在这种条件下的定义操作总会在原型链的第一个object上创建一个新属性.如本例中的obj:

1

2

3

4

5

6

\> Object.defineProperty(obj, "foo", { value: "b" });

\> obj.foo

'b'

\> proto.foo

Getter

'a'

如果在obj上进行赋值操作，意味着你的目的是改变某个已经存在的属性的值.这个操作将会由setter方法来完成.下面的结果表明操作的确调用了原型链上的setter方法：

1

2

3

\> obj.foo = "b";

Setter: b

'b'

你可以通过只定义一个getter方法创建一个只读属性，如下，对象proto2的bar属性就是一个只读属性，它被obj2继承.

1

2

3

4

5

6

7

8

"use strict";

var proto2 = {

    get bar() {

        console.log("Getter");

        return "a";

    },

};

var obj2 = Object.create(proto2);

我们使用严格模式来保证赋值操作可以抛出异常.我们想使用赋值操作来改变被bar属性的值，但是因为bar是read-only属性，操作被禁止.

1

2

\> obj2.bar = "b";

TypeError: obj.bar is read-only

但是我们可以在obj2上定义一个新的自身属性来覆盖原型proto上的bar属性:

1

2

3

4

5

6

\> Object.defineProperty(obj2, "bar", { value: "b" });

\> obj2.bar

'b'

\> proto2.bar

Getter

'a'

2.原型中的只读属性会阻止赋值操作，但不会阻止定义操作.

原型中的只读属性会阻止通过赋值操作的方式为对象添加一个自身属性，你需要使用定义操作来达到这个目的.这个限制在ECMAScript 5.1中被引入. 下面的示范操作中obj的原型proto上有一个只读属性foo，它展示了如果对象原型上有一个值为a的foo只读属性，则赋值操作obj.foo = “b”不会再对象obj上创建一个自身属性foo.

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

    "use strict";

    var proto = Object.defineProperties(

        {},

        {

            foo: {  // attributes of property foo:

                value: "a",

                writable: false,  // read-only

                configurable: true  // explained later

            }

        });

    var obj = Object.create(proto);

/\* Assignment. Assignment results in an exception: */

    \> obj.foo = "b";

    TypeError: obj.foo is read-only

一个继承属性竟然能够影响在对象自身属性的创建，这真是一个奇怪的表现。然而这也非常合理的，它正是一个只读属性的的特性.

通过定义的方式,我们可以成功创建一个新的自身属性:

1

2

3

4

5

\> Object.defineProperty(obj, "foo", { value: "b" });

\> obj.foo

'b'

\> proto.foo

'a'

3.赋值操作不会改变原型链上的属性

给出下面的例子，obj将会从proto上继承foo属性.

1

2

var proto = { foo: "a" };

var obj = Object.create(proto);

赋值操作obj.foo不会改变proto.foo,但是会为obj创建一个新的自身属性.

1

2

3

4

5

6

\> obj.foo = "b";

'b'

\> obj.foo

'b'

\> proto.foo

'a'

这种表现的原因是：原型引用的属性值能够被它所有的后代共享，如果试图在它的后代对象上改变这个属性值，就会在这个后代对象上创建一个新的同名自身属性，这个自身属性不会影响它的原型或这个后代对象的后代. 鉴于此, 只读属性的表现可以概括为: 通过阻止自身属性的创建来阻止属性更改. 然而为什么要重写一个原型属性而不是更改一个属性呢？可能有以下两种运用：

方法：仅仅允许在原型上直接修改原型对象上定义的方法，防止通过原型后代对方法进行意外修改.

无方法属性：原型可以提供一个能够提供一个被所有后代共享的值，这个值可以通过后代对象重写但是不能通过后代对象改变.有人认为这是一种反模式，并且他们也不鼓励使用这种技术.因为使用构造器函数来初始化默认值是一种改为简洁的方法.

4.我们只能通过定义操作来创建一个拥有指定特性的属性

如果通过赋值操作创建一个自身属性，它将只有默认属性特性.如果你想指定属性特性，你只能使用定义操作，这包括定义getter和setter方法.

5.对象字面量方法添加的属性实质是通过定义操作来完成的.

例如下面的对象字面量：

1

2

3

var obj = {

    foo: 123

};

使用对象字面量添加的属性在浏览器内部可能被转化为一系列的声明，有两种实现：

第一种转化为赋值操作来实现：

1

2

var obj = new Object();

obj.foo = 123;

第二种是转化为定义操作来实现：

1

2

3

4

5

6

7

8

9

var obj = new Object();

Object.defineProperties(obj, {

    foo: {

        value: 123,

        enumerable: true,

        configurable: true,

        writable: true

    }

});

第二种实现更好的表现了对象字面量的语义：创建新的属性.同样，Object.create方法通过第二个参数接受一个属性描述符.

6.作为方法的对象属性

方法属性的一种实现如下:

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

"use strict";

function Stack() {

}

Object.defineProperties(Stack.prototype, {

    push: {

        writable: false,

        configurable: true,

                    value: function (x) { /* ... */ }

    }

});

这么做的目的是防止对方法属性进行意外赋值:

1

2

3

\> var s = new Stack();

\> s.push = 5;

TypeError: s.push is read-only

然而，正因为push是可配置的，我们可以通过属性定义来覆盖这个方法.

1

2

3

4

5

\> var s = new Stack();

\> Object.defineProperty(s, "push",

      { value: function () { return "yes" }})

\> s.push()

'yes'

我们甚至可以通过定义操作来重写Stack.prototype.push方法.

结论

属性赋值经常被用来为一个对象添加一个新的属性,这篇文字解释了这种做法可能带来的问题. 因此，我们最好遵循以下的规则:

1.使用定义操作来创建新的属性.

2.使用赋值操作来改变属性的值.

在评论中,medikoo提醒了我们使用属性描述符来创建属性可能会有点慢，因此他经常通过为属性赋值来创建新属性,因为这样很方便.值得高兴的是,ECMAScript.next也许会把属性的定义操作变的既快又方便:已经存在一个“定义属性的运算符”的提案,可以作为Object.defineProperties的替代用法.由于属性定义和属性赋值之间存在微妙但是至关重要的差别,这种改进应该会很受欢迎.

参考资料：

1.[Prototypes as classes – an introduction to JavaScript inheritance](http://www.2ality.com/2011/06/prototypes-as-classes.html);

2.[JavaScript properties: inheritance and enumerability](http://www.2ality.com/2011/07/js-properties.html);

3.[Fixing the Read-only Override Prohibition Mistake \[ECMAScript wiki\]](http://wiki.ecmascript.org/doku.php?id=strawman:fixing_override_mistake).