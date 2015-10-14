es6-guide
=========


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
1.  模式匹配的写法，左边的变量会被赋予对应的值，解构赋值适用于var let const命令,ES6的规则是，只要有可能导致解构的歧义，就不得使用圆括号。
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
    * stratsWith
    * endsWith
    * includes
    * repeat
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



