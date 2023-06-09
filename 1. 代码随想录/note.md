# 代码随想录学习笔记

## 相关网址

[代码随想录 (programmercarl.com)](https://www.programmercarl.com/)

## 写在前面

感谢carl哥的代码随想录网站，这里将算法题目按照题型分类，方便了我学习。

我之前已经刷过一部分，如今重新刷一遍，并且记录下来，时时复习。

## 数组

### 704. 二分查找

[力扣题目链接](https://leetcode.cn/problems/binary-search/)

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

#### 思路

需要注意的点是，while循环的条件，是left<right还是left<=right。

如果一开始选择right=length-1，那循环条件就是left<=right（举一个极端的例子，如果要找的数组只有一个数，那left<=right就能保证第一次查找能进行，反之如果使用left<right作为循环条件，那一次机会都没有了！）

当然，carl哥说的“循环不变量”和“区间”的关系，也值得反复思考。

我这里选择了一个常用的[left,right]这个“左闭右开”区间，所以right=length-1，循环条件是lefft<=right，并且nums[mid]>target时right=mid-1

如果选择[left,right)这个区间，则right=length，循环条件是left<right，并且nums[mid]>target时right=mid

#### 代码

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function(nums, target) {
    let mid;
    let left = 0;
    let right = nums.length - 1;
    while(left <= right) {
        mid = left + ((right - left) >> 1);
        if(nums[mid] == target) {
            return mid;
        } else if(nums[mid] > target) {
            right = mid - 1;
        } else if(nums[mid] < target){
            left = mid + 1;
        }
    }
    return -1;
};
```

#### 注意

我这里一开始用的移位的时候出了问题，我原来写的是`left + (right - left) >> 1`，我本来以为`>>`运算符的优先级高于`+`运算符。然而刚好相反。因此，才改成了代码中的形式。引以为鉴。

#### 相关题目

- [35.搜索插入位置(opens new window)](https://programmercarl.com/0035.搜索插入位置.html)
- [34.在排序数组中查找元素的第一个和最后一个位置(opens new window)](https://programmercarl.com/0034.在排序数组中查找元素的第一个和最后一个位置.html)
- 69.x 的平方根
- 367.有效的完全平方数

我打算做一下相关题目。

### 35.搜索插入位置（相关题目）

[力扣题目链接](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

#### 思路

其实就是一个二分查找的解法，如果找不到，最后跳出循环的时候，left就代表了应该被插入的位置。

我在思考的时候，用[0,2,4,6,8]和需要查找5来理清思路。然后我发现，进行到最后一步left<=right无法满足之前，要么是nums[mid]>target然后right=mid-1（上面的例子就是这种情况），要么是nums[mid]<target然后left+1，这两种情况下，刚好left都等于要被插入的位置。

#### 代码

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
    let mid;
    let left = 0;
    let right = nums.length - 1;
    while(left <= right) {
        mid = left + ((right - left) >> 1);
        if(nums[mid] == target) {
            return mid;
        } else if(nums[mid] > target) {
            right = mid - 1;
        } else if(nums[mid] < target) {
            left = mid + 1;
        }
    }
    return left;
};
```

