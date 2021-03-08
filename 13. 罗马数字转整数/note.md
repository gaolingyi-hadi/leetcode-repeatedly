# 13. 罗马数字转整数
## 思路
先把字符串转成数组,再遍历转换成数字累加,这样先成功转换成数字
```js
 var romanToInt = function (s) {
        const rome = {
            I: 1,
            V: 5,
            X: 10,
            L: 50,
            C: 100,
            D: 500,
            M: 1000
        }
        let arr = s.split('');
        let res = 0;
        for (let i = 0; i < arr.length; i++) {
            res += rome[arr[i]]
        }
        return res
    };
}
```
### 特殊条件
罗马数字有这六种特殊条件
```
I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
```
在出现 `I,X,C`时,对比下一个值满不满足对应条件
满足则让后面这个数-前面这个数

比如 出现`I`时,看一下后面那个数是不是`V`,是的话就`V-I`,表示 5-1 = 4

因为当前数的索引数是`i`,后面那个数的索引是`i+1`,i++ 表示后面这个数用过了,不需要再判断,跳过下次循环
```js
if (arr[i] === 'I') {
    if (arr[i + 1] === 'V' || arr[i + 1] === 'X') {
        res += rome[arr[i + 1]] - rome[arr[i]]
        i++
    }else{
        res += rome[arr[i]]
    }
}
```
上述代码重复3遍可得最终通过代码
```js
var romanToInt = function (s) {
    const rome = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    }
    let arr = s.split('');
    let res = 0;
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === 'I') {
            if (arr[i + 1] === 'V' || arr[i + 1] === 'X') {
                res += rome[arr[i + 1]] - rome[arr[i]]
                i++
            }else{
                res += rome[arr[i]]
            }
        } else if (arr[i] === 'X') {
            if (arr[i + 1] === 'L' || arr[i + 1] === 'C') {
                res += rome[arr[i + 1]] - rome[arr[i]]
                i++
            }else{
                res += rome[arr[i]]
            }
        } else if (arr[i] === 'C') {
            if (arr[i + 1] === 'D' || arr[i + 1] === 'M') {
                res += rome[arr[i + 1]] - rome[arr[i]]
                i++
            }else{
                res += rome[arr[i]]
            }
        } else {
            res += rome[arr[i]]
        }
    }
    return res
};
```
## 优化
字符串只有一个罗马字符时,我们可以不用执行循环;字符串有多个字符时,最后一个字就不用判断了,直接取出,所以把条件变一下, 这样能省下一次循环.
```js
for (let i = 0; i + 1 < arr.length; i++){}
```
未优化的代码判断特殊值时,会去取下一个值看满不满足条件,但标准的罗马数字,一般都是从大到小的顺序表示数字

比如表示6时, 是`VI`,等于`5+1`

而不是用`IVII 4+1+1`表示

所以如果下一位罗马数字大于当前的数字,就能判定是特殊情况
```js
for (let i = 0; i + 1 < arr.length; i++){
    if(rome[arr[i]] < rome[arr[i+1]]){
        //是特殊情况
    }else{
        //不是特殊情况
    }
}
```
下面是优化过的代码
```js
var romanToInt2 = function (s) {
    const rome = {
        I: 1,
        V: 5,
        X: 10,
        L: 50,
        C: 100,
        D: 500,
        M: 1000
    }
    let pre = rome[s[0]];
    let res = 0;
    /*
    提示: 'XIV' = X+(V-I) = 10+(5-1) = 10+5+(-1) 10+(-1)+5
    加法交换律,可以理解为需要减的值能提前减
    */
    for(var i = 0; i + 1 < s.length; i++){
        let next = rome[s[i + 1]];
        pre < next ? res -= pre : res += pre
        pre = next;
    }
    res += pre;
    return res;
};
```
## 解释
假设`s = XIV => 10, 1, 5`带入函数

`pre = rome['XIV'[0]] = 10` 开始进入循环

* 第一次循环: 
```
next = rome['XIV'[i + 1]] = 1
10 < 1 不成立, 于是走 res = 0 + 10 = 10
pre = 1
``` 
* 第二次循环: 
```
next = rome['XIV'[i + 1]] = 5
1 < 5 成立, 于是走 res = 10 - 1 = 9
pre = 5
``` 
* i自增2次之后值为2,不满足循环条件,说明pre保留的是最后一个值,最后一个值永远都可以直接加,所以不用判断,出循环后直接相加即可



