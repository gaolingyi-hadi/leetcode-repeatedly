# 9. 回文数
## 思路
可以将数字转成数组,遍历`length/2`次,头尾数字作比较,即`i`和`length - 1 - i` 比较,只要不相同就返回`false`
```js
if (num_arr[i] != num_arr[num_arr.length - 1 - i]) {
    return false
}
```
## 完整代码
```js
var isPalindrome = function (x) {
    let num_arr = x.toString().split('')
    let max = Math.floor(num_arr.length / 2);
    for (let i = 0; i < max; i++) {
        if (num_arr[i] != num_arr[num_arr.length - 1 - i]) { return false }
    }
    return true
};
```
## 优化思路

## 最终代码