## 作用域(变量和函数的可访问范围)

```js
1.全局作用域

可见性：全局作用域中声明的变量，在代码的任何地方都可以访问

生命周期：伴随着页面的生命周期(页面关闭，销毁)


//函数内部，如果不使用任何关键字声明变量，那么这个变量会变成window的属性
```

```js
2.局部作用域（函数作用域）

在函数内部声明的变量只能在函数内部访问，外部无法访问

可见性：函数外部不能访问函数内部的变量

生命周期：变量在函数调用执行后，就会销毁（清空/回收）
```

``` js
  3.块级作用域
  为什么需要块级作用域？？？
  var temp = new Date()
  function fn() {

   console.log(temp)

   if (false) {  // if是语句

​    var temp = 'hello world'

   }

  }

  fn()
//1. var声明的变量会提升到当前作用域的最前面，只是变量提升。值不会提升
//2. if是语句，不是函数，var声明变量不能形成作用域
//3. var声明的变量，只声明，不赋值，默认是undefined


//var声明的变量，在for循环中，提升到全局
  for (var i = 0; i <= 3; i++) {
      console.log(i)
    }
    // 最后循环完，i++， i变为4 
    console.log(i)


//if/for 不是函数，它们是语句，用var声明的变量，不能形成作用域

//var变量
//1. 内层变量可能覆盖外层变量
//2. 用来计数的循环变量i，会泄漏为全局变量


块级作用域
//{}包裹起来的叫代码块，使用let或者const声明的变量，在{}中会产生块级作用域
   for(let i = 0; i < 3; i++){
            console.log(i)
        }

   for(let i = 0; i < 3; i++){
            console.log(i)
        }

   if(false){
            let temp = 'hello world'
        }
//块级作用域的特点：
//1. 只有let、const会产生块级作用域{}
//2. 块级作用域的外部不能访问内部的变量
//3. 两个块级作用域中的变量相互不影响
//4. if/for语句，但是用let/const声明的变量，会在内部形成块级作用域
```

## var-let-const

```js
***面试题***
//var let const 的区别
    //1. 只有let/const 可以形成块级作用域
    //2. let/const 不存在变量提升，只有var有
    //3. let/const不允许重复声明，只有var可以
    //4. let/const存在暂时性死区（temporal dead zone），不能再声明之前使用
    //5. 浏览器中，var声明的全局变量会挂载到window对象上，let/const不会
    
//const关键字
    //1.const一旦声明，必须马上赋值
    //2. const声明的变量不能改变值（简单类型不能改变值，引用类型不能改变地址）
    
    // ==> 3. 解释
        // function count(){
        //     let a = 1 
        //     var a = 2
        // }
        // function count2(){
        //     let a = 1 
        //     let a = 2
        // }

        // ==> 4. 解释
        // if(true){
        //     temp = 'hello world '
        //     let temp;
        // }

        // ==> 5.解释
        var abc = 666
        console.log(window.abc)
        // ==> 不建议使用var 声明name这个名字， window本身有name这个属性
        var name = 123
        console.log(window)

        //==> const
        // let p; var c;
        // const name = '马上赋值'
        // const test; // Error
    
    
```



## 作用域链

```js
//函数是可以嵌套函数的，每个函数都有一个局部作用域，这样，也会形成作用域的嵌套
//当访问内层作用域中的某个变量的时候，首先在当前作用域中查找这个变量，如果不存在，往上层查找，直到全局作用域

//---由于变量的查找是沿着作用域链来实现的，也被称为作用域为变量的查找机制

//作用域链的本质：底层变量的查找机制

//查找规则：
//1. 优先在当前作用域中查找变量
//2. 如果不存在，往上层作用域中查找变量，直到全局作用域
```



## 垃圾回收机制(面试题)

