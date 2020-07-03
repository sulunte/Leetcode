### 一、数据类型
+ 基本数据类型：undefined、null、boolean、number、string、symbol(es6新增)
  + undefined与null区别
  > null表示空，没有对象，在数值转化时，会被转化为0；
  
  > undefined表示一个已经声明但还没有赋值的对象，在数值转化时，会被转化为NaN;
  
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
+ 对象的创建方式：字面量、new、Object.create()
  + new一个对象的过程
  > 1.new是一个关键字，它会先创建一个新对象
  
  > 2.将新对象的__propo__指向构造函数的propotype
  
  > 3.this指向新对象，将构造函数的作用域赋值给新对象
  
  > 4.添加属性给新对象
  
  > 5.返回新对象
  
  > 其实就是一个原型链，构造函数中的 `prototype` 指向原型对象，原型对象中 `constructor` 指向构造函数，new对象的过程创造了一个对象实例，这个实例中 `__proto__` 指向原型对象
  
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
> 1.浏览器解析html源码，创建一个DOM树

> 2.浏览器解析CSS代码，构建CSSOM树

> 3.DOM与CSS生成渲染树 render树

> 4.一旦渲染树创建好了，浏览器就可以根据渲染树直接把页面绘制到屏幕上。


### 七、浏览器缓存
- 强缓存：利用 `expires` 或 `cache-control` 这两个http response header实现的，它们都用来表示资源在客户端缓存的有效期
+ 协商缓存
  + `Last-Modified`，`If-Modified-Since` 控制协商缓存(类似expires，根据客户端的时间)
  
  > 1.浏览器第一次跟服务器请求一个资源时，服务器会在response的header上加一个 `Last-Modified` ，表示这个资源在服务器上最后修改时间
  
  > 2.浏览器再次请求这个资源时，在request的header上加一个 `If-Modified-Since` ，即上次的Last-Modified
  
  > 3.服务器收到时，比较If-Modified-Since与最后修改时间，如果没有变化返回 `304 Not Modified` ，不会返回资源；如果有变化，则正常返回资源
  
  > 4.浏览器收到 `304` 后，会从缓存中加载资源
  
  + `ETag`、`If-None-Match` 控制协商缓存
  
  > 1.浏览器第一次跟服务器请求一个资源时，服务器会在response的header上加一个 `Etag` ，表示服务器根据当前资源生成的唯一标识
  
  > 2.浏览器再次请求这个资源时，在request的header上加一个 `If-None-Match` ，即上次的Etag
  
  > 3.服务器收到时，比较If-None-Match与服务器资源生成的Etag，如果没有变化返回 `304 Not Modified` ，不会返回资源；如果有变化，则正常返回资源
  
  > 4.浏览器收到 `304` 后，会从缓存中加载资源
  
  + 强缓存与协商缓存比较
  
  > 强缓存不发请求到服务器，所以有时候资源更新了浏览器还不知道，但是协商缓存会发请求到服务器；但是在分布式系统中尽量使用 `Last-Modified` ，因为每台服务器生成的 `Etag` 不一样。所以协商缓存需要配合强缓存使用。

  
### 八、Http与Https
+ http：超文本传输协议，提供一种发布和接收html页面的方法。http协议以明文方式发送报文。客户端首先一般是通过tcp建立连接（三次握手）,服务器接收到请求后响应信息。
+ https：是安全版的http。SSL（Secure socket layer）协议位于tcp/ip协议与应用层协议之间，可分为两层：SSL记录协议（位于tcp之上，提供数据封装、数据加密）、SSL握手协议（位于SSL记录协议之上，提供双方身份验证，交换密钥）
  + 数据保密性：数据是加密的
  + 数据完整性：能够及时发现报文被篡改
  + 数据安全性：通过身份验证，保证数据传给可信的对象
  + https原理：
    
    ① 客户端将他所支持的算法列表和一个用作产生密钥的client-random发给服务器；
    
    ② 服务器从算法列表里选择一种算法，将收到的随机数和CA证书加密，同时还提供了一个用作产生密钥的server-random，发送给客户端；（非对称密钥）
    
    ③ 客户端收到后进行解密，验证证书，并从证书中得到公钥，用公钥加密一个随机密码串secret发送给服务器(DH算法)
    
    ④ 服务器和客户端都根据secret和client-random、server-random独立计算出密钥。
    
    ![image](https://pic2.zhimg.com/v2-da07881966009a44b6cdff3b7ef70b3d_r.jpg)
### 九、组件传值
- 父组件向子组件传值：通过super()继承父组件的属性，props调用父组件的方法
- 子组件向父组件传值：通过调用父组件传递到子组件的方法，让父组件自己去修改自己的状态值
- 兄弟组件传值：通过插件pubsub，在一个组件中PubSub.publish()，在另一个组件中通过PubSub.subscribe()

### 十、GET与POST区别
GET与POST都是http请求方式。

1.GET是从服务器上获取数据，POST是向服务器发送数据

2.GET会在地址栏显示查询字符串，POST则不会暴露发送的数据

3.GET请求有数据长度限制，POST请求无数据长度限制

4.GET请求可被缓存，保留在浏览记录中，POST请求不会被缓存，不会保留在历史记录中

### 十一、页面加载慢有什么优化方法
请求方面：减少同时请求的次数，若过多会造成浏览器阻塞，可以合并某些请求；减少请求数据的大小，若数据过大也会造成阻塞，可以保存一些重复请求的数据在本地，也可以分页请求数据；对于过大的图片，可以实行异步加载（懒加载），这样可以保证页面的其他内容能够显示

### 十二、Promise对象
1.Promise对象，用来传递异步消息，代表一个异步操作。它有两个特点：一是对象的状态不受外界影响，三种状态： `pedding（进行中）` , `resolved(完成)` ， `rejected（拒绝）` ，只有异步操作的结果能决定当前是哪一种状态。二是对象的状态一旦改变，就不再变化。只能从 `pedding->resloved` 、 `pedding->rejected` 

2.Promise的作用：解决回调地狱的问题，提高代码的可维护性、可读性

3.在执行的时候，会先将异步任务挂起，继续执行执行栈中的同步任务，当异步任务返回结果后会放入事件队列，等待执行栈中同步任务清空后，再检查事件队列是否有任务，如果有，则执行。

