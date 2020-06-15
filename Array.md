# **Array**
## 27. 移除元素
```javascript
var removeElement = function(nums, val) {
   for(var i=0;i<nums.length;i++){
       if(nums[i]==val){
           nums.splice(i,1);//删除原数组中第i位后的1个元素
           i--; 
       } 
   }
   return nums.length;
};
```
**知识点：splice用法**
## 26. 删除排序数组中的重复项
```javascript
var removeDuplicates = function(nums) {
  var i =0;
  var j= 0 ;
  var temp=nums[i]
  while(j<nums.length){
    if(temp==nums[j+1]){//比较相邻两个数
      j++;
    }else{
      nums[i+1]=nums[j+1];
      temp=nums[j+1]
      i++;
    }
  }
  return i;
};
```
**知识点：双指针，j一个从头开始指向相同的数，i一个指向不同的数，temp作为中间数进行比较**
## 80. 删除排序数组中的重复项 II
```javascript
//一个数最多出现两次
var flag = 0;
for(var i =0 ;i<nums.length;i++){
    if(nums[i]==nums[i+1]){//比较相邻两个数
        flag++;//如果相同，则记录次数
        if(flag==2){//如果次数为2了
            flag--; //则计数-1
            nums.splice(i,1);//将原数组里的该位数删除
            i--;
        }
    }else{//如果不同计数清零
        flag =0 ;
    }
}
```
## 189. 旋转数组
```javascript
//给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
var rotate = function(nums, k) {
    if(k>nums.length){
        k = k-nums.length;//减少重复循环
    }
    for(var i=0;i<k;i++){
        nums.unshift(nums.pop())//最后一位移除，插入到第一位
    }
};
```
**知识点：unshift()/shift()/pop()/push()**
## 41. 缺失的第一个正数
```javascript
//给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。
var firstMissingPositive = function(nums) {
    nums.sort((a,b)=>a-b);//从小到大排序
    var reslut = 1;//reslut从1开始，记录未出现过的最小正数
    for(var i= 0;i<nums.length;i++){
        if(nums[i]>0){//如果该数为正数
            if(nums[i]==reslut){//判断该数是否为reslut
                reslut++;//如果相等，则表示该数出现过，reslut递增1
            }
        }
    }
    return reslut
};
```
**知识点：nums.sort((a,b)=>a-b);//从小到大排序**
## 299. 猜数字游戏
```javascript
//请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。
var getHint = function(secret, guess) {
    var s1 = secret.split('')//将字符串变为数组
    var s2 = guess.split('')
    var bulls = 0;
    var cows = 0;
    for(var i = 0;i<s1.length;i++){//循环第一次
        if(s1[i]==s2[i]){
            bulls++;
            //先匹配到的都删掉
            s1.splice(i,1);
            s2.splice(i,1)
            i--;
        }
    }
    for(var j = 0;j<s1.length;j++){//再从删掉后的里面找存在的
        var index = s2.indexOf(s1[j]);//返回在guess中找到的位置
        if(index>-1){
            cows++;
            s2.splice(index,1)
        }
    }
    return bulls+'A'+cows+'B'
};
```
**知识点：indexOf()返回找到的从左到右第一个字符的index,没找到返回-1**
## 134. 加油站
```javascript
var canCompleteCircuit = function(gas, cost) {
    //循环初始加油站
    for(var i =0;i<gas.length;i++){
        var oil = 0;
        var flag = 0;//记录是否回到起点
        var j =i;
        oil+= gas[j];//当前的油
        //当oil够时，循环
        while(oil>=cost[j]&&flag<gas.length){
            oil-=cost[j]//减去路程
            j++;
            if(j==gas.length){//如果循环到尾
                j=0;
            }
            oil+=gas[j];//加油
            flag++;
        }
        if(flag==gas.length){
            return i;
        }
    }
    return -1
};
```
## 274. H 指数
```javascript
var hIndex = function(citations) {
    citations.sort((a,b)=>b-a);//逆序排序
    for(var i = 0;i<citations.length;i++){
        if(citations[i]<=i){
            return i
        }
    }
    return citations.length?citations.length:0
};
```
## 275. H指数 II
```javascript
var hIndex = function(citations) {
    for(var i = 0;i<citations.length;i++){
        if(citations[i]>=citations.length-i){//当前值大于或等于之后的值
            return citations.length-i;
        }
    }
    return 0
};
```
## 217. 存在重复元素
```javascript
//给定一个整数数组，判断是否存在重复元素。
var containsDuplicate = function(nums) {
    return new Set(nums).size != nums.length;
};
```
**知识点：set对象：ES6中set的元素的唯一的,如果重复只取其一**
                  
                  set.size获取set对象长度
                  set.add()向set中新增一个对象
                  set.delete()删除set中指定的对象
                  set.has()查看set中是否包含某个对象
                  set.clear()清空set中所有对象
