## 项目初始化,安装基本依赖

```js
mkdir react-demo
cd react-demo

npm init  //这里基本一路回车即可；需要你的系统安装有 node。我安装的 node: v8.2.1、npm: 5.3.0。

$ npm i react react-dom webpack webpack-cli -D
```

## 创建项目基本结构

```
➜  react-demo tree . -L 1
.
├── build  // React 打包的出口, webpack 下默认打包在 dist 目录下，个人习惯放在 build 下
├── node_modules
├── package-lock.json
├── package.json
└── src // 源码文件，后面开发基本在这个目录下完成

```

## 配置webpack

新建webpack.config.js

```js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),  // 打包出口，即打包后的文件会放在这个目录下
    filename: '[name].[hash:8].js'   // 打包后的文件名
    publicPath: './', 	// 静态资源相对路径
  }
};

```

### 安装插件

```js
npm i html-webpack-plugin -D
```

```js
  plugins: [new HtmlWebpackPlugin({
	filename: 'index.html',   // 指定生成的文件名，默认就是 index.html
    	template: 'src/index.html'  // 指定 html 生成使用用的模版文件，我指定 使用 ```src/index.html``` 作为模版文件
  })]

```

模版文件 `src/index.html`， 内容如下：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hello React</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```








