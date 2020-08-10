## 包含min函数的栈

### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的min函数。在该栈中，调用min,pop,push的时间复杂度都是O(1)。

### 思路

1. 首先在栈中添加一个成员变量存放最小元素。
2. 然后把每次的最小元素都保存起来放入另一个辅助栈中。当有元素从栈中弹出，则辅助栈中同时弹出栈顶元素。
```javascript
function MinStack() {
  this.data = []; //数据栈
  this.temp = []; //辅助栈
}

MinStack.prototype.push = function (val) {
  this.data.push(val);
  if (this.temp.length == 0 || val < this.temp[this.temp.length - 1]) {
    //如果辅助栈为空或辅助栈顶元素小于val
    this.temp.push(val);
  } else {
    //否则就继续把最小值压入辅助栈中
    this.temp.push(this.temp[this.temp.length - 1]);
  }
};

MinStack.prototype.pop = function () {
  this.data.pop();
  this.temp.pop();
};

MinStack.prototype.top = function () {
  return this.data[this.data.length - 1];
};
MinStack.prototype.min = function () {
  return this.temp[this.temp.length - 1];
};
```
