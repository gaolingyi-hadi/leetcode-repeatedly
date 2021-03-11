# 7. 整数反转
## 思路
把数字转成数组, 再用数组的`reverse()`反转, 最后判断正负号,并且值在范围内即可
```js
var reverse = function (x) {
    let res = 0
    let arr = x.toString().split('')
    if (arr[0] == '-') {
        arr.shift()
        res = Number('-' + arr.reverse().join(""))
    } else {
        res = Number(arr.reverse().join(""))
    }
    if (Math.pow(-2, 31) > res || res > (Math.pow(2, 31) - 1)) return 0
    return res
}
```
## 优化思路
上述代码`if()`太影响读性了,并省去`x=0`这种情况

```js
var reverse2 = function (x) {
    const minus = `${x}`.includes('-');
    let res = minus ? `${x}`.slice(1) : `${x}`;
    res = res.split("").reverse().join("");
    if (minus) { res *= -1 }
    if (Math.pow(-2, 31) > res || res > (Math.pow(2, 31) - 1)) return 0
    return res;
};
```
稍微好读了那么一点点,时间花费差不多, 其实区别就在于判断符号的时机不一样,但是这3步是必须要的
*   判断符号
*   反转数字
*   判断区间