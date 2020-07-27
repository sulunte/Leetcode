### 一、CSS盒子模型
浏览器布局的基本元素是盒，从里到外依次包括content的width、height、padding、border、margin。标准模式下： `盒子宽度=border*2+padding*2+width(内容)+margin*2` ;怪异模式下： `width=border*2+padding*2+content(内容)` 

box-sizing:content-box;border-box

### 二、选择器优先级
CSS选择器用于选择元素的样式的模式。
+ 基础选择器
  + id选择器：给标签取id名，以#加id名开头
  + 类选择器：给标签取class名，以.class名开头
  + 标签选择器：以标签名开头
  + 通配符选择器：`*{声明}`选择页面所有标签
  + ！important（∞）>行内式（1,0,0,0）>id选择器（0,1,0,0）>类选择器（0,0,1,0）>标签选择器（0,0,0,1）>通配符/继承（0,0,0,0）
+ 复合选择器
  + 后代选择器：`父类名 子类名{}`选择某个父元素的所有子元素
  + 子代选择器：`父类名>子类名{}`选择离父元素最近的子元素
  + 并集选择器：`类名，类名{}`选择多个元素
  + 会有权重叠加，但不会有进位
+ CSS3新特性
  + 属性选择器：`input[属性名]{}`-----(0,0,1,0)
  + 结构伪类选择器：`E:nth-child(n){}`-----(0,0,1,0)
  + 伪元素选择器：`E::before{}  E::after{}`-----(0,0,0,1)

### 三、em rem px
它们都是计量单位，都能表示尺寸
- px：像素，相对于显示器的屏幕分辨率而言，利用px设置的元素比较稳定精确，但是不能适应浏览器的缩放
- em：相对于当前对象内文本的fontSize，可以较好地适应设备屏幕的的大小（多用在移动端开发），但是进行元素设置时需要知道父元素文本的fontSize和当前对象内文本的fontSize
- rem：相对于根元素的fontSize，因此只需要确定一个fontSize就可以了

### 四、居中
+ 水平
  + 文字：text-align:center
+ 垂直
  + 文字：line-height:width-fontsize
  + 行内块元素：vertical-align:middle
+ 居中
  + 盒子：设置width，margin:0px auto
  + 绝对定位的盒子：left:50%//页面的一半 margin-left:width/2//自身宽度的一半
  + 背景：background-position:center
