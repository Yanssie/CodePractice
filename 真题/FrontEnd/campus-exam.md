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
## 4.事件流阶段，事件对象
###事件对象
- 访问event
`IE`: event对象作为window对象的一个属性存在<br>
`其他浏览器`：直接访问
```javascript
function(event){
    return event || window.event
}
```
- 取消冒泡：
`IE`: cancelBubble=true就可以取消冒泡<br>
`其他浏览器`: event.stopPropagation()就ok了
- 取消默认行为：
`IE`: returnValue =false
`其他浏览器`: event.preventDefault()
- 事件的目标元素：
`IE`: srcElement 来获取事件的目标元素
`其他浏览器`: target 来获取事件的目标元素
### 事件流阶段
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
## 15.css display属性
- block :　块对象的默认值。将对象强制作为块对象呈递，为对象之后添加新行(换行符)   可以定义高度和宽度
- none :　隐藏对象。与 visibility 属性的hidden值不同，其不为被隐藏的对象保留其物理空间
- inline :　内联对象的默认值。将对象强制作为内联对象呈递，从对象中删除行，元素前后没有换行符
- inline-block :　IE5.5 将对象呈递为内联对象，但是对象的内容作为块对象呈递。<br>
旁边的内联对象会被呈递在同一行内 
- inherit: 看display默认是不具备继承性的，使用inherit可以让其继承父对象的display属性。
## 16. css overflow属性
- 参数是scroll时候，必会出现滚动条。
- 参数是auto时候，子元素内容大于父元素时出现滚动条。
- 参数是visible时候，溢出的内容出现在父元素之外。
- 参数是hidden时候，溢出隐藏。
## 17. <a>的target属性
例如<a href="/XXXX"  target="_blank" >打开新的网页</a>
- _blank: 以新弹出一个浏览器窗口打开一个新链接页面；
- _self: 在原有的浏览器窗口打开新连接的页面；
- _parent: 在父框架集中打开被链接文档
- _top: 在整个窗口中打开被链接文档。
- 若<form>和<a>标签同时出现该属性，form中的target起作用；
## 18. flash js 交互
- Flash提供了ExternalInterface接口与JavaScript通信
- ExternalInterface有两个方法，call和addCallback，**call**的作用是让Flash调用js里的方法，**addCallback**是用来注册flash函数让js调用。
## 19.css position
- static: 没有定位，可以用position:static取消继承，即还原元素定位的默认值
- absolute: 生成绝对定位的元素，相对于 static 定位以外的第一个祖先元素进行定位。
- fixed: 相对于窗口的固定定位
- relative: 生成相对定位的元素，相对于元素本身正常位置进行定位。
## 20.浏览器模式
- 标准模式：浏览器根据规范呈现页面
- 混杂模式：页面以一种比较宽松的向后兼容的方式显示
DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现
浏览器根据DOCTYPE是否存在以及使用的哪种DTD来选择要使用的呈现方法
## 21.CSS sprites
- 允许你将一个页面涉及到的所有零星图片都包含到一张大图中去
- 利用CSS的“background-image”，“background-repeat”，“background-position”的组合进行背景定位
- CSS Sprites减少了总的图片的字节，很好地减少网页的http请求，从而大大的提高页面的性能
- CSS Sprites整理起来更为方便，同一个按钮不同状态的图片也不需要一个个切割出来并个别命名
## 22.浏览器内核引擎
- **WebKit**: Safari, Google Chrome,傲游3,猎豹浏览器,百度浏览器 opera浏览器<br>
私有属性：-webkit
- **Trident**: IE<br>
私有属性：-ms
- **Gecko**: Firefox <br>
私有属性：-moz
- 搜狗浏览器是双核的，双核并不是指一个页面由2个内核同时处理,而是所有网页（通常是标准通用标记语言的应用超文本标记语言）由webkit内核处理,只有银行网站用IE内核
## 23.HTML5语义化标签
- 结构元素
header: 网页或section的页眉<br>
footer: 页脚<br>
hgroup: 连续多个h1...h6<br>
nav: 用于主要导航部分<br>
aside: 在article内表示附属信息，在article外可做侧边栏<br>
section: 文章的节或段，article、nav、aside可以理解为特殊的section<br>
address: 代表区块容器，必须是作为联系信息出现，邮编地址、邮件地址等等,一般出现在footer<br>
## 24.css伪类选择器
伪类选择器之前只能有一个：
**a元素**
- 由于CSS优先级的关系(后面比前面的优先级高)，在写a的CSS时，一定要按照a:link， a:visited， a:hover， a:actived的顺序书写<br>
- 遵守一个“爱恨原则”LoVe/HAte, 也就是link-visited-hover-active.
**form元素**
:enabled 选择器选取所有启用的表单元素。<br>
:disabled 选择器选取所有禁用的表单元素。<br>
:checked 选择器选取所有选中的复选框或单选按钮。<br>
## 25.数组方法
- 不改变原数组的方法：concat()、slice()、join()、toString()、valueOf()
- 改变原数组的方法：pop() 、push()、reverse()、shift()、unshift()、sort()、splice()
1. array.length: 可读可写，如果设置的值比其当前值小，其底部的元素将被截断。<br>
如果设置的值比它的当前值大，新增的元素被添加到数组的尾部，并且值为 undefined。
2. arrayObject.slice(start,end): 不包括end
3. arrayObject.splice(index, howmany, element1, ….., elementX)
4. var a  = [1,2,3];<br>
 a.join(); // "1,2,3" <br>
 a.join('-') // "1-2-3"
