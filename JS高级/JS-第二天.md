## 创建对象-字面量

```js
 // 对象：是属性和方法的集合（集合本身是无序的）
        // 属性： 事物的特征 ， 名词
        // 方法： 事物的行为 ， 动词

        // 1. 利用字面量来创建一个对象 {}   字面量 ==> 看见就知道是什么 {} [] ''
        const obj = {}  // 创建了一个空对象

        const person = {
            name:'张三丰',
            age:128,
            sayHi:function(){
                console.log('Hi~~~')
            }
        }

        // 2. 获取对象的属性值 两种方式
        // 2.1  对象.属性 
        console.log(person.name)
        // 2.2  对象[属性]
        console.log(person['age'])

        // 3. 调用对象的方法 
        // 对象.方法()
        person.sayHi()

        // person['sayHi']()  ==> 不推荐
```



## 利用new Object()

```js
// 利用new Object()
    const obj = new Object()  // 创建了一个空对象
        console.log(obj)
        obj.name = '俊俊'
        obj.age = 3
        obj.dance = function(){
            console.log('会社会摇')
        }
        console.log(obj)

        const p = new Object({name:'霖霖', age:18})
        console.log(p)

        // const obj = {}  // ==> const obj = new Object()
        // const arr = []  // ==> const arr = new Array()
```



## 利用构造函数创建对象

```js
// function 构造函数名(形参1,形参2,形参3) { // 构造函数的形参与对象的普通属性是一般一致的
    //         this.属性名1 = 形参1; 
    //         this.属性名2 = 形参2; // 属性的值，一般是通过同名的形参来赋值的
    //         this.属性名3 = 形参3;
    //         this.方法名 = function(){
                
    //         };
    //     }
    //     // 调用
    //     const obj = new 构造函数名(实参1，实参2，实参3)

    //     // 简记
    //     function 构造函数名() {
    //         this.属性 = 值;  // 当前的这个对象的
    //         this.方法 = function() {}
    //     }
    //     new 构造函数名();
function Person(name,age,sex){
  this.name = name
  this.age = age
  this.sex = sex
  //如果对象的方法想传参，这里写
  this.sayHi = function(msg){
    console.log(msg)
  }
  // return {} 这里省略了return
}
// 调用 ==> new
const p = new Person('杰伦',28,'男')
console.log(p)
console.log(p.name)
p.sayHi('hello')

//1. 构造函数，相当于一个模板，可以创建一系列具有相同属性和方法的对象
//2. 通过构造函数创建对象的过程，就叫做实例化  ==> new 出一个对象的过程
//3. 创建好的这个对象 ==> 实例 / 实例对象

// 注意: 
        // 1. 构造函数名字首字母要大写
        // 2. 构造函数里 属性和方法前面必须添加 this
        // 2. 构造函数不需要 return 就可以返回结果
        // 3. 我们调用构造函数 必须使用 new
```



## 构造函数的执行过程

```js
function Person(name,age,sex){
      //1. 创建了一个空对象
      // const this = {}
      //2. 让this指向这个空对象
      //3. 执行构造函数里面的代码，给这个空对象添加属性和方法
      this.name = name
      this.age = age
      this.sex = sex
      this.sayHi = function(){
        console.log('Nice to meet U')
             
      }
      //4. return this 返回这个对象(this) 
    }
    //当我们执行new的时候，会去执行构造函数内部的代码
    const p = new Person('俊俊',18,'female')

    // new的执行过程 ==> 背下来
    //1. 在函数内部创建了一个空对象
    //2. 让this指向这个空对象
    //3. 执行构造函数里面的代码，给这个空对象添加属性和方法
    //4. 返回这个对象(this)
```



## 实例成员-静态成员

```js
function Person(name,age,sex){
      this.name = name
      this.age = age
      this.sex =sex
      this.sayHi = function(){
        console.log('Nice to Meet U')
      }
    }
    //实例化：通过构造函数创建出来的对象，就叫做实例
    const p = new Person('龙龙',20,'male')
    //p实例
    console.log(p)
    //实例成员：实例对象的属性和方法
    //实例属性
    console.log(p.name)
    //实例方法
    p.sayHi()
    //实例成员 ==> 通过实例对象去访问
    //也就是构造函数里面通过this添加的属性和方法
```



