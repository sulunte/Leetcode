### webpack打包
webpack是一个模块打包工具，能根据模块的依赖关系递归地构建一个依赖关系图

### 九、组件传值
- 父组件向子组件传值：通过super()继承父组件的属性，props调用父组件的方法
- 子组件向父组件传值：通过调用父组件传递到子组件的方法，让父组件自己去修改自己的状态值
- 跨级组件传值：context全局变量。在父组件中定义一个getChildContex(){}，并声明context的类型，子孙组件就可以通过this.context获取到。（生产者-消费者模式）
  *一般不使用，因为当结构复杂时，这种全局变量不易追述到源头，会导致应用变得混乱，不易维护*
- 非嵌套组件通信：可以利用组件的共同父组件的context对象进行通信；也可以使用emitter，在订阅者组件中添加一个this.eventEmitter = emitter.addListener()监听发布者，组件销毁前emitter.removeListener(this.eventEmitter)，在发布者中触发事件emitter.emit()。（发布者-订阅者模式）
  *耦合度较低，易扩展，易测试，灵活性高*
