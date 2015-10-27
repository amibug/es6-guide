es6-guide
=========

ECMAScript 6 学习笔记

### let命令
1.  let声明的变量，只有所在的代码块内有效，let不像var一样，存在变量提升和函数提升现象
2.  封闭作用域 只要块级作用域内存在let命令，它所声明的变量，不再受外部的影响，没有声明就不能使用
    ```
    var tmp =123;if(true){
      tmp ='abc'; // ReferenceError
    let tmp;}
    ```

3.  在相同作用域内，let不允许同时声明一个变量
4.  如果在严格模式下，函数只能在顶层作用域和函数内声明，其他情况（比如if代码块、循环代码块）的声明都会报错
5.  let命令、const命令、class命令声明的全局变量，不属于全局对象的属性

    ````
    var a =1;
    // 如果在Node的REPL环境，可以写成global.a
    // 或者采用通用方法，写成this.a
    window.a // 1
    let b =1;
    window.b // undefined
    ````

### const命令
1.  const也用来声明变量，但是声明的是常量,常量也是不提升的。一旦声明，常量的值就不能改变，对常量重新赋值不会报错，只会默默地失败


### 变量解构赋值
1.  模式匹配的写法，左边的变量会被赋予对应的值，解构赋值适用于var,let,const命令,ES6的规则是，只要有可能导致解构的歧义，就不得使用圆括号。

    ````
    let[x,, y]=[1,2,3];
    x // 1
    y // 3
    let[a,[b], d]=[1,[2,3],4];
    a // 1
    b // 2
    d // 4
    var{ bar, foo }={ foo:"aaa", bar:"bbb"};
    foo // "aaa"
    bar // "bbb"
    ````
2.  ES6内部使用严格相等运算符（===），判断一个位置是否有值。只有那个位置上的值不严格等于undefined，才能解构赋值。对于没法解构的变量可以设置默认值

    ````
    var[x =1]=[undefined];
    x // 1var[x =1]=[null];
    x // null
    var{x =3}={x: undefined};
    x // 3var{x =3}={x:null};
    x // null
    ````
3.  函数参数的解构赋值

    ````
    functionmove({x =0, y =0}={}){return[x, y];}move({x:3, y:8}); // [3, 8]
    move({x:3}); // [3, 0]
    move({}); // [0, 0]
    move(); // [0, 0]
    ````
##### 解构赋值的用途
1. 交换变量的值

    ````
    [x, y] = [y, x];
    ````
2.  函数返回多个值

    ```
    function example(){return[1,2,3];}var[a, b, c]= example();
    // 返回一个对象
    function example(){return{
        foo:1,
        bar:2};
    }
    var{ foo, bar }= example();
    ````
3.  函数参数的定义，函数调用时不用关注参数的顺序

    ````
    // 参数是一组无次序的值
    function f({x, y, z}){...}
    f({x:1, z:2, y:3})
    ````
4.  提取json数据，精简数据

    ````
    var jsonData = {
      id: 42,
      status: "OK",
      data: [867, 5309]
    }

    let { id, status, data: number } = jsonData;

    console.log(id, status, number)
    // 42, OK, [867, 5309]
    ````
5.  设置默认值,不需要如此设置默认值，var foo = config.foo || 'default foo';

    ````
    {
      async =true,
      beforeSend =function(){},
      cache =true,
      complete =function(){},
      crossDomain =false,
      global =true, // ... more config
    }
    ````
6.  遍历Map结构，实现了Iterator接口的对象都可以使用for of操作。

    ````
    var map =newMap();
    map.set('first','hello');
    map.set('second','world');for(let[key, value] of map){
      console.log(key +" is "+ value);}
    // first is hello
    // second is world
    ````
7.  输入模块的指定方法

    ````
    加载模块时，往往需要指定输入那些方法。解构赋值使得输入语句非常清晰
    const { SourceMapConsumer, SourceNode }=require("source-map");
    ````

### 字符串的扩展
1.  String原型扩展
    - stratsWith
    - endsWith
    - includes
    - repeat
2.  模板字符串

    ````
    var name ="Bob", time ="today";
    `Hello ${name}, \'引号\'how are you ${time}?` // Hello Bob, '引号'how are you today?
    ${name}作为插值，输出表达式结果
    ````
    
