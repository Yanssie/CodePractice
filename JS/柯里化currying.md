### 定义 <br>
js柯里化是逐步传参，逐步缩小函数的适用范围，逐步求解的过程。<br>
![image](https://github.com/Yanssie/CodePractice/blob/master/image/currying.png)
### 例子
```javascript
function curryIt(fn) {
    // 获取fn参数的数量
    var length = fn.length;
    var args = [];
    // 返回匿名函数
    return function(param) {
      args.push(param);
        if(args.length == length) {
            return fn.apply(this, args);
        } else{
            return arguments.callee;
        }
    };
}
```
输入： var fn = function (a, b, c) {return a + b + c}; curryIt(fn)(1)(2)(3);
