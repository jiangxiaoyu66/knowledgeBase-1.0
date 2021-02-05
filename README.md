## 数据类型

七种基本数据类型，一种引用数据类型

- undefined，null，string，number，boolean，symbol，bigInt
- object

NaN 是number中的一个特殊的值，可以通过Number.NaN 来访问



### Symbol

表示独一无二的值。它通过 `Symbol` 函数生成。如果对象使用 `Symbol` 属性名的类型，可以保证不会跟其他属性名冲突。

注意：

1. **Symbol作为属性名时，不能用点运算符。**

	这是会因为点运算符后面总是字符串，所以不会读取 `Symbol` 作为标识名所指代的那个值。
	
	所以在手写 call、apply、bind 的时候使用的 Symbol 值需要用 `obj[Symbol]` 来执行对应的函数。

2. **Symbol作为属性名时，不能被遍历。**
   	`Symbol` 作为属性名，遍历对象的时候，该属性不会出现在 `for...in`、`for...of` 循环中，也不会被 `Object.keys()、Object.getOwnPropertyNames()、JSON.stringfy()`返回。

  但它也不是私有属性，有一个 `Object.getOwnPropertySymbols()` 方法，可以获取指定对象的所有 `Symbol` 属性名，该方法返回一个数组，成员是当前对象的所有用作属性名的 `Symbol` 值。



### 数据存储机制

JavaScript的存储空间分为栈(stack)、堆(heap)和池(一般也归类为栈)，其中栈存放变量，堆存放复杂对象，池存放常量。

- 基础数据类型都有固定大小，它们由系统自动分配空间，可以直接操作保存在栈内存空间的值，因此**基础数据类型都是按值来访问的**；
- 引用数据类型的大小不固定，他们的值是保存在堆内存中的对象，JavaScript不允许直接访问堆内存中的位置，在操作对象时我们是在操作对象的引用，因此**引用类型的值是按照引用访问的**，可以理解为对象的地址指针。

### 类型判断

- typeof可以正确判断除了null以外的基本数据类型，可以判断function的对象类型
-  instanceof也不准确，他的原理是查看原型是否在原型链上（如：A instanceof B是用来判断B的原型是否在A的原型链上）

