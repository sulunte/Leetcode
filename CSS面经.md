### 一、CSS盒子模型
浏览器布局的基本元素是盒，从里到外依次包括content的width、height、padding、border、margin。标准模式下： `width=width` ;怪异模式下： `width=border*2+padding*2+width` 

### 二、选择器优先级
CSS选择器用于选择元素的样式的模式。
- 最高级别是直接在标签中设置的样式
- id选择器：给标签取id名，以#加id名开头
- 类选择器：给标签取class名，以.class名开头
- 标签选择器：以标签名开头

### 三、em rem px
它们都是计量单位，都能表示尺寸
- px：像素，相对于显示器的屏幕分辨率而言，利用px设置的元素比较稳定精确，但是不能适应浏览器的缩放
- em：相对于当前对象内文本的fontSize，可以较好地适应设备屏幕的的大小（多用在移动端开发），但是进行元素设置时需要知道父元素文本的fontSize和当前对象内文本的fontSize
- rem：相对于根元素的fontSize，因此只需要确定一个fontSize就可以了

