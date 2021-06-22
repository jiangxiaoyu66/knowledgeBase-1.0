

## let，const，var的区别

- let和const之间

  1. let声明常量，const声明变量

  2. let声明时可以不赋值，const声明时候必须赋值

- var和let，const之间

  1. var存在变量提升，let和const不存在

  2. var可以重复声明，let和const不可以

  3. var声明的变量是全局作用域，const和let声明的变量是块级作用域

  ```js
    var a = 1;
    //  window.a = 1
  ```

  4. let和const存在暂时性死区

     

  ```js
  var value = "global";
  
  // 例子1
  (function() {
      console.log(value); // Uncaught ReferenceError: value is not defined
  
      let value = 'local';
  }());
  
  // 例子2
  {
      console.log(value); // Uncaught ReferenceError: value is not defined
  
      const value = 'local';
  };
  ```

  虽然在执行上下文中，所有的除形参和函数以外的变量都是先声明，后赋值的。但是let和const额外还有一个机制，叫做暂时性死区。即只有在执行过变量声明语句后，变量才可以被访问，否则就报错。

  

  

### 循环中的块级作用域

```js
var funcs = [];
for (var i = 0; i < 3; i++) {
    funcs[i] = function () {
        console.log(i);
    };
}
funcs[0](); // 3
```

#### es5解决方案：

```js
var funcs = [];
for (var i = 0; i < 3; i++) {
    funcs[i] = (function(i){
        return function() {
            console.log(i);
        }
    }(i))
}
funcs[0](); // 0
```

解决思路：

```js
// 伪代码

var funcs = [];

funcs[0] = function(0) {
    return function() {
        console.log(0)
    }
}

// 就相当于
funcs[0] = funciton() {
    console.log(0)
}

```

#### es6解决方案：

```js
var funcs = [];
for (let i = 0; i < 3; i++) {
    funcs[i] = function () {
        console.log(i);
    };
}
funcs[0](); // 0
```

es6解决思路：

> 那么当使用 let 的时候底层到底是怎么做的呢？
>
> 简单的来说，就是在 `for (let i = 0; i < 3; i++)` 中，即圆括号之内建立一个隐藏的作用域，这就可以解释为什么:
>
> ```js
> for (let i = 0; i < 3; i++) {
>   let i = 'abc';
>   console.log(i);
> }
> // abc
> // abc
> // abc
> ```

然后**每次迭代循环时都创建一个新变量，并以之前迭代中同名变量的值将其初始化**

就相当于：

```js
// 伪代码
(let i = 0) {
    funcs[0] = function() {
        console.log(i)
    };
}

(let i = 1) {
    funcs[1] = function() {
        console.log(i)
    };
}

(let i = 2) {
    funcs[2] = function() {
        console.log(i)
    };
};
```

##### 如果我们把 let 改成 const 呢

不过到这里还没有结束，如果我们把 let 改成 const 呢？

```js
var funcs = [];
for (const i = 0; i < 10; i++) {
    funcs[i] = function () {
        console.log(i);
    };
}
funcs[0](); // Uncaught TypeError: Assignment to constant variable.
```

结果会是报错，因为虽然我们每次都创建了一个新的变量，然而我们却在迭代中尝试修改 const 的值，所以最终会报错。

##### for in中的循环又会如何

```js
var funcs = [], object = {a: 1, b: 1, c: 1};
for (var key in object) {
    funcs.push(function(){
        console.log(key)
    });
}

funcs[0]()
```

结果是 'c';

那如果把 var 改成 let 或者 const 呢？

使用 let，结果自然会是 'a'，const 呢？ 报错还是 'a'?

结果是正确打印 'a'，这是因为在 for in 循环中，**每次迭代不会修改已有的绑定**，而是会创建一个新的绑定。

参考：[ES6 系列之 let 和 const](https://github.com/mqyqingfeng/Blog/issues/82#)#82

