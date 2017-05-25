## ES6核心内容整理 ##


### 一、新的变量声明方式let和const ###

> 新的变量声明方式，提供变量的块级作用域，同时通过一些限制来防止我们范错误。也就是说更好的声明变量的方式。

1）let/const与var的区别是提供了块级作用域与不再具备变量提升。

2）在同一个作用域内let/const禁止重复声明相同的变量

    var a=1;
    let a=2; // SyntaxError
3) let声明的变量可重赋值，const声明的变量不能重复赋值，即常量。
4）在当前作用域，使用的变量已经存在，但是在代码执行到变量声明前禁止访问。

    var tmp = 123
    if (true) {
      tmp = 'abc' // ReferenceError
      let tmp
    }
**常见使用场景**

1）因为能创建块级作用域，所以常见于if和for中

    for (let i = 0; i < arr.length; i++) {
        console.log(arr[i])
    }

2）const在实践中常用来声明一个对象，之后可以再对这个对象的属性进行修改

    const foo = {
        name: 'bar'
    }
    foo.name = 'baz'    console.log(foo)​
