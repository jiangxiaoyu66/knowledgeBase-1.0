# 事件循环

## 是什么？

事件循环机制是为了解决Js单线程问题而出现的处理异步任务的一种循环机制，不断从队列中读取任务进行处理。

## 执行机制

此机制具体如下:

主线程（或者说，执行栈）会不断从任务队列中按顺序取任务执行，

每执行完一个任务都会检查microtask队列是否为空（执行完一个任务的具体标志是函数执行栈为空）。

如果不为空则会一次性执行完所有microtask。然后再进入下一个循环去任务队列中取下一个任务执行。



1.在全局执行上下文中，先执行同步任务，然后微任务，然后依次执行宏任务

2.在宏任务中，先执行**当前宏任务中**的同步任务，然后清空**当前宏任务中**的微任务，然后执行**当前宏任务中**的宏任务

总结：在当前执行上下文中，先执行同步任务，然后微任务，然后宏任务（这里的同步任务，微任务和宏任务都只是针对当前执行上下文）

先看一个简单的示例：

```js
setTimeout(()=>{
    console.log("setTimeout1");
    Promise.resolve().then(data => {
        console.log(222);
    });
});
setTimeout(()=>{
    console.log("setTimeout2");
});
Promise.resolve().then(data=>{
    console.log(111);
});
```

思考一下, 运行结果是什么？

运行结果为:

```js
111
setTimeout1
222
setTimeout2
```



## 宏任务，微任务

宏任务（macrotask）也叫tasks，微任务（microtask）也叫jobs，JavaScript有两种队列，分别是macro task queue和micro task queue。任务将放置到对应的任务队列中。

  是宏任务的异步任务有：

- setTimeout
- setInterval
- setImmediate(Node)
- requestAnimationFrame(browser)
- UI rendering(browser)
- I/O

  是微任务的异步任务有：

- process.nextTick(Node)这个任务会插队到其他微任务前面
- Promise
- Object.observe
- MutationObserver









