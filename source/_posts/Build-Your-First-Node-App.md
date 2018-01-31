---
title: Build Your First Node App
date: 2018-02-01 00:07:18
tags:
---

Build Your First Node App
=========================

1.  [Hello World](#Hello-World)
2.  [一个完整的node的web应用](#一个完整的node的web应用)
3.  [应用构建](#应用构建)

这是一片node入门级别的文章，着重介绍使用node实现一个小型web应用的过程，不会涉及关于原理方面的知识，作为一个初学者，文章中如有不合理的地方还请指正。

### [](#Hello-World "Hello World")Hello World

首先我们来实现一个Hello World。

打开编辑器，创建一个helloWord.js文件，写入以下代码：

1

console.log('Hello World!')

保存文件后通过在命令行执行：

1

node helloWord.js

你会看到命令行输出’Hello World!’。  
这个应用太小儿科了对吧，那我们来点‘干货’。

### [](#一个完整的node的web应用 "一个完整的node的web应用")一个完整的node的web应用

对于初学者来说，我们不太可能写出复杂的web应用，那我们来个简单点儿的！

我将这个应用的需求设定为：

用户可以使用浏览器访问我们的应用。  
用户在浏览器中输入 ‘[http://domain/start‘](http://domain/start‘) 时页面中会出现一个表单。  
用户在表单中输入用户名并提交，页面跳转到 ‘[http://domain/hello‘](http://domain/hello‘) 并显示欢迎信息。  
下面我们来分析这个应用如何分布实现；

我们需要提供web页面，并且可以提交表单，所以我们需要一个HTTP服务器。  
我们需要在不同的URL之间跳转，针对不同URL给出响应，所以我们需要一个路由.  
路由需要处理表单提交的数据，所以我们需要事件处理程序。  
我们来一步步实现：

### [](#应用构建 "应用构建")应用构建

在开始编码之前，我们需要考虑如何组织我们的代码，我们当然可以在一个单独文件中编写所有代码，但这个不利于后期修改和拓展，并且代码可读性也会变得很差，如果你有模块化相关得知识，你应该很清楚这样做的缺点。

所以我们选择将不同功能的代码放入不同的模块中，使用不同模块时只需在主文件中引入即可。  
首先我们创建’index.js’作为我们的入口文件（主文件），执行应用时直接运行入口文件即可。

然后我们创建一个用于启动服务器的文件，语义化的将其命名为’server.js’，并写入以下代码：

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

var http = require("http");

function onRequest(request, response) {

    //下面一行为增加部分

    console.log("Request received.");

    response.writeHead(200, {"Content-Type": "text/plain"});

    response.write("Hello World");

    response.end();

}

http.createServer(onRequest).listen(8888);

console.log("Server has started.");

console.log('在浏览器中打开 http://localhost:8080/ 查看效果！');

现在我们实现了一个可以工作的HTTP服务器，这当然不是最终代码，在构建的过程中我们还是需要了解一下实现的细节。

使用node执行脚本：

1

node server.js

接下来在浏览器中打开 ‘[http://localhost:8080/‘](http://localhost:8080/‘) ，你就会看到一个写着 ‘Hello World!’ 的页面。

分析代码实现：

第一行请求（require）Node.js内置的 http 模块，并且把它赋值给 http 变量。

接下来我们调用http模块提供的函数： ‘createServer’ 。这个函数会返回一个对象，这个对象有一个叫做 ‘listen’ 的方法，这个方法有一个数值参数，指定这个HTTP服务器监听的端口号。

‘http.createServer ‘方法接受一个回调函数，侦听指定端口的服务器在收到一个HTTP请求的时会调用这个函数。

当收到请求时，使用 response.writeHead() 回调函数发送一个HTTP状态200和HTTP头的内容类型（content-type），使用 response.write() 回调函数在HTTP相应主体中发送文本“Hello World”。response.end()结束响应。

现在我们将服务器代码变成一个可供调用的模块：  
将服务器脚本放到一个叫做 start 的函数里，然后导出这个函数。

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

var http = require("http");

function start(){

    function onRequest(request, response) {

        //下面一行为增加部分

        console.log("Request received.");

        response.writeHead(200, {"Content-Type": "text/plain"});

        response.write("Hello World");

        response.end();

    }

    http.createServer(onRequest).listen(8080);

    console.log("Server has started.");

    console.log('Open http://localhost:8080/ in browser to checkout！');

}

exports.start = start;

创建 index.js 文件并写入以下内容：

1

2

var server = require("./server");

server.start();

我们现在已经创建了一个启动服务器的模块，并在入口文件中调用这个模块，通过执行入口来启动服务器：

1

node index.js

你可能会好奇为什么输出了两次 ‘Request received’这是因为访问’[http://localhost:8080/‘](http://localhost:8080/‘) 时浏览器会尝试读取 ‘[http://localhost:8888/favicon.ico‘](http://localhost:8888/favicon.ico‘) 文件。

进行路由选择

还记得我们的我们给服务器代码的回调函数传入了两个参数吗？我们还没有使用 request 参数，这是一个对象，包含请求信息。

要针对不同路由实现不同操作，我们需要使用node的url模块解析请求的URL。

修改服务器代码：

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

var http = require("http"),

url = require('url');

function start(){

    function onRequest(request, response) {

        var pathname = url.parse(request.url).pathname;

        console.log('Request for' + pathname +'received.')

        response.writeHead(200, {"Content-Type": "text/plain"});

        response.write("Hello World");

        response.end();

    }

    http.createServer(onRequest).listen(8080);

    console.log("Server has started.");

   console.log('Open http://localhost:8080/ in browser to checkout！');

}

exports.start = start;

启动服务器我们可以看到URL可以被正确解析。

现在我们开始编写路由，创建一个 router.js 文件,添加路由函数：

1

2

3

4

function route(pathname) {

    return "A request for " + pathname;

}

exports.route = route;

修改start函数将路由函数传递过去：

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

var http = require("http");

var url = require("url");

function start(route) {

    function onRequest(request, response) {

        var pathname = url.parse(request.url).pathname;

        console.log("Request for " + pathname + " received.");

        var res=route(pathname);

        response.writeHead(200, {"Content-Type": "text/plain"});

        response.write(res);

        response.end();

    }

    http.createServer(onRequest).listen(8080);

    console.log("Server has started.");

    console.log('Open http://localhost:8080/ in browser to checkout！');

}

exports.start = start;

启动服务器，打开[http://localhost:8080/,我们可以看到服务器可以正常使用路由模块了。](http://localhost:8080/,我们可以看到服务器可以正常使用路由模块了。)

接下来我们要扩展路由函数使它可以完成我们的应用需求，我们很自然的想到将处理请求的代码写在路由函数里面，但是如果我们有多个路由需要处理，每个请求处理函数又比较复杂，路由函数就会显得比较臃肿，代码也不利于维护。

所以我们将请求处理模块与路由模块分离开来，创建requestHandlers.js，并写入以下代码：

1

2

3

4

5

6

7

8

function start() {

    console.log("Request handler 'start' was called.");

}

function hello() {

    console.log("Request handler 'hello' was called.");

}

exports.start = start;

exports.hello = hello;

修改index.js:

1

2

3

4

5

6

7

8

var server = require("./server");

var router = require("./router");

var requestHandlers = require("./requestHandlers");

var handle = {}

handle\["/"\] = requestHandlers.start;

handle\["/start"\] = requestHandlers.start;

handle\["/hello"\] = requestHandlers.hello;

server.start(router.route, handle);

我们把handle对象作为额外的参数传递给服务器，为此将server.js修改如下：

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

var http = require("http");

var url = require("url");

function start(route, handle) {

    function onRequest(request, response) {

        var pathname = url.parse(request.url).pathname;

        console.log("Request for " + pathname + " received.");

        route(handle, pathname);

        response.writeHead(200, {"Content-Type": "text/plain"});

        response.write("Hello World");

        response.end();

    }

    http.createServer(onRequest).listen(8888);

    console.log("Server has started.");

    console.log('Open http://localhost:8080/ in browser to checkout！');

}

exports.start = start;

这样我们就在start()函数里添加了handle参数，并且把handle对象作为第一个参数传递给了route()回调函数。

然后我们相应地在route.js文件中修改route()函数：

1

2

3

4

5

6

7

8

9

function route(handle, pathname) {

  console.log("About to route a request for " + pathname);

  if (typeof handle\[pathname\] === 'function') {

    handle\[pathname\]();

  } else {

    console.log("No request handler found for " + pathname);

  }

}

exports.route = route;

处理post请求

为了实现提交一个表单路由跳转并且输出提交信息，我们需要显示一个输入框供用户输入内容，然后通过POST请求提交给服务器。最后，服务器接受到请求，通过处理程序将输入的内容展示到浏览器中。

/start请求处理程序用于生成带输入框的表单，我们将requestHandlers.js修改为如下形式：

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

25

26

function start(response) {

  console.log("Request handler 'start' was called.");

  var body = '<html>'+

    '<head>'+

    '<meta http-equiv="Content-Type" content="text/html; '+

    'charset=UTF-8" />'+

    '</head>'+

    '<body>'+

    '<form action="/hello" method="post">'+

    '<textarea name="text" rows="1" cols="10"></textarea>'+

    '<input type="submit" value="Submit text" />'+

    '</form>'+

    '</body>'+

    '</html>';

    response.writeHead(200, {"Content-Type": "text/html"});

    response.write(body);

    response.end();

}

function hello(response) {

  console.log("Request handler 'hello' was called.");

  response.writeHead(200, {"Content-Type": "text/plain"});

  response.write("Hello hello");

  response.end();

}

exports.start = start;

exports.hello = hello;

服务器需要获取所有来自表单的数据，然后将数据传递给请求路由和请求处理函数进行进一步的处理。为了使整个过程非阻塞，Node.js会将POST数据拆分成很多小的数据块，然后通过触发特定的事件，将这些小数据块传递给回调函数。这里的特定的事件有data事件（表示新的小数据块到达了）以及end事件（表示所有的数据都已经接收完毕）。我们需要告诉Node.js当这些事件触发的时候，回调哪些函数。

下面开始修改server.js：

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

var http = require("http");

var url = require("url");

function start(route, handle) {

  function onRequest(request, response) {

    var postData = "";

    var pathname = url.parse(request.url).pathname;

    console.log("Request for " + pathname + " received.");

    request.setEncoding("utf8");

    request.addListener("data", function(postDataChunk) {

      postData += postDataChunk;

      console.log("Received POST data chunk '"+

      postDataChunk + "'.");

    });

    request.addListener("end", function() {

      route(handle, pathname, response, postData);

    });

  }

  http.createServer(onRequest).listen(8080);

  console.log("Server has started.");

  console.log('Open http://localhost:8080/ in browser to checkout！');

}

exports.start = start;

我们接下来在/hello页面，展示用户输入的内容。要实现该功能，我们需要将postData传递给请求处理程序，修改router.js为如下形式：

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

function route(handle, pathname, response, postData) {

  console.log("About to route a request for " + pathname);

  if (typeof handle\[pathname\] === 'function') {

    handle\[pathname\](response, postData);

  } else {

    console.log("No request handler found for " + pathname);

    response.writeHead(404, {"Content-Type": "text/plain"});

    response.write("404 Not found");

    response.end();

  }

}

exports.route = route;

我们现在可以接收POST数据并在请求处理程序中处理该数据了。

我们最后要做的是： 把POST数据中，我们感兴趣的部分传递给请求路由和请求处理程序。在我们这个例子中，我们感兴趣的其实只是text字段。

我们可以使用node内置的querystring模块来实现,修改requestHandlers.js:

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

25

26

27

28

var querystring = require("querystring");

function start(response, postData) {

  console.log("Request handler 'start' was called.");

  var body = '<html>'+

    '<head>'+

    '<meta http-equiv="Content-Type" content="text/html; '+

    'charset=UTF-8" />'+

    '</head>'+

    '<body>'+

    '<form action="/hello" method="post">'+

    '<textarea name="text" rows="1" cols="10"></textarea>'+

    '<input type="submit" value="Submit text" />'+

    '</form>'+

    '</body>'+

    '</html>';

    response.writeHead(200, {"Content-Type": "text/html"});

    response.write(body);

    response.end();

}

function hello(response, postData) {

  console.log("Request handler 'hello' was called.");

  response.writeHead(200, {"Content-Type": "text/plain"});

  response.write("Hello: "+

  querystring.parse(postData).text);

  response.end();

}

exports.start = start;

exports.hello = hello;

启动服务器，打开[http://localhost:8080/,输入用户名，页面就会跳转并输出欢迎信息.查看\[源码\](https://github.com/shengxihu/Exercise/tree/master/node/node-1](http://localhost:8080/,输入用户名，页面就会跳转并输出欢迎信息.查看[源码](https://github.com/shengxihu/Exercise/tree/master/node/node-1))

到目前为止，我们已经实现了一个简单的web应用，虽然它很简单，但是我们在编写过程中注重采用了模块化、解耦合的思想，这给了我们继续完善的空间，随着我们对node了解不断加深，我们可以基于它打造出功能更加强大的应用。