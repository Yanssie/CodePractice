### 使用arguments（伪数组） <br>
arguments能获得函数对象传入的参数组，类似与一个数组，能够通过length获取参数个数，
能通过下标获取该位置的参数，但是它不能使用forEach等方法。 <br>
```javascript
function useArguments() {
    var length = arguments.length;
    var sum = 0;
    for(var i = 0; i < length; i++) {
        sum += arguments[i];
    }
    return sum;
}
```
### 把arguments伪数组转为数组
**Array.prototype.slice.call(arguments)** <br>
或**[].slice.call(arguments)**
```javascript
function useArguments() {
    var arr = Array.prototype.slice.call(arguments);
    // var arr = [].slice.call(arguments);
    var sum = 0;
    arr.forEach(function(a) {
        sum += a;
    })
    return sum;
}
```
### 解释
```javascript
function f(a, b, c){
    alert(arguments.length);   // result: "2"
    a = 100;
    alert(arguments[0]);       // result: "100"
    arguments[0] = "haorooms";
    alert(a);                  // result: "haorooms"
    alert(c);                  // result: "undefined"
    c = 2016;
    alert(arguments[2]);       // result: "undefined"
}

f(1, 2);
```
### callee
- arguments.callee返回此arguments对象所在的当前函数引用。<br>
在使用函数递归调用时推荐使用arguments.callee代替函数名本身。