### 数值扩展
1.  Number
    * Number.isFinite()用来检查一个数值是否非无穷（infinity）
    * Number.isNaN()用来检查一个值是否为NaN。
    * 与传统的全局方法isFinite()和isNaN()的区别在于，传统方法先进行类型装换，调用Number()将非数值的值转为数值，再进行判断，而这两个新方法只对数值有效，非数值一律返回false
    * Number.parseInt(), Number.parseFloat(),全局方法parseInt()和parseFloat()行为完全保持不变
    * Number.isInteger()用来判断一个值是否为整数。需要注意的是，在JavaScript内部，整数和浮点数是同样的储存方法，所以3和3.0被视为同一个值。
    * Number.EPSILON 极小的常量Number.EPSILON
    * Number.isSafeInteger()准确表示的整数范围在-2^53到2^53之间（不含两个端点）

2.  新增指数运算符（**, **=）
    ````
    2**2 // 4
    2**3 // 8
    let b =3;
    b **=3;
    // 等同于 b = b * b * b;
    ````
### 数组扩展
1.  Array.form()
    * Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）比如arguments、NodeList和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）,任何有length属性的对象。
    ````
    Array.from('hello')

    // ['h', 'e', 'l', 'l', 'o']
    Array.from([1, 2, 3])
    // [1, 2, 3]
    let namesSet = new Set(['a', 'b'])
    Array.from(namesSet) // ['a', 'b']
    let ps = document.querySelectorAll('p');
    Array.from(ps).forEach(function (p) {
        console.log(p);
    });
    function foo() {
        var args = Array.from(arguments);
    }
    ````

    *  Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理。
    ````
    Array.from(arrayLike, x => x * x);
    // 等同于
    Array.from(arrayLike).map(x => x * x);

    function typesOf () {
      return Array.from(arguments, value => typeof value)
    }
    typesOf(null, [], NaN)
    // ['object', 'object', 'number']

    Array.from({ length: 2 }, () => 'jack')
    // ['jack', 'jack']
    ````

2.  Array.of()
    * Array.of方法用于将一组值，转换为数组,Array.of基本上可以用来替代new Array()
    ````
    Array.of(3, 11, 8) // [3,11,8]
    Array.of(3) // [3]
    Array.of(3).length // 1
    Array() // []
    Array(3) // [undefined, undefined, undefined]  只有当参数个数不少于2个，Array()才会返回由参数组成的新数组
    Array(3, 11, 8) // [3, 11, 8]
    ````

3.  copyWithin()
4.  find(), findIndex() 
    * 用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员。indexOf 内部使用严格相当运算符（===）进行判断，这会导致对NaN的误判。
    ````
    [NaN].indexOf(NaN)
    // -1

    [NaN].findIndex(y => Object.is(NaN, y))
    // 0
    ````
    
5.  fill()
6.  entries()，keys()和values()
    * 用for...of循环进行遍历，唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历
    ````
    for (let index of ['a', 'b'].keys()) {
      console.log(index);
    }
    // 0
    // 1

    for (let elem of ['a', 'b'].values()) {
      console.log(elem);
    }
    // 'a'
    // 'b'

    for (let [index, elem] of ['a', 'b'].entries()) {
      console.log(index, elem);
    }
    // 0 "a"
    // 1 "b"
    ````

