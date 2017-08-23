reference占4个字节 <br>
[!image](https://github.com/Yanssie/CodePractice/raw/master/image/LinedList.png)
- 使用dummy node时，定义完dummy.next = head; 一定要将dummy赋给一个ref （head或叫做node）
- reverse时
要注意reverse和connect<br>
reverse时用到两根指针
- 判断cycle时，或判断intersection时用到HashSet更容易 
- HashSet.add() 方法
- 可以用while(head.next != null), 但不可以用while(node.next != null)， （NullPointerException）