>b instanceof B（b为实例，B为构造函数）
>
>原理：测试构造函数的prototype是否在实例的原型链上
>
>### 手写实现instanceof
>
>```js
>while (x.__proto__) {
>  if (x.__proto__ === y.prototype) {
>    return true;
>  }
>  x.__proto__ = x.__proto__.__proto__;
>}
>if (x.__proto__ === null) {
>  return false;
>}
>```
>
>参考：
>
>[[js\] 第76天 说说instanceof和typeof的实现原理并自己模拟实现一个instanceof](https://github.com/haizlin/fe-interview/issues/528#)
>
>[instanceof 的实现原理](https://github.com/a1029563229/InterviewQuestions/tree/master/javascript/36)
>
>

#### 各种数据的判断

##### 判断引用数据类型

>对于引用数据类型，大概三个方案：
>
>通过原型链：instanceof，arr.constructor，
>
>字符串：toString

判断数组

- 使用`Array.isArray()`判断数组
- 使用`[] instanceof Array`判断是否在Array的原型链上，即可判断是否为数组
- `[].constructor === Array`通过其构造函数判断是否为数组
- 也可使用`Object.prototype.toString.call([])`判断值是否为'[object Array]'来判断数组

判断对象

- `Object.prototype.toString.call({})`结果为'[object Object]'则为对象
- `{} instanceof Object`判断是否在Object的原型链上，即可判断是否为对象
- `{}.constructor === Object`通过其构造函数判断是否为对象

判断函数

- 使用`func typeof function`判断func是否为函数
- 使用`func instanceof Function`判断func是否为函数
- 通过`func.constructor === Function`判断是否为函数
- 也可使用`Object.prototype.toString.call(func)`判断值是否为'[object Function]'来判断func

判断null

- 最简单的是通过`null===null`来判断是否为null
- `(!a && typeof (a) != 'undefined' && a != 0 && a==a)`判断a是否为null
- `Object.prototype.__proto__===a`判断a是否为原始对象原型的原型即null
- `typeof (a) == 'object' && !a`通过typeof判断null为对象，且对象类型只有null转换为Boolean为false

##### 判断基本数据类型

判断null

- 最简单的是通过`null===null`来判断是否为null
- `Object.prototype.__proto__===a`判断a是否为原始对象原型的原型即null
- `typeof (a) == 'object' && !a`通过typeof判断null为对象，且对象类型只有null转换为Boolean为false

判断是否为NaN

- `isNaN(any)`直接调用此方法判断是否为非数值

一些其他判断

- `Object.is(a,b)`判断a与b是否完全相等，与===基本相同，不同点在于Object.is判断`+0不等于-0`，`NaN等于自身`



### ==和===的区别

#### ===

属于**严格判断**，判断两者值是否相等

#### ==

先看类型是否一致

不一致则**进行隐式转换**，一致则判断值的大小，得出结果

来看下《Javascript高级程序设计》关于==和===的规则

> 1.如果有一个操作数是布尔值，则在比较前先将其转换为数值，true转换为1，false转换为0，例如false == 0，true == 1
> 2.如果一个操作数是字符串，另一个操作数是数值，先将字符串转换成数值，例如"1"==1,'' ==0
> 3.如果一个操作数是对象，另一个操作数不是，则调用对象的valueOf()方法，用得到的基本类型按照前面的规则进行比较。**(解释不清楚)**
> 4.null和undefined是相等的。
> 5.如果有一个数是NaN，则相等操作符返回false，而不相等操作符返回true。NaN == NaN返回为false，因为规则如此。
> 6.如果两个操作数是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回true，否则返回false。
> 例如：var obj = {a:1};foo = obj;bar = obj;foo==bar;foo==bar返回为true，因为他们指向同一个对象，obj。

即：

- boolean则转为数值
- 对象和基本数据类型：valueOf()
- 有NaN，则结果为false
- 对象和对象：看引用地址是否一样
- null == undefined

![image-20210115174901723](https://i.loli.net/2021/01/15/XG2W1RKxSVqwd3n.png)





### 拷贝

参考：

[深拷贝和浅拷贝](https://zhuanlan.zhihu.com/p/146664126)

[浅拷贝与深拷贝](https://juejin.cn/post/6844904197595332622#heading-0)

浅拷贝和深拷贝**只针对引用数据类型**

#### 赋值

赋值，赋的其实是该对象的在栈中的地址，而不是堆中的数据。

#### 浅拷贝

浅拷贝只拷贝第一层属性，对于第一层的引用类型的值无法拷贝。

拷贝的是对象第一层，第二层开始，引用类型只复制地址，不复制值。

只复制对象的第一层空间（如果第一层空间中有引用数据类型，那么拷贝的只是他的地址；如果是剧本数据类型，就直接拷贝的是值）

#### 深拷贝

深拷贝是拷贝的引用对象的值，而不是地址。

>总而言之，浅拷贝只复制指向某个对象的指针，而不复制对象本身，**新旧对象还是共享同一块内存**。但深拷贝会另外创造一个一模一样的对象，**新对象跟原对象不共享内存**，修改新对象不会改到原对象。

![v2-650f96aa6b2566339702f3250b1a98d7_720w](https://i.loli.net/2021/01/17/zUr2q7HtWLSTDv5.jpg)



#### 浅拷贝的实现方式：

1.Object.assign()

`**Object.assign()**` 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
let obj1 = { person: {name: "kobe", age: 41},sports:'basketball' };
let obj2 = Object.assign({}, obj1);
obj2.person.name = "wade";
obj2.sports = 'football'
console.log(obj1); // { person: { name: 'wade', age: 41 }, sports: 'basketball' }
```

2.数组的拼接和裁剪

Array.prototype.concat()

```js
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2 = arr.concat();    
arr2[2].username = 'wade';
console.log(arr); //[ 1, 3, { username: 'wade' } ]
```

Array.prototype.slice()

```js
let arr = [1, 3, {
    username: ' kobe'
    }];
let arr3 = arr.slice();
arr3[2].username = 'wade'
console.log(arr); // [ 1, 3, { username: 'wade' } ]
```

3.lodash的clone方法

```js
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.clone(obj1);
console.log(obj1.b.f === obj2.b.f);// true
```

4.es6的展开运算符

```js
let obj1 = { name: 'Kobe', address:{x:100,y:100}}
let obj2= {... obj1}
obj1.address.x = 200;
obj1.name = 'wade'
console.log('obj2',obj2) // obj2 { name: 'Kobe', address: { x: 200, y: 100 } }	
```

#### 深拷贝的实现方式：

1.手写递归

```js
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就把对象的值放进去
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
let obj = { name: 1, address: { x: 100 } };
obj.o = obj; // 对象存在循环引用的情况
let d = deepClone(obj);
obj.address.x = 200;
console.log(d);
```

https://juejin.cn/post/6844904197595332622#heading-13

2.lodash中cloneDeep()

3.JSON.parse(JSON.stringify())

```js
let arr = [1, 3, {
    username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)
```

**这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则**，因为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）了。

比如下面的例子：

```js
let arr = [1, 3, {
    username: ' kobe'
},function(){}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)
```

![image-20210117221407301](https://i.loli.net/2021/01/17/sHF8D16rXCANfTS.png)









### 面试题

#### null和undefined的区别

意义上来说，null代表“没有对象”，没有值； undefined代表“缺少值”，声明了但是未赋值

用法上：

```js
// null
1.原型链的终点
2.作为函数参数，代表参数不是对象

// undefined
1.变量声明但是没有赋值
2.如果函数调用未给参数，则是undefined
3.函数默认返回值为undefined

```

#### null是对象吗

虽然 typeof null 返回的值是 object,但是null不是对象，而是基本数据类型的一种

#### 基本数据类型和复杂数据类型的区别

- 基本数据类型存储在栈内存，存储的是值。
- 复杂数据类型的值存储在堆内存，地址（指向堆中的值）存储在栈内存。当我们把对象赋值给另外一个变量的时候，复制的是地址，指向同一块内存空间，当其中一个对象改变时，另一个对象也会变化。