```js
//垃圾回收机制（garbage collection）简称GC，JS中内存的分配和回收时自动完成的，内存在不适用的时候会被JS引擎/垃圾回收程序自动回收

//什么是内存泄漏？
//内存泄漏：不再使用的内存，没有及时释放掉
```



## 引用计数法(面试题)

```js
// 垃圾回收策略
        // 1. 引用计数法 （淘汰）
        // 2. 标记清除法 （主流）

        // ==> 引用计数法
        // 1. 跟踪记录每个值被引用的次数
        // 2. 如果这个值被引用了一次，那么就记录次数1 
        // 3. 多次引用会累加
        // 4. 如果减少一个引用就减少1 
        // 5. 如果引用次数是0，则释放内存

        // ==> 引用计数法并不是周期性的垃圾回收，什么时候引用次数为0了，直接回收

        function problem () {
            // oA = new Object()
            let oA = {}
            let oB = {}
            oA.c = oB  //  oB被oA引用
            oB.d = oA  //  oA又被oB引用

        }
        problem()

        //==> 循环引用的时候，堆内存空间中创建的对象相互引用，计算永远不会为0，
        // 这个对象会一直占用着内存空间，造成内存泄漏
```



## 标记清除法(面试题)

```js
//标记清除算法(Mark-Sweep)
//主要将GC的过程分为两个阶段
//1. 标记阶段：标记空间中的活动对象和非活动对象
//2. 清除阶段：回收非活动对象，也就是销毁非活动对象

//标记阶段从一组根元素开始
//递归遍历（一层一层）这组根元素
//在这个遍历过程中，能访问到的元素称为活动对象，不能访问到的可以判断为垃圾数据

//缺陷：内存碎片化

//==>标记整理算法（Mark-compact）
// 1. 标记空间中活动对象和非活动对象
// 2. 回收非活动对象所占用的内存
// 3. 内存整理
```



## 闭包(面试题)

```js
//什么是闭包（closeure）？
//定义：内层函数引用外层函数的变量的集合
//闭包 = 内层函数 + 外层函数的变量

//条件：
//1. 首先要有内层函数
//2. 内层函数使用了外层函数的变量

//(外层函数就相当于把这个整体包裹起来了，所以叫闭包)

//简单的写法：
function outer(){
    const a = 10
    function fn(){
        console.log(a)
    }
    fn()
}
outer()

//1.Local 局部作用域		2. Global 全局作用域		3.Closure 闭包		4.Block	块级作用域


//闭包的作用：让外部可以访问函数内部的变量
function outer(){
    let a = 10
function fn(){
    console.log(a)
}
return fn
}

//简写 outer() === fn ==> function fn(){console.log(a)}

const fun = outer()
//const fun = function fn(){
//console.log(a)
//}
fun()
// ==> 我们在外部调用fun的时候，相当于执行上面注释的函数，执行到log（a），找的是outer函数内部的变量a

 //2.闭包的简写
    function outer() {
      let a = 10
      return function () {
        console.log(a)
      }
    }
    const fn = outer()
    // const fn = function(){
    // console.log(a)
    // }
    fn() // 调用函数

//闭包的应用
//1. 统计一个函数调用的次数
// let i = 0 //全局变量
//function fn(){
// i++
//  当let i = 0 // 局部变量，调用完，i销毁
//  console.log(`调用了${i}次`)
//}
//fn()

//2. 实现数据的私有化(我们在外面是没法修改这个i变量的)
//但是我们可以在外层访问

function count(){
    let i = 0
    return function(){
        i++
        
        console.log(`调用了${i}`)
    }
}
const fn = count()
==>
//const fn = function(){
//	i++
// console.log(`调用了${i}次`)
//      }
      fn()





//闭包会产生的问题：内存泄漏

//标记清除法：从一组根元素开始，或者说从全局作用域出发，找到了这个i变量，能访问到这个i变量，把它标记为活动对象，所以不会GC程序清理掉

//本来，我们调用fn()后，按理说， i是函数count里面的变量，局部变量执行完后应该销毁，但是现在，因为i变量和内部的函数function形成了闭包。i变量没有被销毁，所以，造成了内存泄漏

***面试题***
      闭包的特效(作用)
//通过闭包，我们可以让外部访问到函数内部的变量，同时，实现数据的私有化
//闭包产生的问题：内存泄漏

//闭包的实际应用
//节流 throttle 减少事件执行的频率
//防抖 debounce 先抖动着，什么时候停了，再执行事件
```



