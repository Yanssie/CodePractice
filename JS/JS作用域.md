## 1.
```javascript
var foo = 1;
function bar() {
	foo = 10;
	return;
	function foo(){}
}
bar();
alert(foo);
```
**function的定义会提前到当前作用域之前**，所以等同于：
```javascript
var foo = 1;
function bar() {
	function foo() {}
	foo = 10;
	return;
}
bar();
alert(foo);
```
在foo=10的时候，foo是有定义的，属于局部变量，影响不到外层的foo, 结果为alert 1

## 2.
```javascript
function bar() {
    return foo;
    foo = 10;
    function foo() {}
    var foo = 11;
}
alert(typeof bar());
```
等同于：
```javascript
function bar() {
    function foo() {}
    return foo;
    foo = 10;
    var foo = 11;
}
alert(typeof bar());
```
在return之后声明和赋值的foo都无效, 所以alert为 function
## 3.
```javascript
var x = 3;
var foo = {
    x: 2,
    baz: {
        x: 1,
        bar: function() {
            return this.x;
        }
    }
}

var go = foo.baz.bar;

alert(go());
alert(foo.baz.bar());
```
**this指向执行时刻的作用域**, go的作用域是全局，所以相当于window，取到的就是window.x，也就是var x=3;这里定义的x。 <br>
而foo.baz.bar()里面，this指向foo.baz，所以取到的是这个上面的x，也就是1。

## 4.
```javascript
var x = 4,
    obj = {
        x: 3,
        bar: function() {
            var x = 2;
            setTimeout(function() {
                var x = 1;
                alert(this.x);
            }, 1000);
        }
    };
obj.bar();
```
obj作用域是全局，不管有这个setTimeout还是把这个函数立即执行，它里面这个function都是孤立的，this只能是全局的window，即使不延时，改成立即执行结果同样是4。
## 5.
```javascript
x = 1;
function bar() {
    this.x = 2;
    return x;
}
var foo = new bar();
alert(foo.x);
```
2，这里主要问题是最外面x的定义，试试把x=1改成x={}，结果会不同的。这是为什么呢？在把函数当作构造器使用的时候，如果手动返回了一个值，要看这个值是否简单类型，如果是，等同于不写返回，如果不是简单类型，得到的就是手动返回的值。如果，不手动写返回值，就会默认从原型创建一个对象用于返回
## 6.
```javascript
function foo(a) {
    alert(arguments.length);
}
foo(1, 2, 3);
```
答案3，arguments取的是实参的个数，而foo.length取的是形参个数。
## 7. 
```javascript
var foo = function bar() {};
alert(typeof bar);
```
答案：undefined，这种情况下bar的名字从外部不可见，那是不是这个名字别人就没法知道了呢？不是，toString就可以看到它，比如说alert(foo)，可以看看能打出什么。
## 8.
