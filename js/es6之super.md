## super指向问题

1.super作为对象，如super.x

作为对象时，只能在对象的方法中使用，指向父类的原型对象。

```js
const proto = {
  x: 'hello',
  foo() {
    console.log(this.x);
  },
};

const obj = {
  x: 'world',
  foo() {
    super.foo();
  }
}

Object.setPrototypeOf(obj, proto);

obj.foo() // "world"
```

2.super作为函数，如super()

作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。

>Class 可以通过`extends`关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
>
>```js
>class Point {
>}
>
>class ColorPoint extends Point {
>}
>```
>
>上面代码定义了一个`ColorPoint`类，该类通过`extends`关键字，继承了`Point`类的所有属性和方法。但是由于没有部署任何代码，所以这两个类完全一样，等于复制了一个`Point`类。下面，我们在`ColorPoint`内部加上代码。
>
>```js
>class ColorPoint extends Point {
>  constructor(x, y, color) {
>    super(x, y); // 调用父类的constructor(x, y)
>    this.color = color;
>  }
>
>  toString() {
>    return this.color + ' ' + super.toString(); // 调用父类的toString()
>  }
>}
>```
>
>上面代码中，`constructor`方法和`toString`方法之中，都出现了`super`关键字，它在这里表示父类的构造函数，用来新建父类的`this`对象。
>
>子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错。这是因为子类自己的`this`对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用`super`方法，子类就得不到`this`对象。

子类自己的`this`对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。

react里面类组件中，super(props)其实就是：

1. 执行父类的construtor，来创建子类的this对象

2. 将父类的实例对象和属性通过props传给子类

循环中的块级作用域


