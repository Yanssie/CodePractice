## 1. time timeout
```javascript
    console.time(label)
    console.timeout(label); // 记录从time到timeout的时间
```
JS单线程执行
## 2.判断题
>* A、null instanceof Object //false
>* B、null == undefined      //true
>* C、NaN == NaN             //false
>* D、false == undefined     //true

**null** 是对象，但值为空，type of null 返回Object，参与运算时值为0<br>
**undefined** 是全局对象的一个属性，值未定义，type of undefined 返回undefined，Boolean(undefined) == false
**NaN** 与任何数值都不等的数字
## 3.JS预加载
- 预加载：确定var作用域（全局／局部），值为undefined
- 执行：确定具体类型<br>
在func范围内查找var的值，若有var定义的局部变量，返回其值，若没有，查找全局变量
```JavaScript
var name = 'World!';
(function () {
  if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();

```
//Goodbye Jack
## 4.事件流阶段
- 捕获阶段：addListener设为true
- 处于目标阶段
- 冒泡阶段：addListener设为false<br>

1.`parentEle.addEventListener("click", parentDoSomething, true);`
2.`childEle.addEventListener("click", childEleDoSomething, false);`

如果用户点击子元素`childEle`会发生如下事情：

>a、点击事件开始于`捕获阶段`。它会先查询是否有`childEle`的任何祖先元素在`捕获阶段`绑定了`onclick`事件。

>b、它发现祖先元素1在捕获阶段绑定了`onclick`事件，于是`parentEle.parentDoSomething()`首先被执行。

>c、事件一直查询到目标元素`childEle`都没有再发现别的在`捕获阶段`绑定的`onclick`事件，事件转到它的`冒泡阶段`并执行`childEleDoSomething()`(注册在`childEle`上的在`冒泡阶段`执行的事件处理程序)。

>d、事件再次向上查询并检查是否有任何祖先元素在`冒泡阶段`绑定了`onclick`事件，并没有查询到，所以什么都没有发生。


再看相反的例子：

1.`parentEle.addEventListener("click", parentDoSomething, false);`

2.`childEle.addEventListener("click", childEleDoSomething, false);`

现在如果用户点击`childEle`，下面的事情会按顺序发生：

>a、点击事件发生于`捕获阶段`。事件查询`childEle`是否有任何祖先元素在`捕获阶段`绑定了`onclick`事件并且没有查找到这样的元素。

>b、事件查询到目标元素`childEle`自己。事件转为`冒泡阶段`并执行`childEleDoSomething()`。

>c、事件再次向上查询并检查目标元素是否有任何祖先元素在`冒泡阶段`绑定了`onclick`事件。

>d、它找到了满足条件的`parentEle`，然后执行`parentDoSomething()`。所以选项`C`是`错`的，应该是`fn2`先触发。


