# 二分查找——一份略显废话的二分查找学习文档。  
本文档力求一份文档讲通讲透二分查找，因此思路介绍尽量详细，可能略显废话，请尽量保持耐心和信心学完简介却并不简单的二分查找  

首先初步认识二分查找的原理和应用范围。  
二分查找就是利用二分法，不断将搜索范围减半从而快速在有序数组中查找目标值的下标。  
根据搜索区间的表示方式的不同，常见的二分查找有两个版本，左闭右开和左闭右闭。
```cpp
//左闭右开版本
//确定初始搜索范围
int left = 0;
int right = nums.size();
//构建循环进行搜索
while(left<right){
  int mid = left + (right-left)/2;
  int midValue = nums[mid];
  if(midValue == target)
    return mid;
  if(midValue < target)
    left = mid+1;
  if(midValue > target)
    right = mid;
}
// 搜索结束后处理
return -1;
```

```cpp
//左闭右闭版本
//确定初始搜索范围
int left = 0;
int right = nums.size()-1;
//构建循环进行搜索
while(left<=right){
  int mid = left + (right-left)/2;
  int midValue = nums[mid];
  if(midValue == target)
    return mid;
  if(midValue < target)
    left = mid+1;
  if(midValue > target)
    right = mid-1;
}
// 搜索结束后处理
return -1;
```

首先确定好搜索范围的表示方式，根据搜索范围，确定循环结束条件和边界移动方式。
左右坐标内为待搜索元素。
初始搜索范围必须包含0到nums.size()-1，循环结束条件为搜索范围内无元素。
二分搜索的代码结构如下

二分搜索的代码可以分为三部分，第一部分，确定初始搜索范围，第二部分，构建循环进行搜索，第三部分，循环结束后的处理。

主流的思路中，搜索范围的表示方式有两种，左闭右开和左闭右闭。

下面先以左闭右开为例按顺序进行解析。

## 第一种，左闭右开[left,right)
即待搜索元素下标必须满足大于等于left且小于right。
### 确定初始搜索范围
nums数组中元素下标范围为0到nums.size()-1。

初始范围：要使[left,right)满足包括0到nums.size()-1，则需要满足，left<=0,nums.size()-1<right。因此[left,right)最小范围为left=0，right=nums.size()。
### 构建循环进行搜索
二分法基本思路是通过判断中间元素值与目标值的关系来缩小搜索范围。  
搜索范围[left,right)内**存在**待搜索元素时继续进入循环体，通过增大left的值或减小right的值来缩小[left,right)表示的范围。  
搜索范围[left,right)内**不存在**待搜索元素时，循环结束。即我们无法预知循环需要进行的次数，因此需要采用while循环而不是for循环。 

使用while循环需要先确定循环进行条件。此处循环进行条件若写错就会进入死循环导致超时错误。

对于搜索问题，当搜索范围内仍有待搜索元素时才进行循环。下面使用分类讨论的方法，将“搜索范围内存在待搜索元素”转为数学语言。

对于左开右闭区间[left,right)，区间内存在待搜索元素即为存在整数x满足left <= x < right。

初始条件为left = 0，right = nums.size()，nums中所有元素坐标均满足条件。进入循环后，left和right的值不断被改变，
我们知道当left < right时，存在整数x = left使得left <= x且left = x < right。而left = right时，不存在整数x满足right = left <= x且x < right。当left > right时，不存在整数x满足即大于等于left又小于
循环条件：当left>=right时，不存在整数t满足left<=t且t<right。因此循环条件为left<right。

l等于r时，区间内无元素，搜索停止
判断中间值m与目标值t的关系
如果t大于m，说明目标值在右半区，移动左边界。
如果t小于m，说明目标值在左半区，移动右边界。
移动边界的方式
因为m已经确定不等于t，所以要保证移动后的搜索区域内不包含m
左边界，闭区间，会取到索引为l的元素，所以l要移动到m+1，保证不会再取到m。
右边界，开区间，不会取到索引为r的元素，所以r移动到m，保证不会取到m

### 第二种，左闭右闭[left,right]
左闭右闭区间，初始条件，l为0，r为size-1，判断条件相同，区别在于，移动方式。
l大于r时范围内无元素，搜索停止
左边界，会取到，因此移到m+1
右边界，会取到，因此移动到m-1

循环内，三种条件，不论哪种都会直接跳出或缩小范围，则最终循环一定会被跳出

左闭右开，循环条件小于等于的时候，会死循环。卡死在右边界
左闭右闭，循环条件小于，会导致少搜索
