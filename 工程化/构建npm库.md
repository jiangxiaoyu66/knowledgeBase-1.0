参考：

[webpack 快速构建 React 学习环境（1）](https://juejin.cn/post/6844903629581713416)



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






## 写页面

```js
import React from 'react';
import ReactDOM from 'react-dom';

export default class HelloReact extends React.Component{
  constructor(props) {
    super(props);
  }

  render(){
    return( <div>Hello React</div>);
  }
}

ReactDOM.render(<HelloReact />, document.getElementById('root'));

```



## 编译 React 应用

安装 babel loader解析es6

```js
// # Webpack 接入 Babel 必须依赖的模块
npm i -D babel-core babel-loader 
// ＃根据我们的需求选择不同的 Plugins Presets
npm i -D babel-preset-env 
```

注意：7.0 之后，包名升级为 @babel/core。我的理解，`@babel` 相当于一种官方标记，和以前大家随便起名形成区别。

```js
yarn add @babel/core @babel/preset-env @babel/preset-react -D
```











## 配置less

### 安装依赖

```js
npm i css-loader style-loader less-loader -D
```









### webpack-config.js配置

```js
{
    test: /\.less$/i,
    loader: [
      // compiles Less to CSS
      "style-loader",
      "css-loader",
      "less-loader",
    ],
  },
```







### 问题出现：

```
一、问题的出现：
在进行 react 项目开发的时候，出现了这个错误，TypeError: this.getOptions is not a function

二. 问题的分析及解决：
问题的分析：这个实际上就是 less-loader 的版本过高，不兼容 getOptions 函数方法，所以需要对 less-loader 进行降级处理
问题的解决：通过 npm uninstall less-loader 命令卸载原版本的 less-loader，然后 通过 npm install less-loader@5.0.0 命令下载降级版本的 less-loader，这个问题就可以解决了

```

### 配置cssModule

#### 单配置less只能像下面这样用

![image-20210420114735596](https://gitee.com/jiang-xiaoyu/picture-bed-10/raw/master/images/image-20210420114735596.png)



