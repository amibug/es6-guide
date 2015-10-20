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
    const key2 = Symbol();
    let obj = {
      [key1]() {},
      [key2]() {},
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

