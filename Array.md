# **Array**
## 27. 移除元素
```
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
知识点：splice用法
## 26. 删除排序数组中的重复项
```
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
知识点：双指针，j一个从头开始指向相同的数，i一个指向不同的数，temp作为中间数进行比较
## 80. 删除排序数组中的重复项 II
```
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
```
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
知识点：unshift()/shift()/pop()/push()
## 41. 缺失的第一个正数
```
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
知识点：nums.sort((a,b)=>a-b);//从小到大排序
## 299. 猜数字游戏
```
//请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。

```