## 变量的提升

```js
// 1.所有var声明的变量会执行之前，会提升到当前作用域的最前面
// 2.var声明的变量，只提升声明，不提升赋值
// 3.var声明的变量，如果不赋值，默认undefined
```



## 函数的提升

```js
//声明式
fn()
function fn(){
    console.log(666)
}

//表达式
const foo = function(){
    console.log(888)
}
var bar = function(){
    console.log(999)
}

//1. 会把用function声明的函数，提升到当前作用域的最前面
//2. 表达式的情况，如果是用var声明的函数，只提升声明，不提升赋值

//一般：先声明，后调用
```



## arguments对象

```js
//1. 除箭头函数以外，所有的函数都内置了(默认就存在，自带的)一个arguments ==> 接受了 / 存储了我们传递过来的实参

//2. arguments 伪 / 类数组 有length，有索引，没有push，pop等数组方法

//3. arguments对象，只存在于函数中

function getSun(){
    for(let i = 0;i<arguments.length;i++){
        sum += arguments[i]
    }
    return sum
}
//const res = getSum(2,3)
const res = getSum(2,3,5,6,8)
console.log(res)


//箭头函数没有arguments
const fn = () => {
    console.log(arguments)
}
fn()//error

```



## rest剩余参数

```js
//剩余参数 rest
//形式 ...变量名 变量名自定义

//1.剩余参数rest是一个真数组，存了剩余的实参。
//2.只能放到参数的最后一位
function getSum(...arr){
    console.log(arr) //使用的时候不需要写...
}
function getSum(a,b,...arr){
    console.log(arr)
}
//getSum(1,2,3,4,5)

//求所有传入参数的和
//箭头函数没有arguments对象
const getSum = (...args) => {
    let sum = 0
    args.forEach(el => sum += el)
    return sum
}
const res = getSum(1,2,3)
console.log(res)
```



## 拓展运算符

```js
//spread  扩展运算符
//扩展运算符(spread)是三个点(...)
//它好比 rest 参数的逆运算
//作用： ==> 将一个数组转为用逗号分隔的参数序列
//注意: 它不会改变原数组

const arr = [1,2,3,4,5]
console.log(...arr)//1,2,3,4,5 浏览器上没有显示逗号，假装有

//主要应用
//1. 求一个数组的最大最小值
const arr2 = [1,5,6,8,3]
// Math.max()  它本身接收的是参数列表
    // console.log(Math.max(1,5,6,8,3))
    console.log(Math.max(...aarr2))
    console.log(Math.min(...aarr2))

    // 2. 复制/拷贝数组（浅拷贝）
    const a1 = [1, 2]
    const a2 = a1
    //赋值，数组的直接赋值，赋的是地址，指向的是堆内存中同一个数组对象
    a2[0] = 666
    console.log(a1)

    const a3 = [5, 6, 7]
    const a4 = [...a3] // ==> [5,6,7]
    a3[0] = 9
    console.log(a4) //  [5,6,7]

    // 3. 合并数组  (浅拷贝)
    const a5 = ['c', 'd']
    const a6 = ['e', 'f']

    // ES5 的合并 concat
    const resArr = a5.concat(a6)
    console.log(resArr)
    // ES6 ...
    const a7 = [...a5, ...a6]
    console.log(a7)

    // 4. 将字符串转为数组
    //扩展运算符还可以展开字符串
    //[...str] ==> 将字符串展开为数组
    const str = 'hello world'
    const a8 = [...str]
    console.log(a8)
    // 反转字符串
    const res = str.split('').reverse().join('')

    // ==> 扩展运算符的方式
    const resStr = [...str].reverse().join('')
    console.log(resStr)


    
    // 5. 将伪数组转为真数组
    const divs = document.querySelectorAll('div')
        // console.log(divs)
        // divs.push(666)
        const resDiv = [...divs]
        console.log(resDiv)
        resDiv.push(666)
        console.log(resDiv)

/* ==================== 将伪数组转为真数组 ===================== */
        // 1. [...likeArr] 使用扩展运算符
        // 2. Array.from(likeArr)
        const resReal = Array.from(divs)
        console.log(resReal)
```