5. sort()<br>
- 当不带参数调用 sort()方法时，数组元素以字母顺序排序
- 为了按照其他方式而非字母表顺序进行排序，必须给sort()方法传递一个比较函数。假设如果想让第一个参数在前，比较函数应该返回一个小于0的数
```javascript
var arr = [10,5,40,25,1000,1];
function sortNumber(a,b)
{
  return a - b;
}
console.log(arr);  //["10", "5", "40", "25", "1000", "1"]
console.log(arr.sort());  //["1", "10", "1000", "25", "40", "5"]
console.log(arr.sort(sortNumber));  //["1", "5", "10", "25", "40", "1000"]
```
- JavaScript里面，没有原生的方法来区别Array和Object，一般可以通过下面的方法来识别数组：
```javascript
var isArray = function(value){
  return Object.prototype.toString.apply(value) === '[object Array]';
}
```
## 26.正则表达式
在一串包含数字以及英文字母的字符串中中找出数字并保存在数组中。
```javascript
var arr = str.match(/\d+/g);
```
- 匹配模式
'i'：为 ignore case，即 忽略大小写。<br>
'g'：为 global search，即全局搜索。<br>
'm'：为 multi-line search，即多行搜索。<br>
- 支持正则表达式的 String 对象的方法
str.search(/Html5/i) <br>
str.match(/\d+/g)<br>
str.replace()
```javascript
var pattern1 = "JS,HTML5,CSS";
var replace1 = pattern1.replace(/(\w{1,}),(\w+)/, "$2,$1");
console.log(replace1);//HTML5,JS,CSS
```
## 27. substring 与 substr
string.substring(start,stop): 字符串 从 start 开始到 end-1 的所有字符。<br>
string.substr(start,length): 第一个代表起始位置，第二个代表截取的长度
## 28. 基本类型与引用类型
基本类型变量的值是保存在栈（stack）中的简单数据段，也就是说，它们的值直接存储在变量访问的位置 <br>
引用类型变量的值是保存在堆（heap）中的对象
## 29.Cookie
localStorage和Web Sqls是将数据保存在客户端，不跟服务端交互；<br>
而Cookie是页面跟服务器端互相传递信息<br>
浏览器保存的 cookie 是字符串 `typeof window.document.cookie //string`<br>
```javascript
window.document.cookie="name=guwei"; //名称
window.document.cookie="a=1; s=2; d=3;"; // 只有 a = 1 会被写入，值需要URI编码后处理
```
## 30.闭包
**内部函数的作用域链引用着外部函数活动变量**<br>
因为浏览器的自动垃圾回收机制，被内部函数的作用域链指针引用着的内部变量，会**一直存在与内存中**,不会被回收，这就是闭包的原理<br>
**存在原因**：<br>
JavaScript中因为有作用域链的存在，`在函数内部的变量在函数外部是无法访问的`，在函数内部却可以得到外部变量。但是有时因为程序的需要，我们希望在外部函数访问到函数内部的变量，这就需要创建一种特殊的闭包来解决这个难题。
```javascript
function outer() {
    var i = 5;
    return function inner() {
        alert(++i);
    }
}
var c = outer();
c();  //6
c();  //7
```
```javascript
var age = 21;
    var o = (function(){
        var age=23;
        return{
            setAge : function(){
                alert(++age);
            }
        }
    }());
    o.setAge(); //24
    o.setAge(); //25
```
结论：能创建新的作用域的就是闭包。而其中将内部函数返回，**函数外部只能通过内部返回的方法才能访问内部var变量的闭包**，能实现对内部var变量很好的封装，所以面试中经常会被问到此种闭包


