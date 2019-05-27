# Nodejs总结

本文参考<http://nqdeng.github.io/7-days-nodejs/>

### NPM

- 允许用户从NPM服务器下载别人编写的三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

工具：[npmjs.org](https://npmjs.org/)提供了个搜索框可以根据包名来搜索

下载好之后，包就放在了工程目录下的`node_modules`目录中，因此在代码中只需要通过`require('xxx')`的方式就好，无需指定三方包路径。如果想要下载指定版本的话，可以在包名后边加上`@<version>`

如果使用到的三方包比较多，在终端下一个包一条命令地安装未免太人肉了。因此NPM对`package.json`的字段做了扩展，允许在其中申明三方包依赖。因此，上边例子中的`package.json`可以改写如下：

```
{
    "name": "node-echo",
    "main": "./lib/echo.js",
    "dependencies": {
        "argv": "0.0.2"
    }
}
```

这样处理后，在工程目录下就可以使用`npm install`命令批量安装三方包了。更重要的是，当以后`node-echo`也上传到了NPM服务器，别人下载这个包时，NPM会根据包中申明的三方包依赖自动下载进一步依赖的三方包。例如，使用`npm install node-echo`命令时，NPM会自动创建以下目录结构。

```
- project/
    - node_modules/
        - node-echo/
            - node_modules/
                + argv/
            ...
    ...
```

#### Stream

```
var fs = require('fs');
var rs = fs.createReadStream(pathname);

rs.on('data', function (chunk) {
    doSomething(chunk);
});//事件驱动机制、data事件不断触发，给doSomething函数加上了回调，因此我们可以在处理数据前暂停数据读取，并在处理数据后继续读取数据。

rs.on('end', function () {
    cleanUp();
});
```

#### **大文件读写注意防爆仓控制（读写速度控制、管道设置pipe）**

```
var rs = fs.createReadStream(src);
var ws = fs.createWriteStream(dst);

rs.on('data', function (chunk) {
    if (ws.write(chunk) === false) {
        rs.pause();
    }
});

rs.on('end', function () {
    ws.end();
});

ws.on('drain', function () {
    rs.resume();
});
```

#### FileSyetem

NodeJS通过`fs`内置模块提供对文件的操作。`fs`模块提供的API基本上可以分为以下三类：

- 文件属性读写。

  其中常用的有`fs.stat`、`fs.chmod`、`fs.chown`等等。

- 文件内容读写。

  其中常用的有`fs.readFile`、`fs.readdir`、`fs.writeFile`、`fs.mkdir`等等。

- 底层文件操作。

  其中常用的有`fs.open`、`fs.read`、`fs.write`、`fs.close`等等。

基本上所有`fs`模块API**异步**的回调参数都有两个。第一个参数在有错误发生时等于异常对象，第二个参数始终用于返回API方法执行结果。

`fs`模块的所有异步API都有对应的同步版本，用于无法使用异步操作时，或者同步操作更方便时的情况。同步API除了方法名的末尾多了一个`Sync`之外，异常对象与执行结果的传递方式也有相应变化。同样以`fs.readFileSync`为例：

```
try {
    var data = fs.readFileSync(pathname);
    // Deal with data.
} catch (err) {
    // Deal with error.
}
```

总结：异步回调2个参数err，data；同步试用try...catch...

#### Http

HTTP请求本质上是一个数据流，由请求头（headers）和请求体（body）组成。例如以下是一个完整的HTTP请求数据内容。

```
POST / HTTP/1.1
User-Agent: curl/7.26.0
Host: localhost
Accept: */*
Content-Length: 11
Content-Type: application/x-www-form-urlencoded

Hello World
```

可以看到，空行之上是请求头，之下是请求体。HTTP请求在发送给服务器时，可以认为是按照从头到尾的顺序一个字节一个字节地以数据流方式发送的。而`http`模块创建的HTTP服务器在接收到完整的请求头后，就会调用回调函数。在回调函数中，除了可以使用`request`对象访问请求头数据外，还能把`request`对象当作一个只读数据流来访问请求体数据。

#### 创建客户端

```
var options = {
        hostname: 'www.example.com',
        port: 80,
        path: '/upload',
        method: 'POST',
        headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
        }
    };

var request = http.request(options, function (response) {});

request.write('Hello World');
request.end();
```

可以看到，`.request`方法创建了一个客户端，并指定请求目标和请求头数据。之后，就可以把`request`对象当作一个只写数据流来写入请求体数据和结束请求。另外，由于HTTP请求中`GET`请求是最常见的一种，并且不需要请求体，因此`http`模块也提供了以下便捷API。

```
http.get('http://www.example.com/', function (response) {});
```

当客户端发送请求并接收到完整的服务端响应头时，就会调用回调函数。在回调函数中，除了可以使用`response`对象访问响应头数据外，还能把`response`对象当作一个只读数据流来访问响应体数据。

```
http.get('http://www.example.com/', function (response) {
    var body = [];

    console.log(response.statusCode);
    console.log(response.headers);

    response.on('data', function (chunk) {
        body.push(chunk);
    });

    response.on('end', function () {
        body = Buffer.concat(body);
        console.log(body.toString());
    });
});

------------------------------------
200
{ 'content-type': 'text/html',
  server: 'Apache',
  'content-length': '801',
  date: 'Tue, 05 Nov 2013 06:08:41 GMT',
  connection: 'keep-alive' }
<!DOCTYPE html>
...
```

#### 异步编程

可以预见一般我们是怎么写代码的。基本上，我们的代码都是做一些事情，然后调用一个函数，然后再做一些事情，然后再调用一个函数，如此循环。如果我们写的是同步代码，只需要在代码入口点写一个`try`语句就能捕获所有冒泡上来的异常。

异步代码很容易写成`>`模式难看难懂。所以，nodejs推出**域**

#### **域（Domain）**

NodeJS提供了`domain`模块，可以简化异步代码的异常处理。在介绍该模块之前，我们需要首先理解“域”的概念。简单的讲，一个域就是一个JS运行环境，在一个运行环境中，如果一个异常没有被捕获，将作为一个全局异常被抛出。NodeJS通过`process`对象提供了捕获全局异常的方法，示例代码如下

```
function async(request, callback) {
    // Do something.
    asyncA(request, function (data) {
        // Do something
        asyncB(request, function (data) {
            // Do something
            asyncC(request, function (data) {
                // Do something
                callback(data);
            });
        });
    });
}

http.createServer(function (request, response) {
    var d = domain.create();

    d.on('error', function () {
        response.writeHead(500);
        response.end();
    });

    d.run(function () {
        async(request, function (data) {
            response.writeHead(200);
            response.end(data);
        });
    });
});
```

**JS本身的`throw..try..catch`异常处理机制并不会导致内存泄漏，也不会让程序的执行结果出乎意料，但NodeJS并不是存粹的JS。NodeJS里大量的API内部是用C/C++实现的，因此NodeJS程序的运行过程中，代码执行路径穿梭于JS引擎内部和外部，而JS的异常抛出机制可能会打断正常的代码执行流程，导致C/C++部分的代码表现异常，进而导致内存泄漏等问题。**

**因此，使用`uncaughtException`或`domain`捕获异常，代码执行路径里涉及到了C/C++部分的代码时，如果不能确定是否会导致内存泄漏等问题，最好在处理完异常后重启程序比较妥当。而使用`try`语句捕获异常时一般捕获到的都是JS本身的异常，不用担心上诉问题。**

