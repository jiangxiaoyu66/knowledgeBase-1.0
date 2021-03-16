参考：

[ESLint里的规则教会我，无规矩 不编程](https://juejin.cn/post/6844903608379506701)

## 安装ESLint

现在前端项目开发中，都用到了webpack此等屌屌的构建工具([如果有对webpack不熟悉使用的可移步这里](https://juejin.im/post/6844903599080734728))，那么就结合着一起来看下如何使用到项目里吧

### 安装命令

```js
// 安装eslint
// eslint-loader 负责在webpack中解析
// babel-eslint  对Babel解析器的包装与ESLint兼容
// -D 安装在开发依赖环境 devDependencies 原--save-dev的简写
npm i eslint eslint-loader babel-eslint -D

```

**友情提示**：ESLint是基于Node的(当然webpack也是)，所以在使用之前，请确保Node已经安装

### 创建.eslintrc.js配置文件

(默认是.eslintrc文件)

```js
// .eslintrc.js
module.exports = {
    'parse': '',  // 指定解析器
    'parserOptions': {},   // 指定解析器选项
    'env': {},  // 指定脚本的运行环境
    'globals': {},  // 脚本在执行期间访问的额外的全局变量
    'rules': {} // 启用的规则及其各自的错误级别
};
```

### 在webpack中使用

将配置好的规则添加到webpack中对js文件检查

```js
// webpack.config.js

module.exports = {
    entry: '',
    output: {},
    module: {
        rules: [
            {
                test: /\.js$/,
                use: ['babel-loader', 'eslint-loader']
            }
        ]
    },
    plugins: [],
    devServer: {}
};
```







## 工作中用到的规则

### 级别

**非友情提示**：每个规则对应的0，1，2分别表示off, warning, error三个错误级别

### 常见规则api

- no-unused-vars： 定义了变量却没有在代码中使用，这是防止产生多余没用的变量

- semi：缺少分号，行尾必须使用分号，这是为了在压缩代码的时候出现意外情况

- no-console：禁止使用 console，提醒开发者，上线时要去掉，因为是warning不会导致编译的js出问题

- consistent-this： this的别名规则，只允许self和that，防止有些人写成_this或者me等等，哈哈

- curly： if 后必须包含 { ，单行 if 除外，也是为了方便阅读代码

  ```js
  // 错误写法
  function fn (key) {
     if (key === 'a') 
         return 1;
  }
  fn('a');
  // 正确写法
  function fn (key) {
     if (key === 'a') {
         return 1;
     }
     if (key === 'b') return 2;
  }
  fn('a');
  复制代码
  ```

- default-case：switch 语句必须包含 default

  ```js
  // 错误写法
  function fn (key) {
      let str = '';
  
      switch (key) {
          case 'a':
              str = 'a';
              break;
          case 'b':
              str = 'b';
              break;
      }
      return str;
  }
  // 正确写法
  function fn (key) {
      let str = '';
  
      switch(key) {
          case 'a':
              str = 'a';
              break;
          case 'b':
              str = 'b';
              break;
          default:
              str = 'c';
              break;
      }
      return str;
  }
  复制代码
  ```

- eqeqeq：必须使用全等===进行比较，防止隐式转换带来的意外问题

- guard-for-in： for in时需检测hasOwnProperty，避免遍历到继承来的属性方法

- max-depth：最大块嵌套不能超过5层

  ```js
  // 正确写法
  if () {
      if () {
          if () {
              if () {
                  if () {
                      
                  }
              }
          }
      }
  }
  复制代码
  ```

- max-params： 函数的形参不能多于8个,如果形参过多，我们现在可以用扩展运算符...来代替后面多余的形参

- new-cap： new关键字后类名应首字母大写，区分类和函数

- no-array-constructor： 禁止使用Array构造函数，定义数组直接用最快捷的方式[1, 2, 3]

- no-await-in-loop

  - 禁止将await写在循环里,循环属于同步操作，不该将await异步操作写在内部

- no-caller

  - 禁止使用arguments.caller和arguments.callee，ES6中废弃了

- no-const-assign

  - 禁止对const定义重新赋值

- no-delete-var

  - 禁止对变量使用delete关键字，delete只适用于对象的属性，提醒使用的范围

- no-dupe-args

  - 函数参数禁止重名

- no-empty-function

  - 禁止空的function,保证写的每一个function都有用

- no-eval

  - 禁止使用eval，eval是“魔鬼”，所以在开发中避免

- no-extra-semi

  - 禁止额外的分号，有些地方没必要加分号比如if () {};这样就是错误的

- no-global-assign

  - 禁止对全局变量赋值







