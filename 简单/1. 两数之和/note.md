# 1. 两数之和
## 思路
嵌套循环,每一个值与剩下的值相加比较是否满足条件
```js
var twoSum1 = function(nums, target) {
    for (let i = 0; i < nums.length; i++) {
        for (let j = 0; j < nums.length; j++) {
            if (j == i) continue
            let res = (nums[i] + nums[j] === target)
            if (res) return [i, j]
        }
    }
};
```
## 优化思路
上述代码假设参数为 `[1,2,3], 5`
* 第一次循环, `1`去和`2,3`判断
* 第二次循环, `2`去和`1,3`判断

**这里就能发现,`1`已经找过`2`了,所以`2`是不需要找前面的数的,,只要继续找后面的数即可**

所以内部循环不应该从0开始, 而是从`i+1`开始
```js
for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
       
    }
}
```
## 最终代码
```js
var twoSum2 = function (nums, target) {
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            let res = (nums[i] + nums[j] === target)
            if (res) return [i, j]
        }
    }
};
```
**注意:** 虽然函数2省了一半循环次数,但提交代码之后,运行时间反而更长了,内存消耗也增加了一点,俗称反向优化...

反向优化(1/1) **完成**