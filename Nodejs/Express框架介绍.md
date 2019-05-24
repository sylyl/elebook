# 框架

### 项目结构

- bin, 存放启动项目的脚本文件
- node_modules, 存放所有的项目依赖库。
- public，静态文件(css,js,img)
- routes，路由文件(MVC中的C,controller)
- views，页面文件(Ejs模板)
- package.json，项目依赖配置及开发者信息
- app.js，应用核心配置文件

### package.json

```
{
  "name": "nodeexpress",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"  //scripts属性是用于定义操作命令的，可以非常方便的增加启动命令，比如								//默认的start，用npm start代表执行node ./bin/www命令
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "ejs": "~2.6.1",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "morgan": "~1.9.1"
  }
}
```

### app.js核心文件

```
//加载依赖库，原来这个类库都封装在connect，现在需要单独加载 4.x.x版本
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

//加载路由控制
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

//创建项目实例
var app = express();

//定义EJS模板引擎和模板文件位置，也可以使用其他模型引擎
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

//定义日志和输出级别
app.use(logger('dev'));

//定义数据解析器
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

//定义cookie解析器
app.use(cookieParser());

//定义静态文件目录
app.use(express.static(path.join(__dirname, 'public')));

//匹配路径和路由
app.use('/', indexRouter);
app.use('/users', usersRouter);

//404错误处理
app.use(function(req, res, next) {
  next(createError(404));
});

//500错误处理
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

//导出模型app
module.exports = app;
```

### 查看./bin/www文件

* www文件也是一个node的脚本，用于分离配置和启动程序。

  ```
  #!/usr/bin/env node
  
  /**
   * Module dependencies.
   */
  
  var app = require('../app');
  var debug = require('debug')('nodeexpress:server');
  var http = require('http');
  
  /**
   * Get port from environment and store in Express.
   */
  
  var port = normalizePort(process.env.PORT || '3000');
  app.set('port', port);
  
  /**
   * Create HTTP server.
   */
  
  var server = http.createServer(app);
  
  /**
   * Listen on provided port, on all network interfaces.
   */
  
  server.listen(port);
  server.on('error', onError);
  server.on('listening', onListening);
  
  /**
   * Normalize a port into a number, string, or false.
   */
  
  function normalizePort(val) {
    var port = parseInt(val, 10);
  
    if (isNaN(port)) {
      // named pipe
      return val;
    }
  
    if (port >= 0) {
      // port number
      return port;
    }
  
    return false;
  }
  
  /**
   * Event listener for HTTP server "error" event.
   */
  
  function onError(error) {
    if (error.syscall !== 'listen') {
      throw error;
    }
  
    var bind = typeof port === 'string'
      ? 'Pipe ' + port
      : 'Port ' + port;
  
    // handle specific listen errors with friendly messages
    switch (error.code) {
      case 'EACCES':
        console.error(bind + ' requires elevated privileges');
        process.exit(1);
        break;
      case 'EADDRINUSE':
        console.error(bind + ' is already in use');
        process.exit(1);
        break;
      default:
        throw error;
    }
  }
  
  /**
   * Event listener for HTTP server "listening" event.
   */
  
  function onListening() {
    var addr = server.address();
    var bind = typeof addr === 'string'
      ? 'pipe ' + addr
      : 'port ' + addr.port;
    debug('Listening on ' + bind);
  }
  ```

  