因为`IE`没有提供对`事件捕获阶段`的支持，所以`IE`跟`标准浏览器`对于`DOM事件流`实现不一样。
## 5.获取页面的html元素
>* A、document.getElementById	
>* B、document.getElementsByClassName
>* C、document.querySelector
>* D、document.querySelectorAll
## 6.IE W3C
- IE: attachEvent/detachEvent方法来添加和删除事件监听器; 全局的Event对象; 阻止冒泡：设置event对象的cancelBubble为true
- W3C: addEventListener/removeEventListener; event对象作为参数传递给监听器; 执行stopPropagation方法
## 7.array引用
```JavaScript
var array1 = [1,2];

var array2 = array1;

array1[0] = array2[1];

array2.push(3);

console.log(array1);

console.log(array2);
```
// Array1的值为[2,2,3];Array2的值为[2,2,3]
## 8.原型，原型链
- prototype<br>
**JS继承机制**，进行属性继承<br>
所有对象继承自Object.prototype
- 原型链<br>
- hasOwnProperty<br>
JS中唯一处理属性，但不查找原型链的函数<br>
```JavaScript
function Test(name,age){
	this.name = name;
this.age = age;
};
Test.prototype = {
	name:'aliyun',
	hasOwnproperty:function(){
	return false;
}
};
var instance = new Test('alibaba',102);
```
## 9.实现函数range([start,]stop[,step])返回一个数组（step大于1）
```JavaScript
> range(1,11); => [1,2,3,4,5,6,7,8,9,10]

> range(0); => []

> range(10); => [0,1,2,3,4,5,6,7,8,9]

> range(0,30,5); => [0,5,10,15,20,25]
```
<i class="icon-pencil"></i> 解析：
实现代码如下：
```JavaScript
function range(){
    
	var argLength = arguments.length,
	
	    newArray = [],
	
	    i = 0,
	
	    start = arguments[0],
	
	    stop = arguments[1],
	
	    step = arguments[2];
	
	switch(argLength){
	
		case 0 : throw Error('至少输入一个参数,限止数组在哪里结束');
		
		case 1 : {
		
		    stop = arguments[0];
		    
		    for( i = 0 ; i < stop ; i++){
		    
		    	newArray.push(i);
		    	
		    }
		    
		    return newArray;
		    
		}
		
		case 2 : {
		
		    for( i = start ; i < stop ; i++){
		    
		    	newArray.push(i);
		    	
		    }
		    
		    return newArray;
		    
		}
		
		case 3 : {
		    
		    if(step < 1) {
		    
		        throw Error('step > 1');
		
		    }else{
		    
			for( i = start ; i < stop ; i += step){
		    
		    		newArray.push(i);
		    		
		    	}
		    	
		    	return newArray;
		    }
		
		}
		
		default : throw Error('最多传入三个参数');
		
	}
	
}
```
## 10.消息处理器
>1、对象A直接调用对象B的某个方法，实现交互逻辑。但是导致的问题是A和B紧密耦合，修改B可能导致A调用B的方法失效。
>2、为了解决耦合导致的问题，我们可以设计成：
对象A生成消息 -> 将消息通知给一个消息处理器（Observable）-> 消息处理器将消息传递给B
具体的调用过程变成：
A.emit(‘message’,data); B.on(‘message’,function(data){});
请实现这一事件消息代理功能
//请将事件消息功能补充完整
function EventEmitter(){}

<i class="icon-pencil"></i> 解析：

实现代码如下：
```JavaScript
function EventEmitter() {

	this.eventFunctionMap = {};
	
}

EventEmitter.prototype.emit = function(eventName, data){

	this.eventFunctionMap[eventName].call(this, data);
	
}

EventEmitter.prototype.on = function(eventName, callback) {

	this.eventFunctionMap[eventName] = callback;
	
}
```
## 11.布局
```css
div{
	height: 50px;
	width: 50px;
	background-color: black;
	display: inline-block;
	margin-left: 10px;
	color: white;
	font-size: 20px;
	text-align: center;
	line-height: 50px;
}
```
## 12.删除该行
<i class="icon-pencil"></i> 解析：

实现代码如下：
```html
<script type="text/javascript">

window.onload = function(){

	var oUl = document.getElementsByTagName('ul')[0];
	
	oUl.onclick = function(ev){
	
		var ev = ev || window.event;
		
		var target = ev.target || ev.srcElement;
		
		if(target.tagName.toLowerCase() == 'button'){
		
			var tr = target.parentNode;
			
			tr.parentNode.removeChild(tr);
			
		}
		
	}
	
}
</script>
<body> 
	<ul>
		<li>一<button>删除一</button></li>
		<li>二<button>删除二</button></li>
		<li>三<button>删除三</button></li>
		<li>四<button>删除四</button></li>
		<li>五<button>删除五</button></li>
		<li>六<button>删除六</button></li>
	</ul>
</body>
```
## 13.垂直水平居中
```css
div{
	width : 300px;
	height : 300px;
	border : 1px solid red;
	position : absolute;
	top : 50%;
	left : 50%;
	margin-top : -150px;
	margin-left : -150px;  
}
```
## 14.写一个traverse函数，输出所有页面宽度和高度大于50像素的节点。
```javascript
function traverse(oNode) {
    var aResult = [];
    oNode = oNode || document.body;
    if (oNode.style) {
        var nWidth = window.parseInt(oNode.style.width, 10) || 0;
        var nHeight = window.parseInt(oNode.style.height, 10) || 0;
        if (nWidth > 50 && nHeight > 50) {
            aResult.push(oNode);
        }
    }
    var aChildNodes = oNode.childNodes;
    if (aChildNodes.length > 0) {
        for (var i = 0, l = aChildNodes.length; i < l; i++) {
            var oTmp = aChildNodes[i];
            aResult = aResult.concat(traverse(oTmp));
        }
    }
    return aResult;
}
```
```javascript
function traverse(){
    return Array.prototype.filter.call(document.querySelectorAll('body *'), function(node){
        return node.offsetWidth > 50 && node.offsetHeight > 50;
    });
}
```


