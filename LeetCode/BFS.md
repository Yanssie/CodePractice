### 1.二叉树的层级遍历
Binary Tree Level Order Traversal，使用队列作为主要的数据结构
- Queue接口的方法：(FIFO) <br>
    - offer()添加
    - poll() 相当于pop
    - size() <br>
LinkedList是queue的实现
- Collections.reverse(list) 将数组顺序反向，改变原数组
- Treverse in Zigzag level order <br>
Use two stack(curr and next), cannot use queue. <br>
Use a boolean normalOrder.

### 2.Binary Tree Serialization 序列化
- Object to String <br>
应用场景：将内存中的数据写入硬盘；网络传输，接收方将字符串解析后存入内存；<br>
序列化手段：XML，JSON
- 算法 <br>
![image](https://github.com/Yanssie/CodePractice/blob/master/image/serial.png)
### 3.简单图求最短路径（每条边长度为1，且没有方向）