7.  includes() 数组是否包含给定的值
8.  数组的空位
    * 数组的空位指，数组的某一个位置没有任何值，空位不是undefined，一个位置的值等于undefined，依然是有值的。空位是没有任何值，in运算符可以说明这一点。
    ````
    // 第一个数组的0号位置是有值的，第二个数组的0号位置没有值。
    0 in [undefined, undefined, undefined] // true
    0 in [, , ,] // false
    ````

    * ES5对空位的处理，已经很不一致了，大多数情况下会忽略空位。
    ````
    // forEach(), filter(), every() 和some()都会跳过空位。
    // forEach方法
    [,'a'].forEach((x,i) => log(i)); // 1
    // filter方法
    ['a',,'b'].filter(x => true) // ['a','b']
    // every方法
    [,'a'].every(x => x==='a') // true
    // some方法
    [,'a'].some(x => x !== 'a') // false
    // map方法 map()会跳过空位，但会保留这个值
    [,'a'].map(x => 1) // [,1]
    // join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。
    // join方法 
    [,'a',undefined,null].join('#') // "#a##"
    // toString方法
    [,'a',undefined,null].toString() // ",a,,"
    ````

    * ES6则是明确将空位转为undefined。
    ````
    Array.from方法会将数组的空位，转为undefined，也就是说，这个方法不会忽略空位。
    Array.from(['a',,'b'])
    // [ "a", undefined, "b" ]
    扩展运算符（...）也会将空位转为undefined。
    [...['a',,'b']]
    // [ "a", undefined, "b" ]
    copyWithin()会连空位一起拷贝。
    [,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
    fill()会将空位视为正常的数组位置。
    new Array(3).fill('a') // ["a","a","a"]
    entries()、keys()、values()、find()和findIndex()会将空位处理成undefined。
    // entries()
    [...[,'a'].entries()] // [[0,undefined], [1,"a"]]
    // keys()
    [...[,'a'].keys()] // [0,1]
    // values()
    [...[,'a'].values()] // [undefined,"a"]
    // find()
    [,'a'].find(x => true) // undefined
    // findIndex()
    [,'a'].findIndex(x => true) // 0
    ````
    *空位的处理规则非常不统一，建议避免出现空位*
9.  数组推导(ES7)，可以等价于map()和filter()
10. Array.observe()，Array.unobserve()(ES7)，用于监听（取消监听）数组的变化，指定回调函数。

### 函数扩展
1.  函数默认值设置
    参数默认值可以与解构赋值，联合起来使用，参数默认值可以与解构赋值，联合起来使用定义了默认值的参数，必须是函数的尾参数，其后不能再有其他无默认值的参数。这是因为有了默认值以后，该参数可以省略，只有位于尾部，才可能判断出到底省略了哪些参数。
    ```
    ES6之前，当实参（0, ''）类型隐式转为false时，参数还是被设置成默认值，这不是我们想要的
    function log(x, y) {
      y = y || 'World';
      console.log(x, y);
    }

    log('Hello', '') // Hello World
    log('Hello', 0) // Hello World
    log('Hello', 'China') // Hello China

    ES6 实参为undefined，将触发该参数等于默认值，null则没有这个效果
    function log(x, y = 'World') {
      console.log(x, y);
    }
    log('Hello', '') // Hello ''
    log('Hello', 0) // Hello 0
    log('Hello', 'China') // Hello China
    ```

2.  reset参数
    rest参数（形式为“...变量名”），rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错，用于获取函数的多余参数，这样就不需要使用arguments对象了。rest参数搭配的变量是一个数组，该变量将多余的参数放入数组中
    **rest参数代替arguments变量**
    ```
    // arguments变量的写法
    const sortNumbers = () =>
      Array.prototype.slice.call(arguments).sort();

    // rest参数的写法
    const sortNumbers = (...numbers) => numbers.sort();
    ```

    ```
    // 将剩余的实参包装成一个array-items
    function push(array, ...items) {
      items.forEach(function(item) {
        array.push(item);
        console.log(item);
      });
    }

    var a = [];
    push(a, 1, 2, 3)
    ```

3.  扩展知识要点spread运算符(...)
    ```
    在函数新参中使用（reset参数的用法，items会初始化成数组）
    function push(array, ...items) {
      items.forEach(function(item) {
        array.push(item);
        console.log(item);
      });
    }
    var a = [];
    push(a, 1, 2, 3);

    const [first, ...rest] = ["foo", "bar", "baz"];
    first // "foo"
    rest  // ["bar","baz"]

    操作数组，将一个数组转为用逗号分隔的参数序列
    var numbers = [4, 38];
    add(...numbers) // 42
    等同于
    add(4, 38) // 42
    ```

4.  箭头函数=>
    ```
    (a, b)=>{ a + b }
    等同于
    function(a, b){
        return a + b;
    }
    参数只有一个时，()可以省略
    函数体只有一个语句时，{}可以省略
    ```

    **箭头函数有几个使用注意点**
    * 函数体内的this对象，绑定定义时所在的对象，而不是使用时所在的对象，**在箭头函数中，它是固定的，不想其他函数可以通过bind设置**。
    * 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
    * 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
    * 不可以使用yield命令，因此箭头函数不能用作Generator函数。
5.  函数绑定
    ```
    foo::bar;
    // 等同于
    bar.call(foo);

    foo::bar(...arguments);
    i// 等同于
    bar.apply(foo, arguments);
    ```

### 对象扩展
1.  属性的简洁表示法
    ES6定义对象，允许只写属性名，这时候属性值等于属性名所代表的变量   
    ```
    var Person = {
      name: '张三',
      //等同于birth: birth
      birth,
      // 等同于hello: function ()...
      hello() { console.log('我的名字是', this.name); }
    };
    ```

2.  属性名表达式
    ES6允许字面量定义对象时，用表达式作为对象的属性名，即把表达式放在方括号内。
    ```
    let propKey = 'foo';
    let obj = {
      [propKey]: true,
      ['a' + 'bc']: 123
    };
    ```
3.  方法的name属性
    ```
    var person = {
      sayName: function() {
        console.log(this.name);
      },
      get firstName() {
        return "Nicholas"
      }
    }

    person.sayName.name   // "sayName"
    person.firstName.name // "get firstName"
    (new Function()).name // "anonymous"

    var doSomething = function() {
      // ...
    };
    doSomething.bind().name // "bound doSomething"
    const key1 = Symbol('description');
    const key2 = Symbol();{},

    let obj = {
      [key1]()       [key2]() {},
    };
    obj[key1].name // "[description]"
    obj[key2].name // ""
    ```

4.  Object.is() 
    用来比较两个值是否严格相等。它与严格比较运算符（===）的行为基本一致。
    ```
    // 不同之处
    +0 === -0 //true
    NaN === NaN // false

    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true
    ```

 5. Object.assign()
    Object.assign方法用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。它只拷贝自身属性，不可枚举的属性（enumerable为false）和继承的属性不会被拷贝。
    ```
    var target = { a: 1, b: 1 };
    var source1 = { b: 2, c: 2 };
    var source2 = { c: 3 };
    Object.assign(target, source1, source2);
    target // {a:1, b:2, c:3}
    ```

    对于嵌套的对象，Object.assign的处理方法是替换，而不是添加。
    ```
    var target = { a: { b: 'c', d: 'e' } }
    var source = { a: { b: 'hello' } }
    Object.assign(target, source)
    // { a: { b: 'hello' } }
    // 不会得到{ a: { b: 'hello', d: 'e' } }
    ```

    **Object.assign的用处**
    * 为象添加属性和方法
    ```
    Object.assign(SomeClass.prototype, {
      xxx,
      anotherMethod() {
        ···
      }
    });
    ```

    * 克隆对象
    ```
    function clone(origin) {
      return Object.assign({}, origin);
    }
    // 克隆原型链上的属性    
    function clone(origin) {
      let originProto = Object.getPrototypeOf(origin);
      return Object.assign(Object.create(originProto), origin);
    }
    ```

    * 合并对象

6.  属性的可枚举性
    描述对象的enumerable属性，称为”可枚举性“，如果该属性为false，就表示某些操作会忽略当前属性。

    **ES5有三个操作会忽略enumerable为false的属性。**
    * for...in 循环：只遍历对象自身的和继承的可枚举的属性
    * Object.keys()：返回对象自身的所有可枚举的属性的键名
    * JSON.stringify()：只串行化对象自身的可枚举的属性

    **ES6新增了两个操作，会忽略enumerable为false的属性。**
    * Object.assign()：只拷贝对象自身的可枚举的属性
    * Reflect.enumerate()：返回所有for...in循环会遍历的属性
    
    这五个操作之中，只有for...in和Reflect.enumerate()会返回继承的属性。实际上，引入enumerable的最初目的，就是让某些属性可以规避掉for...in操作。比如，对象原型的toString方法，以及数组的length属性，就通过这种手段，不会被for...in遍历到。

7.  __proto__属性，Object.setPrototypeOf()，Object.getPrototypeOf()
    虽然目前，所有浏览器（包括IE11）都支持了__proto__这个属性，无论从语义的角度，还是从兼容性的角度，都不要使用这个属性，而是使用下面的Object.setPrototypeOf()（写操作）、Object.getPrototypeOf()（读操作）、Object.create()（生成操作）代替

    **Object.setPrototypeOf()**
    ```
    let proto = {};
    let obj = { x: 10 };
    // obj.__proto__ = proto;
    Object.setPrototypeOf(obj, proto);
    proto.y = 20;
    proto.z = 40;
    obj.x // 10
    obj.y // 20
    obj.z // 40
    ```

    **Object.getPrototypeOf()**
    ```
    function Rectangle() {
    }

    var rec = new Rectangle();
    Object.getPrototypeOf(rec) === Rectangle.prototype
    ```

8.  Object.observe()，Object.unobserve()
    Object.observe方法目前共支持监听六种变化。
    add：添加属性
    update：属性值的变化
    delete：删除属性
    setPrototype：设置原型
    reconfigure：属性的attributes对象发生变化
    preventExtensions：对象被禁止扩展（当一个对象变得不可扩展时，也就不必再监听了）

### Symbol

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。它是JavaScript语言的第七种数据类型，前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）
Symbol不是一个构造器，不能用new操作符。Symbol函数的参数只是表示对当前Symbol值的描述，相同参数的Symbol函数的返回值是不相等的。
```
// 传参数是为了在控制台显示，或者转为字符串时，比较容易区分
var s1 = Symbol('foo');
var s2 = Symbol('bar');
s1 // Symbol(foo)
s2 // Symbol(bar)
s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"

```

**作为属性名**
可以用表达式的地方就可以使用Symbol类型的变量
```
var mySymbol = Symbol();
// 第一种写法
var a = {};
a[mySymbol] = 'Hello!';
// 第二种写法
var a = {
  [mySymbol]: 'Hello!'
};
// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });
// 以上写法都得到同样结果
a[mySymbol] // "Hello!"

// a的结构
Object {Symbol(): "Hello!"}
```

**属性名的遍历**
Symbol类型的值作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有Symbol属性名
```
var obj = {};
var foo = Symbol("foo");
Object.defineProperty(obj, foo, {
  value: "foobar",
});
for (var i in obj) {
  console.log(i); // 无输出
}
Object.getOwnPropertyNames(obj)
// []
Object.getOwnPropertySymbols(obj)
// [Symbol(foo)]
```
Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和Symbol键名
 ```
    let obj = {
      [Symbol('my_key')]: 1,
      enum: 2,
      nonEnum: 3
    };
Reflect.ownKeys(obj)
// [Symbol(my_key), 'enum', 'nonEnum']
```

**Symbol.for()，Symbol.keyFor()**
Symbol.for接受一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值。如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值
```
Symbol.for("bar") === Symbol.for("bar")
// true
Symbol("bar") === Symbol("bar")
// false

var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"
var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

### Proxy
Proxy用于修改某些操作的默认行为,Proxy可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```
var obj = {}; 
var proxy = new Proxy(obj, {
  get: function(target, property) {
    return 35;
  }
});
obj.name  //undefined
proxy.name // 35
proxy.title // 35
```

要使得Proxy起作用(handler中重载的方法)，必须针对Proxy实例（上例是proxy对象）进行操作，而不是针对目标对象（上例是空对象）进行操作。支持Proxy支持的拦截操作的属性ES6定义了15个（get set has等）

**Proxy实例**
*get*
```
var person = {
  name: "张三",
  get age () {
    return 18;
  }
};

以上代码等同于
"use strict";
var person = Object.defineProperties({
  name: "张三"
}, {
  age: {
    get: function get() {
      return 18;
    },
    configurable: true,
    enumerable: true
  }
});
// 对象每个属性都可以设置get，set。在赋值和取值是自动调用

var proxy = new Proxy(person, {
  get: function(target, property) {
    if (property in target) {
      return target[property];
    } else {
      throw new ReferenceError("Property \"" + property + "\" does not exist.");
    }
  }
});
// 重写了obj.__proto__.get
// 会代理所有属性的getter 

console.log(person.name) // "张三"
console.log(person.age)  // 18
console.log(proxy.name) // "张三"
console.log(proxy.age) // 18
console.log(proxy.address) // 抛出一个错误
```

*set*
set方法用来拦截某个属性的赋值操作。
```
let validator = {
  set: function(obj, prop, value) {
    if (prop === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError('The age is not an integer');
      }
      if (value > 200) {
        throw new RangeError('The age seems invalid');
      }
    }

    // 对于age以外的属性，直接保存
    obj[prop] = value;
  }
};
let person = new Proxy({}, validator);
person.age = 100;
person.age // 100
person.age = 'young' // 报错
person.age = 300 // 报错
```

*apply*
apply方法拦截函数的调用、call和apply操作。代理一个function
```
var twice = {
  apply (target, ctx, args) {
    return Reflect.apply(...arguments) * 2;
  }
};
function sum (left, right) {
  return left + right;
};
var proxy = new Proxy(sum, twice);
proxy(1, 2) // 6
proxy.call(null, 5, 6) // 22
proxy.apply(null, [7, 8]) // 30
```

*has*
has方法可以隐藏某些属性，不被in操作符发现。

*construct*
construct方法用于拦截new命令。

*deleteProperty*
deleteProperty方法用于拦截delete操作，如果这个方法抛出错误或者返回false，当前属性就无法被delete命令删除。

*defineProperty*
defineProperty方法拦截了Object.defineProperty操作。
```
var handler = {
  defineProperty (target, key, descriptor) {
    return false
  }
}
var target = {}
var proxy = new Proxy(target, handler)
proxy.foo = 'bar'
// TypeError: proxy defineProperty handler returned false for property '"foo"'
```

*enumerate*
enumerate方法用来拦截for...in循环。注意与Proxy对象的has方法区分，后者用来拦截in操作符，对for...in循环无效。
```
var handler = {
  enumerate (target) {
    return Object.keys(target).filter(key => key[0] !== '_')[Symbol.iterator]();
  }
}
var target = { prop: 'foo', _bar: 'baz', _prop: 'foo' }
var proxy = new Proxy(target, handler)
for (let key in proxy) {
  console.log(key);
}
// "prop"
```

### Reflect

* 将Object对象的一些明显属于语言层面的方法，放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。
* 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
* 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
* Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，*不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。*
```
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target,name, value, receiver);
    if (success) {
      log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```

### Set
*set*
ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。set内部判断两个值是否不同，使用的算法类似于精确相等运算符（===），这意味着，两个对象总是不相等的。唯一的例外是NaN等于自身（精确相等运算符认为NaN不等于自身）

Set结构的实例有以下属性
* Set.prototype.constructor：构造函数，默认就是Set函数。
* Set.prototype.size：返回Set实例的成员总数。

Set实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。
* add(value)：添加某个值，返回Set结构本身。
* delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
* has(value)：返回一个布尔值，表示该值是否为Set的成员。
* clear()：清除所有成员，没有返回值。

*WeakSet*
WeakSet结构与Set类似，也是不重复的值的集合。但是，它与Set有两个区别。
首先，WeakSet的成员只能是对象，而不能是其他类型的值。
其次，WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于WeakSet之中。这个特点意味着，无法引用WeakSet的成员，因此WeakSet是不可遍历的。WeakSet的一个用处，是储存DOM节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

### Map
JavaScript的对象（Object），本质上是键值对的集合（Hash结构），但是只能用字符串当作键。这给它的使用带来了很大的限制。
```
var data = {};
var element = document.getElementById("myDiv");

data[element] = metadata;  
// 等同于
data[element.toString()] = metadata;  
data["[Object HTMLDivElement]"] // metadata
```

ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现。只有对同一个对象的引用，Map结构才将其视为同一个键。
遍历map
```
let map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}

