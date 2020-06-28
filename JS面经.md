### 一、数据类型
- 基本数据类型：undefined、null、boolean、number、string、symbol(es6新增)
- 引用数据类型：object、array、function
- 数据类型检测：typeof、instanceof
```javascript
typeof对于基本类型来说，除了null都可以正确显示，对于对象来说，除了函数都会显示object
instanceof是根据原型链来判断数据类型的
Object.prototype.toString.call()可以检测所有的数据类型
```
+ 浅拷贝：只拷贝一层，更深层次的对象级别只拷贝引用
  + Object.assign(拷贝给谁，拷贝的对象)(es6)
  + var a={} ; a=o ;
+ 深拷贝
  + JSON.parse(JSON.stringify(obj))//将对象转为json字符串，再转为json对象
  + 递归+浅拷贝

### 二、作用域
- 变量声明提升：在预解析阶段，js引擎会把函数声明与变量声明提升到作用域顶部，（函数声明优先级，高于变量声明优先级）
- 作用域链：内部函数访问外部函数的变量，采用链式查找
+ 闭包：有权访问另一个函数作用域中变量的函数（被访问的变量所在的函数就是闭包）
  + 引用的变量可能发生变化（采用立即执行函数封装）
  + this指向
  + 内存泄漏（手动释放）
+ 闭包的作用：延长变量的作用范围

### 三、原型与继承
- 对象的创建方式：字面量、new、Object.create()
- 原型链：遍历一个实例的属性时，先遍历实例对象上的属性，再遍历它的原型对象，一直遍历到Object
+ 继承
  + 借用构造函数继承属性
    ```javascript
    function Animal(name,color){
       this.name = name;
       this.age = color;
    }
    
    function Cat(name,color,score){
      Animal.apply(this, arguments);
      this.score = score;
    }
    ```
  + 借用原型对象继承方法
    ```javascript
    function Animal(name,color){
       this.name = name;
       this.age = color;
    }
    Animal.prototype.say=function(){
      console.log('hhhh')
    }
    
    function Cat(name,color,score){
      Animal.apply(this, arguments);
      this.score = score;
    }
    Cat.prototype = new Animal();//原型对象指向实例
    Cat.prototype.constructor = Cat;//构造函数指向自己
    ```
  + extends继承
    ```javascript
    class ColorPoint extends Point {
      construcor(x,y,color){
        super(x,y);//调用父类的constructor(x,y)
        this.color = color;
      }
      toString(){
        retrun this.color +''+super.toString();//调用父类的toString()
      }
    }
    ```
   
### 四、call、apply、bind
- bind()是返回新函数，当调用这个新函数时，会以创建它时传入bind()方法的第一个参数作为this
- call()、apply()立即调用
- call()传递参数列表、apply()传递数组

### 五、Event Loop
  - 主线程运行的时候会生成堆和栈；
  - JS从上到下解析，将其中的同步任务按照执行顺序排列到执行栈中；
  - 当程序调用外部的API时（ajax,setTimeout），会将此类异步任务挂起，继续执行执行栈中的任务。等异步任务返回结果后，再按照顺序排列到事件队列中；
  - 主线程先将执行栈中的同步任务清空，然后检查事件队列中是否有任务，如果有，就将第一个事件对应的回调推到执行栈中执行，若在执行过程中遇到异步任务，则继续将这个异步任务排列到事件队列中；
  - 主线程每次将执行栈清空后，就去事件队列中检查是否有任务，如果有，就每次取出一个推到执行栈中执行，这个循环往复的过程就被称为“Event Loop 事件循环”。

### 六、浏览器页面渲染过程
1.浏览器解析html源码，创建一个DOM树
2.浏览器解析CSS代码，构建CSSOM树
3.DOM与CSS生成渲染树 render树
4.一旦渲染树创建好了，浏览器就可以根据渲染树直接把页面绘制到屏幕上。

### 七、浏览器缓存
- 强缓存：利用expires或cache-control这两个http response header实现的，它们都用来表示资源在客户端缓存的有效期
- 协商缓存：Last-Modified，If-Modified-Since控制协商缓存