## 55. 跳跃游戏
```javascript
var canJump = function(nums) {
    var max = 0;//每个位置能走到的最大位置下标
    for(var i= 0;i<nums.length;i++){
        if(i>max) return false;//判断能否走到当前下标i
        max = Math.max(max,nums[i]+i)//更新能走到的最大下标
    }
    return true
};
```
**知识点：贪心算法，每个点都取最大值，判断能到达的最大下标**
## 45. 跳跃游戏 II
```javascript
//使用最少的跳跃次数到达数组的最后一个位置。
var jump = function(nums) {
    //遍历每一步能跳到的区间的下一步的最大距离
    var list_last = 0;
    var max = 0;
    var step= 0;
    if(nums.length==1){
        return 0
    }
    for(var i = 0;i<nums.length;i++){
        max = Math.max(nums[i]+i,max)//记录该区间中，能跳的最大距离
        if(list_last == i){//当i到区间末尾，证明这个区间已经选出了最优的，步数加一
            list_last = max;//更新区间，开始下一个区间的遍历
            step++;
        }
        if(list_last==nums.length-1){//说明选择的这一步可以走到头
            return step;
        }
    }
    return step;
};
```
**知识点：贪心算法，动态规划，遍历每个区间，不用记录所有值，只用记录区间中，能走得最远的位置，当指针走到该位置时，进入下一区间遍历**
## 121. 买卖股票的最佳时机
```javascript
var maxProfit = function(prices) {
    var reslut = 0;
    var min = prices[0];
    for(var i=1;i<prices.length;i++){
        if(prices[i]<min){
            min = prices[i]
        }else{
           reslut = Math.max(reslut,prices[i]-min) 
        }

    }
    return reslut
};
```
## 122. 买卖股票的最佳时机 II
```javascript
var maxProfit = function(prices) {
    var reslut = 0;
    for(var i = 1;i<prices.length;i++){
        reslut+= Math.max(prices[i]-prices[i-1],0) 
    }

    return reslut
};
```
**贪心算法**
## 11. 盛最多水的容器
```javascript
var maxArea = function(height) {
    var reslut = 0;
    var leftIndex = 0;
    var rightIndex = height.length-1;
    while(leftIndex<rightIndex){
        if(height[leftIndex]<height[rightIndex]){
            reslut = Math.max(reslut,(rightIndex - leftIndex)*height[leftIndex] )
            leftIndex++;
        }else{
            reslut = Math.max(reslut,(rightIndex-leftIndex)*height[rightIndex] )
            rightIndex--;
        }
    }
    return reslut
};
```
**知识点：左右两边同时开始，大的不动小的动**
## 42. 接雨水
```javascript
var trap = function(height) {
    if (height.length < 3) return 0
    let leftMax = 0 ;
    let rightMax = 0; 
    let left = [];
    let right = [];
    let reslut = 0;
    for(let i = 0;i<height.length;i++){//从左开始遍历

        if(leftMax<height[i]){
            leftMax = height[i]
        }
        left.push(leftMax)
    }
    for(let j = height.length-1;j>=0;j--){//从左开始遍历
        if(rightMax<height[j]){
            rightMax = height[j]
        }
        right.unshift(rightMax)
    }

    for(let k = 0;k<height.length;k++){
        reslut+= Math.min(left[k],right[k])-height[k]
    }
    return reslut;
};
```
**知识点：动态规划**
## 334. 递增的三元子序列
```javascript
var increasingTriplet = function(nums) {
    let flag =[1];
    for(let i =1 ;i<nums.length;i++){
        let temp = 1;
        for(let j = 0;j<i;j++){
            if(nums[j]<nums[i]&&temp<=flag[j]){//记录最大的j
                temp = flag[j]+1; 
            }
        }
        if(temp==3){
            return true
        }else{flag.push(temp);} 
    }
    return false
};
```
**知识点：动态规划（本题中暴力解法要更简单）**
## 128. 最长连续序列
```javascript
var longestConsecutive = function(nums) {
    let arr = new Set(nums)//去除重复元素，用.has()判断元素是否存在
    let count = 0;
    let i =0;
    let temp=0;
    while(i<nums.length){
        if(!arr.has(nums[i]-1)){//无左边元素
            let j = nums[i]
            temp=1;
            while(arr.has(j+1)){
                temp++;
                j++
            }
            count = Math.max(temp,count)
        }
        i++;
    }
    return count
};
```
**知识点：set.has()的查找算法复杂度为O(1)**
## 164. 最大间距
```javascript
var maximumGap = function(nums) {
    if(nums.length<2){return 0}
    nums.sort((a,b)=>(a-b))
    if(nums.length==2){return nums[1]-nums[0]}
    var i=1;
    var max =0;
    while(i<nums.length){
        max = Math.max(max,nums[i]-nums[i-1]);
        i++;
    }
    return max;
};
```
**知识点：先排序，再循环做差，比较大小**
## 287. 寻找重复数
```javascript
var findDuplicate = function(nums) {
    let arr = nums.sort((a,b)=>a-b)
    for(let i=0;i<nums.length-1;i++){
        if(arr[i]==arr[i+1]){
            return arr[i]
        }
    }
};
```
**知识点：可用对象{}存储undefined位置的数据，如果这个位置已经有人占了则return**