## 语法糖

```js
// const obj = {} // const obj = new Object()
  // const arr = [] // const arr = new Array()

  //! 1. 所有的引用类型（对象 数组 函数），都具有对象的特性，可以自由扩展属性
  // const obj = {} obj.a = 100
  // const arr = [] [].a = 200
  // function fn(){} fn.a = 100

  //! 2. 所有的对象，都有一个__proto__属性，属性值是一个普通的对象 [[prototype]] / __proto__ 也叫隐式原型
  console.log(obj.__proto__)
  console.log(arr.__proto__)
  console.log(fn.__proto__)

  //! 3. 所有的函数，都有一个prototype属性，属性值也是一个普通的对象。 (这个属性值是一个指针，指向原型对象) prototype也叫做显式原型
  console.log(fn.prototype)

  //! 4. 所有对象的隐式原型__proto__，指向它构造函数的prototype显式原型
  console.log(obj.__proto__ === Object.prototype)
  console.log(arr.__proto__ === Array.prototype)
  console.log(fn.__proto__ === Function.prototype)

  //! 5. 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的__proto__中寻找

  // 原型对象

  function Person(name,age){
  this.name = name
  this.age = age
  }

  const ldh = new Person('刘德华', 20)
  console.log(ldh.__proto__ === Person.prototype)
```



## 箭头函数

```js
// 1. 箭头函数 基本语法
const fn1 = () => {
  console.log(123)
}
fn1()
// 传参
const fn2 = (x) => {
  console.log(x)
}
fn2(1)
// 2. 只有一个形参的时候，可以省略小括号
const fn3 = x => {
  console.log(x)
}
fn3(1)
// 3. 只有一行代码的时候，我们可以省略大括号
const fn4 = x => console.log(x)
fn4(6)
// 4. 只有一行代码的时候，可以省略return
const fn5 = x => x + x
console.log(fn5(1))
// 5. 箭头函数可以直接返回一个对象, 但必须在对象外面加上括号
const fn6 = (name) => ({ user_name: name })
console.log(fn6('刘德华'))


// ==> 1. this指向在定义的时候不能确定，只有在调用执行的时候才能确定！！！
    // ==> 2. 箭头函数本身没有this，它的this是找上层作用域this，定义的时候已经确定了。
    // 以前this的指向：  谁调用的这个函数，this 就指向谁
    // console.log(this)  // window
    // 普通函数
    // function fn() {
    //   console.log(this) // window
    // }
    // window.fn()
    // // 对象方法里面的this
    // const obj = {
    //   name: 'andy',
    //   sayHi: function () {
    //     console.log(this) // obj
    //   }
    // }
    // obj.sayHi()

    // // 2. 箭头函数的this  是上一层作用域的this 指向
    // const fn = () => {
    //   console.log(this) // window
    // }
    // fn()
    // // 对象方法箭头函数 this
    // const obj = {
    //   uname: 'pink老师',
    //   sayHi: () => {
    //     console.log(this) // this 指向谁？
    //   }
    // }
```



## 数组的解构