map.forEach(function(value, key, map)) {
  console.log("Key: %s, Value: %s", key, value);
};
```

*与其他数据结构的互相转换*
* Map转为数组
* 数组转为Map
* Map转为对象
* 对象转为Map
* Map转为JSON
* JSON转为Map

*WeakMap*
WeakMap结构与Map结构基本类似，唯一的区别是它只接受对象作为键名（null除外），不接受其他类型的值作为键名，而且键名所指向的对象，不计入垃圾回收机制。


### Iterator
* 四种数据集合 Array Object Set Map，
在ES6中，有三类数据结构原生具备Iterator接口：数组、某些类似数组的对象、Set和Map结构。
实现了Iterator接口，可以使用for..of..遍历
ES6规定，默认的Iterator接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable） 

```
var array = [
  1,
  2,
  3
];
var iterator = array[Symbol.iterator](); //返回Iterator类型的对象，有next方法
iterator.next(); // { value=1, done=false}
iterator.next(); // { value=2 done=false}
```

### Generator
*   基本概念
    Generator函数是一个状态机，封装了多个内部状态。执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。

*   yield
    Generator函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield语句就是暂停标志。
    遍历器对象的next方法的运行逻辑如下。
    1. 遇到yield语句，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。
    2. 下一次调用next方法时，再继续往下执行，直到遇到下一个yield语句。
    3. 如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。
    4. 如果该函数没有return语句，则返回的对象的value属性值为undefined。
*   next方法的参数
    yield句本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield语句的返回值。
    ```
    function* foo(x) {
      var y = 2 * (yield (x + 1));
      var z = yield (y / 3);
      return (x + y + z);
    }

    var it = foo(5);

    it.next()
    // { value:6, done:false }
    it.next(12)
    // { value:8, done:false }
    it.next(13)
    // { value:42, done:true }
    ```

*   for...of循环
    ```
    function *foo() {
      yield 1;
      yield 2;
      yield 3;
      yield 4;
      yield 5;
      return 6;
    }

    for (let v of foo()) {
      console.log(v);
    }
    // 1 2 3 4 5
    ```

    一旦next方法的返回对象的done属性为true，for...of循环就会中止，且不包含该返回对象，所以上面代码的return语句返回的6，不包括在for...of循环之中。

*   yield*语句
    在Generater函数内部，调用另一个Generator函数
    yield* 后面可以加Generator执行返回的遍历器对象，或者直接遍历器对象（Array）
    ```
    function* foo() {
      yield 'a';
      yield 'b';
    }

    function* bar() {
      yield 'x';
      foo();
      yield 'y';
    }
    for (let v of bar()){
      console.log(v);
    }
    // "x"
    // "a"
    // "b"
    // "y"
    ```

*   应用（Generator可以暂停函数执行，返回任意表达式的值）
    1. 异步操作的同步化表达
    ```
    function* main() {
      var result = yield request("http://some.url");
      var resp = JSON.parse(result);
        console.log(resp.value);
        }

    function request(url) {
      makeAjaxCall(url, function(response){
        it.next(response);
      });
    }

    var it = main();
    it.next();
    ```

    上面代码的main函数，就是通过Ajax操作获取数据。可以看到，除了多了一个yield，它几乎与同步操作的写法完全一样。注意，makeAjaxCall函数中的next方法，必须加上response参数，因为yield语句构成的表达式，本身是没有值的，总是等于undefined

### Promise
有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。
Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称Fulfilled）和Rejected（已失败）。
一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从Pending变为Resolved和从Pending变为Rejected。

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署
resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从Pending变为Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从Pending变为Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为Resolved时调用，第二个回调函数是Promise对象的状态变为Reject时调用

* Promise对象实现的Ajax操作的例子
```
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.status === 200) {
        *resolve(this.response);*
      } else {
        *reject(new Error(this.statusText));*
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

resolve函数的参数除了正常的值以外，还可能是另一个Promise实例。
```
p1和p2都是Promise的实例，但是p2的resolve方法将p1作为参数，即一个异步操作的结果是返回另一个异步操作。
注意，这时p1的状态就会传递给p2，也就是说，p1的状态决定了p2的状态。如果p1的状态是Pending，那么p2的回调函数就会等待p1的状态改变

var p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})
var p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)    
  // p2的状态由p1决定，虽然p2中调用了resolve，p1变为rejected，p2也跟着变为rejected
})
p1.then(result => console.log('11'), result => console.log('33'))
p1.catch(error => console.log(error))
p2.then(result => console.log('22'), result => console.log('44'))
p2.catch(error => console.log(error))
// 33
// fail
// 44
// fail
p1是一个Promise，3秒之后变为rejected。p2的状态由p1决定，1秒之后，p2调用resolve方法，但是此时p1的状态还没有改变，因此p2的状态也不会变。又过了2秒，p1变为rejected，p2也跟着变为rejected。
```

* Promise.prototype.then()
then方法返回的是一个新的Promise实例.
前一个promise的状态觉得后一个promise的状态，链式调用的时候，前一个回调函数返回的结构作为参数，传入后面那个回调函数。
```
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("Resolved: ", comments),
  err => console.log("Rejected: ", err)
);
```

* Promise.prototype.catch()
Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
```
p.then((val) => console.log("fulfilled:", val))
  .catch((err) => console.log("rejected:", err));

// 等同于

p.then((val) => console.log(fulfilled:", val))
  .then(null, (err) => console.log("rejected:", err));
```

* Promise.all()
Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。
```
var p = Promise.all([p1,p2,p3]);
```
上面代码中，Promise.all方法接受一个数组作为参数，p1、p2、p3都是Promise对象的实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为Promise实例，再进一步处理。（Promise.all方法的参数不一定是数组，但是必须具有iterator接口，且返回的每个成员都是Promise实例。）

p的状态由p1、p2、p3决定，分成两种情况。
1. 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
2. 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。


* Promise.race()

Promise.race方法同样是将多个Promise实例，包装成一个新的Promise实例。
```
var p = Promise.race([p1,p2,p3]);
```

上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数。
```
var p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
])
p.then(response => console.log(response))
p.catch(error => console.log(error))
```

上面代码中，如果5秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数。

* Promise.resolve()
```
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

Promise实例的状态从一生成就是Resolved，所以回调函数会立即执行

* 应用
加载图片
我们可以将图片的加载写成一个Promise，一旦加载完成，Promise的状态就发生变化。
```
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    var image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```


### 异步操作和Async函数
ES6诞生以前，异步编程的方法，大概有下面四种。
    1. 回调函数 (settimeout,  requestanimationframe, readFile)
    2. 事件监听 (ajax, on-click, on-load) 
    3. 发布/订阅
    4. Promise 对象

### Class
*   基本词法
    ES5的构造函数Point，对应ES6的Point类的构造方法
    ```
    class Point{
      // ...
    }
    typeof Point // "function"

    class Point {
      constructor(){
        // ...
      }

      toString(){
        // ...
      }

      toValue(){
        // ...
      }
    }

    // 等同于

    Point.prototype = {
      constructor(){},
      toString(){},
      toValue(){}
    }

    Point.name // "Point"
    ```

    prototype对象的constructor属性，直接指向“类”的本身，和ES5的概念是一样的。
    ```
    Point.prototype.constructor === Point // true
    ```

    ES6中，类的内部所有定义的方法，都是不可枚举的（enumerable）。和ES5不一致
    ```
    class Point {
      constructor(x, y) {
        // ...
      }

      toString() {
        // ...
      }
    }

    Object.keys(Point.prototype)
    // []
    Object.getOwnPropertyNames(Point.prototype)
    // ["constructor","toString"]

    var Point = function (x, y){
      // ...
    }

    Point.prototype.toString = function() {
      // ...
    }

    Object.keys(Point.prototype)
    // ["toString"]
    Object.getOwnPropertyNames(Point.prototype)
    // ["constructor","toString"]
    ```

*   继承
    ```
    class ColorPoint extends Point {
      constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
      }

      toString() {
        return this.color + ' ' + super.toString(); // 调用父类的toString()
      }
    }
    ```
    
    类的prototype属性和__proto__属性
    ```
    class A {
    }

    class B extends A {
    }

    B.__proto__ === A // true
    B.prototype.__proto__ === A.prototype // true
    ```

    Object.getPrototypeOf方法可以用来从子类上获取父类,判断，一个类是否继承了另一个类。
    ```
    Object.getPrototypeOf(ColorPoint) === Point
    // true
    ```

*   Mixin模式的实现
    Mixin模式指的是，将多个类的接口“混入”（mix in）另一个类
    ```
    class DistributedEdit extends mix(Loggable, Serializable) {
      // ...
    }
    ```