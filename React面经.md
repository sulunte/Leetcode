### webpack打包
webpack是一个模块打包工具，能根据模块的依赖关系递归地构建一个依赖关系图

### 九、组件传值
- 父组件向子组件传值：通过super()继承父组件的属性，props调用父组件的方法
- 子组件向父组件传值：通过调用父组件传递到子组件的方法，让父组件自己去修改自己的状态值
- 跨级组件传值：context全局变量。在父组件中定义一个getChildContex(){}，并声明context的类型，子孙组件就可以通过this.context获取到。（生产者-消费者模式）
  *一般不使用，因为当结构复杂时，这种全局变量不易追述到源头，会导致应用变得混乱，不易维护*
- 非嵌套组件通信：可以利用组件的共同父组件的context对象进行通信；也可以使用emitter，在订阅者组件中添加一个this.eventEmitter = emitter.addListener()监听发布者，组件销毁前emitter.removeListener(this.eventEmitter)，在发布者中触发事件emitter.emit()。（发布者-订阅者模式）
  *耦合度较低，易扩展，易测试，灵活性高*

### 组件的生命周期

- 初始化：constructor()--->getDerivedStateFromProps()--->render()--->componentDidMount()
- 更新时：getDerivedStateFromProps()--->shouldComponentUpdate--->render()--->getSnapShotBeforeUpdate()--->componentDidUpdate()

### Ref、onRef

- onRef():通过 props 的事件机制将组件的 this(组件实例)当做参数传到父组件,父组件就可以操作子组件的 state 和方法
- ref:就是通过 React 的 ref 属性获取到整个子组件实例,再进行操作

### Diif算法

- tree Diff
 tree diff是两棵新旧虚拟DOM树，按照层级的对应关系，把同一层级的结点遍历一遍，这样就能快速找到有差异的地方。它们都是同一父节点下的子节点进行比较，父节点不同就无须比较下面的节点了。（比较适用于界面DOM节点跨层级操作少的情形）

- component diff
 对于同类型的组件，根据虚拟DOM树按照原来的策略进行比较tree即可
 对于不同类型的组件，react会将这个组件内部所有的子节点重新替换。而这个组件在react中被称为dirty component。

- element diff
 react在遇到类型相同的组件时，会继续对组件内部元素进行对比，检查内部元素异同。先将新节点集合进行遍历，然后通过唯一标识key去老的节点集合中寻找是否命中，如果有就执行移动操作。
 
 ### reflow（重绘）和repaint（重排）
 
 重绘和重排是影响页面性能的最大因素。使用diff算法可以把新老虚拟DOM树进行比较，如果新老虚拟DOM树相等，则不重新渲染，否则就重新渲染。
 
 在react中，任何时刻调用setState()方法，react不是立即对其更新，而是先标记为脏状态，使用事件轮询对变更做批量处理绘制。当事件轮询结束后，react会将标记dirty的组件及其子节点进行重绘。
 
 不过react还是提供了一种方法对这些后代节点进行优化，可以阻止后代节点树的重绘，那就是重写shouldComponetUpdate()生命周期函数，设置为false，当前组件不去渲染，从而提升更新速度。
 
