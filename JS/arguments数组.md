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
**Array.prototype.slice.call(arguments)**
```javascript
function useArguments() {
    var arr = Array.prototype.slice.call(arguments);
    var sum = 0;
    arr.forEach(function(a) {
        sum += a;
    })
    return sum;
}
```