```js
//解构赋值
	// ES6 允许按照一定的模式， 从数组和对象中提取值，对变量进行赋值。这被称为解构赋值
        
        //ES5 为变量赋值，只能直接指定值
        // let a = 1 
        // let b = 2 
        // let c = 3

        // ES6 解构赋值
        let [a, b, c] =  [1, 2, 3]
        console.log(a,b,c)
        // 从数组中提取值，按照对应的位置，对变量赋值。 注意：数组的解构有顺序。

        const [max, min, avg] = [100, 60, 80]

        ;[d, f] = [11, 22]// 加分号！！！
        // 如果不加let const关键字, 相当于是var声明的，会成为window的属性
        console.log(d, f, window.d)

        // 2. 数组的解构赋值，可以用于交换两个变量

        // ES5 引入一个temp变量
        let g = 1 
        let h = 2 
        // let temp 
        // temp = g 
        // g = h 
        // h = temp
        // console.log(g, h)

        // ES6 交换
        ;[h, g] = [g, h]  // 注意分号
        console.log(g, h)


        // JS中需要加分号的两种情况
        // ()  立即执行函数    
        // []  数组

        ;[1,2,3].forEach(el => {
            console.log(el)
        })


//数组解构的细节
	// 1. 左边变量多，数组元素少
    const [a, b, c, d] = [1, 2, 3]
    console.log(a, b, c, d) // d ==> undefined

    // 2. 变量少，数组元素多
    const [e, f] = [1, 2, 3]
    console.log(e, f)

    // 3. 变量少，数组元素多，可以利用剩余参数接收多余的数组元素
    const [g, h, ...i] = [1, 2, 3, 4, 5, 6]
    console.log(g, h, i)

    // 4. 数组的解构，按着对应的位置赋值
    const [j, k, , l] = [1, 2, 3, 4]
    console.log(j, k, l)

    // 5. 解构赋值，还可以设置默认值
    cosnt[m = 0, n = 1] = [66, 77]
    console.log(m, n)

    //默认值什么时候生效
    const [o = 0, p = 0] = [66, 77]
    console.log(o, p)
    //eg.2 严格等于undefined的时候，默认值生效
    cosnt[aa = 0, bb = 1] = ['undefined', undefined]
    console.log(aa, bb) //undefined，1

    // 6. 如果等号右边不是数组，那么会报错
    let [f1] = 1
    let [f2] = false
    let [f3] = NaN
    let [f4] = undefined
    let [f5] = null
    let [f6] = {}
    
    
 //多维数组
     //多维数组：数组嵌套数组
    const arr = [1,2,[3,4]] //二维数组
    console.log(arr[0])
    console.log(arr[1])
    console.log(arr[2])
    console.log(arr[3]) //[3,4]
    
    console.log(arr[2][0])
    console.log(arr[2][1])

    //多维数组的解构
    //const [a,b,c] = arr
    // console.log(a,b,c)
    // 数组的解构是一种模式匹配，左侧和右侧的模式一样
    const [a,b,[c,d]] = arr //[1,2,[3,4]]
    console.log(a,b,c,d)


//数组解构小结
	// 本质上，解构的语法属于模式匹配，只要等号两边的模式相同（长得一样）
        // 就可以把右边的值，赋值给左边对应的变量。

        let [a, [[b], c]] = [1, [[2], 3]]
        console.log(a, b, c)

        let [, , d] = ['1', '2', '3']
        console.log(d)

        let [x, , y] = [1, 2, 3]

        let [head, ...tail] = [1, 2, 3, 4]
        console.log(head, tail)

        let [aa, bb, ...cc] = ['a']
        console.log(aa, bb, cc) // 'a' , undefined, []

        // 如果解构不成功吗，变量的值就是undefined
        let [foo] = []
        let [f1, f2] = [1]
        console.log(foo) // undefined
        console.log(f1, f2) // 1, undefined
```



## 对象解构