## 基本包装类型(了解)

```js
 // 基本数据  Number String Boolean undefined null Symbol BigInt
        // 复杂类型  Object 
                    // Function Array 
        
        // ==> 按理说，对象才有属性和方法 ， 基本数据类型没有
        const num = 10.123
        // num.toFixed() 保留小数位数，返回值是string
        console.log(num.toFixed(2))

        const str = 'hello world'
        console.log(str.length)

        // 基本包装类型： JS底层，将基本数据类型，包装成了复杂数据类型（对象）
        // ==> 基本数据类型，可以使用相应的属性和方法， 原因是底层做了一些处理，包装成了对象
```



## Object静态方法

```js
    const obj = {name:'军军',age:18}
    for(let key in obj){
      // console.log(key) //  属性名
      console.log(obj[key])// 属性值 方括号，key是一个变量
      console.log(obj.key)  // undefined
    }

    // obj['key'] === obj.key     这两个是一样的 key表示属性名
    // obj[key] ==>  key表示一个变量


    // Object.keys(obj) 静态方法
    // 返回一个对象的所有属性名，得到一个数组
    const res = Object.keys(obj)
    console.log(res)

    // Object.values(obj)
    // 返回一个对象的所有属性值，得到的也是一个数组
    const res2 = Object.values(obj)
    console.log(res)
```



## Object.assign()浅拷贝

```js
 // 语法：
    // Object.assign(target, ...sources)
    // 作用： 对象的浅拷贝
    // target : 目标对象
    // sources: 源对象
    // 返回值：目标对象。

    const obj = {}  //目标对象
    const o = {name : '琳琳',age :80} //源对象

    const res = Object.assign(obj,o)
    console.log(res)
    console.log(obj)  //  目标对象被修改了


    // 应用：合并对象
    const o1 = {a : 1}
    const o2 = {b : 2}
    const o3 = {c : 3}

    const res2 = Object.assign(o1,o2,o3)

    console.log(res2)
    console.log(o1 === res2)// 返回值就是目标对象
```



## arr.reduce()

```js
 // 实例方法： 通过构造函数创建的对象，它调用的方法。
        // MDN上 Array.prototype.reduce()  ==> reduce 实例方法
        
        // 语法： arr.reduce(callbackFn, initialValue)
        // 作用： 对数组里的每个元素都执行一个自定义的reducer函数，将其结果汇总为单个返回值。
        //   参数： 
        // callbackFn               :  回调函数   必须     √
        // initialValue             :  初始值     （可选）  √

        // callbackFn的参数：    
        //       previousValue      :  上一次调用callbackFn的返回值    √
        //       currentValue       :  当前元素                      √
        //       currentIndex       :  当前元素的索引  （可选）
        //       array              :  源数组  （可选）

        // 返回值： 使用reducer回调函数遍历整个数组后的结果

        // 注意： 如果有初始值，那么prev第一次执行的时候，就是写的初始值
        //       如果没有初始值，initValue就是数组的第一个元素 arr[0], cur就依次取第二个元素

        
        const arr = [1, 2, 3] 
        const res = arr.reduce(function(pre,cur){
          return pre + cur
        })
        console.log(res)

        const res2 =arr.reduce(function(pre,cur){
          return pre + cur
        },6)
        console.log(res2)
        
      //作用: 常用语数组的求和
      const res3 = arr.reduce((pre,cur) => pre + cur,8)
      console.log(res3)
```



## array常用的方法

