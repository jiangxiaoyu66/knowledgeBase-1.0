



## 虚拟DOM，diff算法

### 虚拟DOM是什么

虚拟DOM 就是使用一个 原生的JavaScript对象来描述 一个DOM节点。与真实 DOM 相比，更新 JavaScript 对象更加容易、快捷。

```js
<div id="wrap">
    <p class="text">好好学习，天天向上</p>
</div>
```

使用虚拟DOM表示如下：

```js
const element = {
  // 标签名
  tagName: 'div',
  properties: {
    id: 'wrap',
  },
  children: [
    {
      tagName: 'p',
      properties: {
        class: 'text',
        children: ['好好学习，天天向上']
      },
    }
  ]
}
```



### Diff算法

参考：

[**diff.md**](https://github.com/ChellyAI/note/blob/master/React/diff.md)

[官网原文](https://react-1251415695.cos-website.ap-chengdu.myqcloud.com/docs/reconciliation.html)

[diff算法](https://juejin.cn/post/6844903762859917320#heading-1)

#### 定义：

Diff算法就是比较虚拟dom树渲染前后的不同，从而对虚拟dom树进行更新的一种算法机制。

#### 核心比较机制

 传统的diff算法，是需要跨级对比两个树之间的不同，时间复杂度为O(n^3)

react提出的diff算法，只需要对比同级元素，这样算法复杂度就变成了O(n),时间复杂度大大减少

#### 具体如何比较：

1. 对两个虚拟dom树，从上往下，各个节点进行对比

   如果标签名发生改变，则直接替换

   如果标签名消失或者添加，则分别进行删除节点和添加节点的操作

   如果标签名未发生改变：

   ​	a. 对比属性，属性发生改变，则更新属性

   ​	b. 对比子节点，继续按照刚刚的流程进行对比

2. 当节点下面有循环渲染子节点的时候，如下列表：

   ```js
   <ul>
     <li>first</li>
     <li>second</li>
   </ul>
   ```

   当递归 `DOM` 节点的子元素时，`React` 会遍历列表。此时 `React` 会针对每个子元素进行差异比对。但这种方式下，特别是知识单纯添加或者删除某一个子节点的时候，这样比较会非常影响性能，开销会变大。

   为了解决以上问题，`React` 支持 `key` 属性，当子元素拥有了 `key` 时，`React` 使用 `key` 来匹配原有树上的子元素以及最新树上的子元素。加上了 `key` 值后，`React` 知道这次差异是新元素的插入和旧元素的移动。

   注意：

   1. 使用数组下标作为 `key` 值

      在元素不重新排序的前提下是可行的。

      但如果元素顺序发生改变，等于修改了元素的 `key` 值，原本的值和key就对应不上了，就会产生乱套的现象
      
     2. 使用不稳定的 `key` 值（例如通过 Math.random()生成的）会导致许多组件实例和 `DOM` 节点被不必要地重新创建，可能导致性能下降和子组件中的状态丢失。

    

    DOM-diff过程

   	1. 产生虚拟Dom
    	2. 虚拟Dom转化为真实Dom
    	3. diff算法，判断两个虚拟dom树的差异，得出差异对象
    	4. 蒋差异对象应用到真正的dom上

   >- 用JavaScript模拟DOM（虚拟DOM）
   >- 把虚拟DOM转换为真实的DOM插入页面中（render）
   >- 如果有事件发生修改了虚拟DOM，则比较两个虚拟DOM树的差异，得到差异对象diff
   >- 将差异对象应用到真正的DOM上（patch）



### React diff原理，如何从 O(n^3)变成 O(n)

### 为什么要使用Key，key有什么好处

在处理循环子节点的时候，

如果是增删节点，就不需要将之前的所有子节点破坏掉，然后新建所有子节点；只需要在对应位置添加需要增删的节点就可以了。

如果是修改节点，可以直接根据key值快速找到对应的节点，从而进行修改。

## 生命周期（16版本之前和之后的）

