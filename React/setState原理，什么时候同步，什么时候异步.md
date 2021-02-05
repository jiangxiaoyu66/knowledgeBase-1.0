## setState原理，什么时候同步，什么时候异步

结论：

在React中，如果是由React引发的事件处理（比如通过 onClick引发的事件处理，类似这种大写的都是react进行包装过的事件方法位的是批量处理setstate，从而优化性能，小写的比如onclick就属于原生事件），调用 setState 不会同步更新this.state，

除此之外的setState调用会同步执行this.state。所谓"除此之外”，指的是绕过React通过 addEventListener 直接添加的事件处理函数，还有通过 setTimeout/setInterval/promise产生的异步调用。

注意：17版本以前是这么搞得，但是17版本以后会尽量全部搞成批量。



原理：

在React的setState函数实现中，会根据一个变量isBatchingUpdates来判断是直接更新this.state还是放到队列中回头再说。

而isBatchingUpdates默认是false，也就表示setState会同步更新this.state。

但是，有一个函数batchedUpdates，这个函数会把isBatchingUpdates修改为true，而当React在调用事件处理函数之前就会调用这个batchedUpdates，造成的后果，就是由React控制的事件处理过程setState不会同步更新this.state。

参考：

[【面试题】React知识点整理(附答案)](https://github.com/funnycoderstar/blog/issues/129#)

[**setState.md**](https://github.com/ChellyAI/note/blob/master/React/setState.md)

[好评论](https://stackoverflow.com/questions/48563650/does-react-keep-the-order-for-state-updates/48610973#48610973)

[你真的理解setState吗？](https://juejin.cn/post/6844903636749778958#heading-3)