```js
const obj = {
      name: '霖霖',
      age: 18
    }
    // 对象的解构 ==> {}

    // 1. 对象的解构，是按着属性名解构，可以交换位置
    //const {name，age} = obj
    // console.log(name,age)

    // 2. 要求变量名必须和属性名一致才可以解构出对象中的属性值
    // const{username,age} = obj
    // console.log(username,age) //undefined 18

    //3. 解构某一个属性名
    const {
      age
    } = obj
    console.log(age)


//对象解构重命名
// 对象解构的变量名 可以重新命名 
        // 旧变量名: 新变量名
        const obj = {
            name:'俊俊',
            age: 18
        }
        
        const {name:user_name, age} = obj
        console.log(name, user_name, age)

        const obj2 = {first:'hello', last:'world'}
        //Q fisrt last解构， 重名为 f , l 
        const{first:f,last:l} = obj2

        //结论
        //对象解构的内部机制
        //1. 先找到同名的属性，然后再赋值给对应的变量
        //2. 如果有冒号重命名，被赋值的是冒号后面的那个新变量名
        
// 多级对象的解构
        // 多级对象的解构
    const pig = {
      name: '佩奇',
      family: {
        mother: '猪妈妈',
        father: '猪爸爸',
        sister: '乔治',
        demo: {
          test: '123'
        }
      },
      age: 6
    }

    // const {name, family} = pig
    // console.log(name, family)
    const {
      name,
      family: {
        mother,
        father,
        sister,
        demo: {
          test
        }
      }
    } = pig

    console.log(name, mother, father, sister, test)

    // 每当有一个冒号出现，冒号的左边就是查找对应位置，冒号的右边就是赋值的目标


// 函数参数的解构
 // 1. 函数参数的解构
    // const pos = {x:10, y:20}
    // function move(pos){
    //     console.log(pos)
    //     const {x, y} = pos

    // }
    // move(pos)

    // 2. 可以在形参的位置，直接解构
    // 如果放在形参上解构，相当于是let声明的变量， 可以修改
    // const pos = {x:10, y:20}
    // function move({x, y}){
    //     // console.log(pos)
    //     // const {x, y} = pos
    //     x = 666
    //     console.log(x, y)


    // }
    // move(pos)

    // 3. 可以在形参的位置，直接解构，并且，可以重命名
    // 如果放在形参上解构，相当于是let声明的变量， 可以修改
    const pos = {
      x: 10,
      y: 20
    }

    function move({
      x: aa,
      y
    }) {
      // console.log(pos)
      // const {x, y} = pos
      console.log(aa, y)


    }
    move(pos)

    // 4. 如果函数的参数是数组的情况
    function add([x, y]) {
      return x + y
    }
    const res = add([1, 2])
    console.log(res)

//array.filter()
// array.filter()      filter筛选，过滤的意思
    // 作用：创建一个新的数组，新数组的元素是通过筛选后的元素 （满足筛选的条件）

    // 参数
    // array.filter 接收一个函数， 函数接收两个参数
    // ==>  第一个参数    正在处理的元素
    // ==>  第二个参数    index 索引号  （可选）

    // 返回值： 由满足条件的元素，组成的新数组

    //注意： 这个函数里面一般要写return，看是否满足条件 返回true/false
    // ==> true 表示满足筛选条件，把这个元素放到新数组中

    const arr = [10, 20, 30, 40]
    // >= 20
    const res = arr.filter(function (item) {
      return item >= 20
    })
    console.log(res)
    const resl = arr.filter(el => el >= 30)
    console.log(resl)
    console.log(arr)



    // 1. filter过滤  得到一个新的数组，元素的个数一般会减少
    const resFilter = arr.filter(el => el > 30)
    console.log(resFilter)
    // 2. map映射 一对一  [1, 2, 3] ===> [a, b, c]
    const resMap = arr.map(el => el + 6)
    console.log(resMap)

    //map和filter都返回了一个新数组，forEach不会
```

