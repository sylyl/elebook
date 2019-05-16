# GitBook基本配置（Windows环境）

1. 安装Node.js环境（Windows环境）

   - 根据PC系统环境，选择相应的版本
   - 安装成功后cmd敲入`node -v`会显示出当前系统的安装版本`v10.15.3`

2. 安装GitBook（Windows环境）

   - `npm install gitbook-cli -g`	此处需要注意的无法翻墙的，请自觉更改npm源。

     **修改npm源**

     以修改为淘宝镜像为例（最好以管理员身份运行cmd，**非管理员未亲测**）

     - `npm config set registry https://registry.npm.taobao.org`
     - 查看当前源`npm config get registry`

   - 安装完成后，敲入`gitbook -V`命令检测版本，如下所示：

     ```
     CLI version: 2.3.2
     GitBook version: 3.2.3
     //初始化会生成Summry.md和ReadMe.md两个默认文件。
     gitbook init
     //预览静态服务，默认端口4000
     gitbook serve .
     //创建生成静态网站
     gitbook build
     ```
     
     **生成目录最好非英文，目前支持二级目录。**
     
     **写完Summary.md需要使用`gitbook init`命令初始化一下，必要时候需要备份。**
     
     **目前亲测无法删除introduction目录**
     
     **使用`gitbook serve .`命令有时候会出现错误提示：`no such the file or directory...`这种提示，没有关系继续在执行一次该命令就可以。**

3. 安装GitBook编辑器（Windows环境）

   - 推荐使用Typora（64/32版本)，支持markdown语法。

