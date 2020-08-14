### 一、数据类型
+ 基本数据类型：undefined、null、boolean、number、string、symbol(es6新增)
  + undefined与null区别
  > null表示空，没有对象，在数值转化时，会被转化为0；
  
  > undefined表示一个已经声明但还没有赋值的对象，在数值转化时，会被转化为NaN;
  
- 引用数据类型：object、array、function
- 数据类型检测：typeof、instanceof
```javascript
typeof对于基本类型来说，除了null都可以正确显示，对于对象来说，除了函数都会显示object
instanceof是根据原型链来判断数据类型的,不能判断prototype
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
    + 在箭头函数中，this的指向是在定义时由词法作用域模型决定的，this继承外层普通函数的this或windows。
    + 在普通函数中，this的指向是被调用时决定的。
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
  1. 创建了一个新函数
  2. 将传入的参数绑定到该函数
  3. 参数合并
  4. 返回该函数
  ```javascript
  Function.prototype._bind = function(){
  //首先去缓存参数列表，避免直接更改参数列表
  let _arguments = arguments;
  //类数组转换成数组
   _arguments = Array.prototype.slice.call(_arguments);
  //拿到当前方法
  let _this = this;
  //拿到指定的this值,shift操作会改变原数组
  let target = Array.prototype.shift.call(_arguments);
  // 创建一个函数,并执行合并之后的参数
  let fn = function(){//是不是被当做构造函数使用了
    let ctx = this instanceof fn ? this : target
     _this.apply(ctx,_arguments.concat(Array.prototype.slice.call(arguments)));
  }
  //添加原函数所有的prototype的值
  fn.prototype = Object.create(this.prototype);
  //最后返回这个方法
  return fn
  }
  ```
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

> 3.DOM与CSS生成渲染树 render树，只包含可见节点

> 4.reflow:计算节点在可视窗口中的位置和大小

> 5.repaint:最后将渲染树上每个节点都转化为屏幕上的实际像素，把页面绘制到屏幕上。

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


### 十、GET与POST区别
GET与POST都是http请求方式。

1.GET是从服务器上获取数据，POST是向服务器发送数据

2.GET会在地址栏显示查询字符串，POST则不会暴露发送的数据

3.GET请求有数据长度限制，POST请求无数据长度限制

4.GET请求可被缓存，保留在浏览记录中，POST请求不会被缓存，不会保留在历史记录中

5.GET操作是幂等性的，执行操作相同，结果也相同；POST不是幂等性

6.GET只能进行URL编码，只接受ASCII字符；POST没有限制

7.GET在发送时会一次性发送请求；POST会先发一个header，当响应码100后再发送完整的数据

### 十一、页面加载慢有什么优化方法
请求方面：
  + 减少同时请求的次数，若过多会造成浏览器阻塞，可以合并某些请求；
  + 减少请求数据的大小，若数据过大也会造成阻塞，可以保存一些重复请求的数据在本地，也可以分页请求数据；
  + 对于过大的图片，可以实行异步加载（懒加载），这样可以保证页面的其他内容能够显示
  
渲染方面：由于每次reflow都会额外的计算消耗，浏览器通常会通过队列化修改并且批量执行reflow过程。但是当访问了offset-left/width/height、scroll-left/top/height等属性，会强制清空队列，也就是强制回流。
  + 减少发生reflow和repaint的次数：
    + 合并多次对DOM和样式的修改；
    + 使用visibility代替display;
    + 避免使用tabel布局

### 十二、Promise对象
回调函数：自定义，且不亲自调用，但会执行
同步回调：遍历回调，不会放入队列
异步回调：会放入队列，将来执行

常见的内置错误：ReferenceError 引用的变量不存在
               TypeError 数据类型不正确
               RangeError 数据值不在其所允许的范围内
               SyntaxError 语法错误
            
捕获错误：`try{}catch(error){//输出error对象属性error.message  error.stack}`
抛出错误：`throw new Error('message')`

async 函数：返回promise对象
await 表达式（promise或其他）：等待表达式成功的返回结果，如果想得到失败的结果，要用try catch

宏队列：ajax回调、dom事件回调、定时器回调
微队列：promise回调、mutation回调
每次准备取出第一个宏任务之前，要清空微队列

1.抽象的来说，promise是一种解决异步编程的新方法（以前用的是纯回调，这样会产生回调地狱）。具体来说，Promise是一个构造函数，它的实例对象，用来封装一个异步操作，传递异步消息。它有两个特点：一是对象的状态不受外界影响，三种状态： `pedding（进行中）` , `resolved(完成)` ， `rejected（拒绝）` ，只有异步操作的结果能决定当前是哪一种状态。二是对象的状态一旦改变，就不再变化。只能从 `pedding->resloved` 、 `pedding->rejected` 

2.promise的状态改变：resolve,reject,throw new Error('reason')

2.Promise的作用：指定回调函数的方式更灵活（旧的必须在启动异步任务前指定，promise可在任意时刻指定回调函数）；支持链式调用，所以可解决回调地狱的问题，提高代码的可维护性、可读性

3.在执行的时候，会先将异步任务挂起，继续执行执行栈中的同步任务，当异步任务返回结果后会放入事件队列，等待执行栈中同步任务清空后，再检查事件队列是否有任务，如果有，则执行。

### 十三、XSS攻击
+ 反射型XSS（非持久化）：需要用户主动点击连接

### 十四、link和@import
+ link:引入CSS文件。
```javascript
<link href="CSSurl路径" rel="stylesheet" type="text/css" />
```
+ @import：引入CSS文件。
```html
<style type="text/css">
@import url(CSS文件路径地址);
</style>
```
```css
@import url(CSS文件路径地址);
```
+ 区别

1. 加载页面时，link标签引入的CSS同时被加载，@import引入的CSS将在页面加载完毕后被加载
2. 兼容性，link是HTML元素不存在兼容性问题，@import只可在IE5+才可识别
3. 可以通过JS操作DOM，插入link标签；无法使用@import插入
4. link的权重大于@import引入的样式

### 十五、如何解决跨域问题

+ 浏览器的同源策略：如果两个URL的协议、主机、端口号都相同的话，则这两个URL是同源
  URL           | 结果          | 原因
  ------------- | ------------- | -------------
  http://store.company.com/dir2/other.html | 同源 | 只有路径不同
  http://store.company.com/dir/inner/another.html | 同源 | 只有路径不同
  https://store.company.com/secure.html | 失败 | 协议不同
  http://store.company.com:81/dir/etc.html | 失败 | 端口不同 ( http:// 默认端口是80)
  http://news.company.com/dir/other.html | 失败 | 主机不同
  
  因为浏览器的同源策略所以才有了跨域问题。

+ 解决方法

  1. JSONP
  2. CORS（跨域资源共享）
  简单请求：就是在头信息之中，增加一个Origin字段。后端会有一个'Access-Control-Allow-Origin'字段检测。
  非简单请求：非简单请求会发出一次预检测请求，返回码是204，预检测通过才会发真正的请求，才返回200。这里通过前端发请求的时候增加一个额外的header来触发非简单请求。除了Origin还需增加Access-Control-Request-Method和Access-Control-Request-Headers字段，后端会响应Access-Control-Allow-Methods、Access-Control-Allow-Headers、Access-Control-Allow-Credentials（表示是否同意访问）
  
### 十六、CDN 内容分发网络



### script标签为什么放在body后面，defer/async

当浏览器加载一个含有<script>标签的页面时，会发生以下动作：
1. 获取HTML文件，拉取HTML页面(比如：index.html)
2. 开始解析html文件
3. 当解析器遇到一个<script>标签时，准备去获取<script>标签对应的js文件
4. 当解析器获取js文件时，同时中断了页面上其他html的解析
5. 一段时间后，js文件解析完毕，页面上其他的html标签继续解析

+ 废弃的解决方式：将<script>标签放在html文件的<body>标签之后执行
  
但这种加载方式存在的问题就是只有当所有的html元素加载完成后才能开始加载<script>标签的内容，如果这个加载需要花很长时间的话，那在这段时间内，就无法操作页面，要知道两秒之内不能操作页面的话，这个用户体验是非常差的。

+ 现在的解决方案
  + async：标记的 <script> 异步下载并执行。这意味着<script>下载时不阻塞HTML的解析，并且下载结束<script>马上执行。（代码 script2 可能比 script1 先下载完并执行完）
  ```html
  <script type="text/javascript" src="path/to/script1.js" async></script>
  <script type="text/javascript" src="path/to/script2.js" async></script>
  ```
  + defer：标记的script顺序执行，这种方式也不会阻断浏览器解析HTML,同时下载结束时<script> 马上执行（代码中 script2 肯定比 script1 后执行完）
  ```html
  <script type="text/javascript" src="path/to/script1.js" defer></script>
  <script type="text/javascript" src="path/to/script2.js" defer></script>
  ```
### setState()为什么是异步处理

### let 与 var
变量提升；块级作用域；let不能重复声明
### 普通函数与箭头函数
this指向；new 实例化;arguments
