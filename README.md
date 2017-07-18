## ES6核心内容整理 ##


### 一、新的变量声明方式let和const ###

> 新的变量声明方式，提供变量的块级作用域，同时通过一些限制来防止我们范错误。也就是说更好的声明变量的方式。

1）let/const与var的区别是提供了块级作用域与不再具备变量提升。

2）在同一个作用域内let/const禁止重复声明相同的变量
```javascript
var a=1;
let a=2; // SyntaxError
```
3) let声明的变量可重赋值，const声明的变量不能重复赋值，即常量。
4）在当前作用域，使用的变量已经存在，但是在代码执行到变量声明前禁止访问。
```javascript
var tmp = 123
if (true) {
  tmp = 'abc' // ReferenceError
  let tmp
}
```
**常见使用场景**

1）因为能创建块级作用域，所以常见于if和for中
```javascript
var arr = [];
for (let i = 0; i < 10; i ++){
  arr[i] = function (){
    return i
  }
}
arr[6]()  // => 6

//如果用var声明i，无论多少次迭代，外层的i始终被每次迭代的函数内部引用着(闭包)，
//不会被当做垃圾回收，最后的结果都指向同一个i，值为10。
//以往为了避免这个问题，通常会这么做：    
for (var i = 0; i < 10; i ++){
  arr[i] = (function (i){
    return function (){
      return i
    }
  })(i)
}
```
2）const在实践中常用来声明一个对象，之后可以再对这个对象的属性进行修改
```javascript
const foo = {
  name: 'bar'
}
foo.name = 'baz'
console.log(foo)
```
## 二、解构 ##
>ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。也就是说通过模式匹配来进行变量赋值。

1)数组基本用法
```javascript
let [a,b,c]=[1,2,3];
a //1
b //2
c //3
```
2)对象基本用法
```javascript
let {foo,bar} = {foo:"aaa",bar:"bbb"}
foo // "aaa"
bar // "bbb"
```
**常见使用场景**

1） 交换变量的值
```javascript
[x,y] = [y,x]
```
2)提取JSON数据
```javascript
let jsonData = {
  id:42,
  status:"OK",
  data:[1,2]
}
let {id,status,data:number} = jsonData
console.log(id,status,number) // 42,"ok",[1,2]
```
## 三、class类 ##

> 在没有class的时候，创建类的一种比较标准的方式是将非函数的属性放到构造函数里，函数属性在原型链里添加。类的继承的实现就更为多样：对象冒充、call/apply方式、原型链方式等。es6的class和extends关键字的出现给出了一个统一的规范
```javascript
class People{
  constructor(name,age,gender){
    this.name = name;
  }
  sayName(){
    return this.name;
  }
}

class Student extends People{
  constructor(name,age,gender,skill){
    super(name,age,gender)
    this.skill = skill;	
  }
  saySkill(){
    return this.skill;
  }
}

let tom = new Student('tom',16,'male','computer');
tom.sayName() // tom
tom.saySkill() // computer
tom.__proto__ == Student.prototype // true
Student.__proto__ == People // true
```
## 四、箭头函数 ##

> 箭头函数可以用来替换函数表达式，不用写function，更加简化。也就是说是函数表达式的简化方式。
> es6之前的function有一个特点：函数内部的上下文并不是由该函数写在那里决定的，而是由谁调用决定的，谁调用函数内部的this就指向谁。然后我们有些时候并不想让他这样，但又没办法，只能通过先保存this，或者call/apply，或者bind来调整上下文。箭头函数的出现解决了这个宁人苦恼的问题，因为箭头函数内的上线文(this)是由函数写在哪决定的，无论被哪个对象调用，上下文都不会改变。

1）箭头函数中的this指向外层的this
```javascript
var obj = {
  test1 : function (){
    window.setTimeout(function (){
      console.info(this)
    }, 100)
  },
  test2 : function (){
    window.setTimeout(() => {
      console.info(this)
    }, 100)
  }
}

obj.test1() // => Window
obj.test2() // => obj
```
2）无法用call/apply/bind来改变指向

3）在ES6中，会默认采用**严格模式**，因此默认情况下this不是指向window对象，而是undefined。

```javascript
setTimeout(() =console.log(this),1000)//undefined,不是window
```
4）不可以使用arguments对象，该对象在函数体内不存在，如果要用，可以用rest参数代替。

5）箭头函数还有一个特点就是能够简化return的书写。

```javascript
var a = function (n){
    return n;
}
var b = (n) => n //可以省略return和花括号
a(1)  // => 1
b(1)  // => 1
```
    

