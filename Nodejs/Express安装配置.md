# Express

**本地Windows（win10专业版）环境：**

* ```
  node -v
  v10.15.3
  ```

* ```
  npm -v
  6.4.1
  ```

* ```
  where node
  C:\Program Files\nodejs\node.exe
  ```

* 不必切换目录，全局安装`npm install express -g`

* 4.x.x版本express  由于Express generator被划分为单独工具命令，所以这里需要安装express-generator,同理推荐全局模式安装。**（执行express --version,发下该错误）**

  ```
  npm install express-generator -g
  ```

* 安装express-generator后，帮助

  ```
    Usage: express [options] [dir]
  
    Options:
  
          --version        output the version number
      -e, --ejs            add ejs engine support
          --pug            add pug engine support
          --hbs            add handlebars engine support
      -H, --hogan          add hogan.js engine support
      -v, --view <engine>  add view <engine> support (dust|ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
          --no-view        use static html instead of view engine
      -c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
          --git            add .gitignore
      -f, --force          force on non-empty directory
      -h, --help           output usage information
  
    error: unknown option `-r'
  ```

* 查询安装后的版本

  ```
  express --version
  4.16.1
  ```

* 创建Express工程**（express默认使用的是jade模板，推荐ejs会更容易点）**

  ```
  //切换工作目录
  e:
  //切换ejs模板
  express --view ejs
  //创建效果
  e:\nodeexpress>express --view ejs
  
     create : public\
     create : public\javascripts\
     create : public\images\
     create : public\stylesheets\
     create : public\stylesheets\style.css
     create : routes\
     create : routes\index.js
     create : routes\users.js
     create : views\
     create : views\error.ejs
     create : views\index.ejs
     create : app.js
     create : package.json
     create : bin\
     create : bin\www
  
     install dependencies:
       > npm install
  
     run the app:
       > SET DEBUG=nodeexpress:* & npm start
  //如果此时直接执行命令
  e:\nodeexpress>npm start
  //提示如下错误
  e:\nodeexpress>npm start
  
  > nodeexpress@0.0.0 start e:\nodeexpress
  > node ./bin/www
  
  internal/modules/cjs/loader.js:584
      throw err;
      ^
  
  Error: Cannot find module 'http-errors'
      at Function.Module._resolveFilename (internal/modules/cjs/loader.js:582:15)
      at Function.Module._load (internal/modules/cjs/loader.js:508:25)
      at Module.require (internal/modules/cjs/loader.js:637:17)
      at require (internal/modules/cjs/helpers.js:22:18)
      at Object.<anonymous> (e:\nodeexpress\app.js:1:81)
      at Module._compile (internal/modules/cjs/loader.js:701:30)
      at Object.Module._extensions..js (internal/modules/cjs/loader.js:712:10)
      at Module.load (internal/modules/cjs/loader.js:600:32)
      at tryModuleLoad (internal/modules/cjs/loader.js:539:12)
      at Function.Module._load (internal/modules/cjs/loader.js:531:3)
  npm ERR! code ELIFECYCLE
  npm ERR! errno 1
  npm ERR! nodeexpress@0.0.0 start: `node ./bin/www`
  npm ERR! Exit status 1
  npm ERR!
  npm ERR! Failed at the nodeexpress@0.0.0 start script.
  npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
  npm WARN Local package.json exists, but node_modules missing, did you mean to install?
  
  npm ERR! A complete log of this run can be found in:
  npm ERR!     C:\用户\XXX\AppData\Roaming\npm-cache\_logs\2019-05-24T00_52_10_202Z-debug.log
  //回顾上面切换模板时的help
     install dependencies:
       > npm install
  
     run the app:
       > SET DEBUG=nodeexpress:* & npm start
  //需要在当前express工程目录下安装依赖，再运行
  e:\nodeexpress>npm install
  npm notice created a lockfile as package-lock.json. You should commit this file.
  added 53 packages from 38 contributors and audited 141 packages in 5.326s
  found 0 vulnerabilities
  //运行结果
  e:\nodeexpress>npm start
  
  > nodeexpress@0.0.0 start e:\nodeexpress
  > node ./bin/www
  
  GET / 200 16.261 ms - 207
  GET /stylesheets/style.css 200 7.280 ms - 111
  GET /favicon.ico 404 1.599 ms - 843
  GET / 200 1.369 ms - 207
  GET /stylesheets/style.css 200 0.763 ms - 111
  GET /favicon.ico 404 1.668 ms - 843
  //配置默认访问3000端口
  {
      // 使用 IntelliSense 了解相关属性。 
      // 悬停以查看现有属性的描述。
      // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
      "version": "0.2.0",
      "configurations": [
          {
              "type": "chrome",
              "request": "launch",
              "name": "Launch Chrome against localhost",
              "url": "http://localhost:3000",
              "webRoot": "${workspaceFolder}"
          }
      ]
  }
  //运行结果
  Express
  Welcome to Express
  出现此结果标明安装成功。
  ```

  

