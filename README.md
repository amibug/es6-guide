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
1.  const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变，对常量重新赋值不会报错，只会默默地失败


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