```js
    // 1. 语法： 先大致了解
        // 2. 作用： 看这个方法是干嘛的
        // 3. 参数： 熟悉一下参数
        // 4. 返回值： 看是否有返回值

        // 5. 描述： 注意事项 大致过一下
        // 6. 示例代码，测试一下 


        // 语法: arr.find(callbackFn)
        // 作用: 返回数组中满足条件的第一个的元素， 否则返回 undefined
        // 参数: 
        //    cbFn 函数   ==> 注意，这个回调函数，一般需要写上return
        //    cbFn的参数
        //             第一个参数   ： item  当前元素
        //             第二个参数   ： index  索引  （可选）
        // 返回值： 数组中第一个满足条件的元素   / undefined

        // 注意： 返回的是第一个元素

        const arr = ['orange','blue','red']

        const res = arr.find(function(item){
          return item === 'blue'
        })
        console.log(res)

        const res2 = arr.find(el => el === 'blue')


        // 实际应用 可以搜索查找
        // 需求： 找到数组中名字为小米的那个元素？
        const arrTemp = [{name:'小米', price:1999}, {name:'华为', price: 3999}, {name:'Apple', price:8999}]
        const mi = arrTemp.find(item => item.name === '小米')
        const mi2 = arrTemp.find(({name}) => name === '小米')
        console.log(mi)
```



## arr.findIndex()

```js
// 语法：arr.findIndex(cbFn)
        // 作用：查找数组中满足条件的第一个元素，如果满足条件，返回索引，如没没有返回-1
        // 参数：  cbFn
        //    cbFn: 
        //      第一个参数： item  当前数组
        //      第二个参数： index 索引 （可选）
        // 返回值： 索引号/ -1 

        const arrTemp = [{name:'小米', price:1999}, {name:'华为', price: 3999}, {name:'Apple', price:8999}]
        const index = arrTemp.findIndex(item => item.price === 3999)
        console.log(index)




        // 语法：arr.every(cbFn)
        // 作用：检测数组内的所有元素是否都满足指定的条件，如果都满足，返回true  , 否则false
        // 参数：  cbFn     ===> return 
        //    cbFn: 
        //      第一个参数： item  当前数组
        //      第二个参数： index 索引 （可选）
        // 返回值： boolean     true / false
        // 注意： cbFn 要写return

        const arr = [10,20,30]
        const resBool = arr.every(function(item){
          console.log(item)
          return item >= 10
        })
        console.log(resBool)

        const resBool1 = arr.every(item => {
          console.log(item)
          return item >= 10
        })
        console.log(resBool1)


        // 3. 
        // arr.some(cbFn)
        // 作用： 检测数组中的元素是否满足指定的条件，只要有一个满足，就返回true 、 否则false
        const resSome = arr.some(el => el >= 30)
        console.log(resSome)

        // 4. 
        // arr.includes()
        // 作用： 判断数组中是否包含某个元素，如果有， true， 否则false
        const resIn = arr.includes(10)
        console.log(resIn)
```



## str.split()

```js
// 1 .str.split(分隔符)
        // 作用： 使用指定的分隔符，将字符串分割，得到一个字符串数组
        const str = 'Hello-world-JS'
        const words = str.split('-')
        console.log(words)

        // 2. arr.join('分隔符')
        // 作用：将数组转为字符串
        const arr = [2022, 10, 30]
        const res = arr.join('-')
        console.log(res)


        // 3. str.substring(开始索引号[, 结束的索引号]) 
        // 语法：str.substring(indexStart[, indexEnd])
        // 作用：从字符串中截取某一个子段出来
        
        // 注意：
        //   1. 如果省略indexEnd，取到最后 
        //   2. [start, end) 前闭后开区间

        const str2 = '饿了吗？灌醉了？'
        console.log(str2.substring(2))
        console.log(str2.substring(2,4))
```



## startsWith

```js
// 1. str.startsWith()
        // 语法：startsWith(searchString, position)
        // 作用：判断是否以某个字符串开头
            // searchString:  要搜索的字符串
            // position    :  在str中开始搜索的位置，默认为0  可选
        // 返回值  true / false

        const str = "To be, or not to be, that is the question.";

        console.log(str.startsWith("To be")); // true
        console.log(str.startsWith("not to be")); // false
        console.log(str.startsWith("not to be", 10)); // true



        // 2. str.includes(substr[, pos])

        // 作用：判断一个字符串是否包含另一个子串   true / false
        const str2 = "To be, or not to be, that is the question.";
        
        console.log(str2.includes('not to be'))
```

