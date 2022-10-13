本博文是webpack的学习笔记📒，主要来自课程[从基础到实战 手把手带你掌握新版Webpack4.0](https://coding.imooc.com/class/316.html)以及其他博客分享。

[Webpack官网](https://webpack.js.org/)

## Webpack究竟是什么？

> At its core, **webpack** is a *static module bundler* for modern JavaScript applications. When webpack processes your application, it internally builds a [dependency graph](https://webpack.js.org/concepts/dependency-graph/) from one or more *entry points* and then combines every module your project needs into one or more *bundles*, which are static assets to serve your content from.

![image-20220703110355900](http://qny.mrpwei.cc/uPic/image-20220703110355900.png)



原生浏览器不认识ES Moudule引入方式，只能以script标签的方式引入，不利于拆分模块、debug。

```js
// ES Moudule模块引入方式
import Header from "./header.js"
```

```js
function Header() {
  var dom = document.getElementById('root');
  var header = document.createElement('div');
	header.innerText = 'header';
  dom.append(header);
}

export default Header;
```

需要webpack进行翻译：

```js
npx webpack index.js
```

运行后根目录会出现一个新的文件夹dist，里面有翻译好的main.js文件。  



Webpack同样可以识别CommonJS模块引入方式：

```js
// CommonJS模块引入方式
var Header= require('./header.js')
```

```js
function Header() {
  var dom = document.getElementById('root');
  var header = document.createElement('div');
	header.innerText = 'header';
  dom.append(header);
}

modules.exports = Header;
```



Webpack是一个**模块打包器**，可以识别多种模块引入方式。



## 搭建Webpack环境

💡webpack入门：https://webpack.js.org/guides/getting-started/

在当前项目里面安装好webpack后，输入webpack命令会提升command not found。因为node会默认在全局去寻找命令，要查找当前项目的包，可以使用npx：

```bash
webpack -v // command not found
npx webpack -v // 4.26.0
```



```bash
npm info webpack // 查看webpack信息，里面有历史版本
```



https://webpack.js.org/guides/getting-started/#using-a-configuration

若不配置webpack.config.js，则会使用默认的配置，需要手动指定入口文件：

```bash
npx webpack index.js
```

配置后则不需要：

```bash
npx webpack
```



mode设置成development时打包出来的代码不会被压缩。

```js
const path = require('path');

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```



