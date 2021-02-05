## JSX原理，为什么自定义的React组件必须大写

结论：

babel在编译过程中会判断 JSX 组件的首字母。

如果是小写，则当做原生的DOM标签解析，就编译成字符串。如果是大写，则认为是自定义组件，编译成对象。

解释：

JSX实际上是React.createElement(component, props, ...children)的语法糖。如下JSX代码

```js
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

会编译为

```js
React.createElement(MyButton, {
  color: "blue",
  shadowSize: 2
}, "Click Me");
```

注意：这里的MyButton不是字符串，而是一个对象

如果没有子节点，你还可以使用自闭合的标签形式，如

```js
<div className="sidebar" />
```

会编译为

```js
React.createElement("div", {
  className: "sidebar"
});
```