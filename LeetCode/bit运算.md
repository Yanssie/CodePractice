- bit 与
- bit 异或 ^
- bit 非 ～， ～2（0010）= -3（1101） <br>
如果求负数：~x+1
- 【Sum of two nums】<br>
```java
    public int getSum(int a, int b) {
        if(a == 0) return b;
        if(b == 0) return a;
        while(b != 0) {
            // 进位
            int carry = a & b;
            a = a ^ b;
            b = carry << 1;
        }
        return a;
    }
```
- 【Subtract of nums】
```java
public int getSubtract(int a, int b) {
	while (b != 0) {
		int borrow = (~a) & b;
		a = a ^ b;
		b = borrow << 1;
	}
	
	return a;
}
```
