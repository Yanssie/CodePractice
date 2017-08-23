时间复杂度：基本为T(n) = T(n/2) + O(1) = T(n/4) + 2O(1) = T(1) + O(logn) = O(logn) 
## 1.二分模版
Find any/first/last position of target in nums[]
```java
int start = 0, end = nums.length - 1;
while (start +1 < end) {
  int mid = start + (end - start) / 2;    (avoid overflow)
  if(nums[mid] == target) {}
}
```
## 2.XXOO
Find first/last 满足某条件的position
- eg. Search in a big sorted array <br>
int index = 1 <br>
index * 2 增大至 > target, 找到end = index - 1

- eg. Find minimum in a rotated sorted array <br>
target = 最后一个数，再二分

- eg. Search a 2D matrix 
**常规**： search a value in 严格递增的 matrix <br>
在第一列二分，找到target所在的row <br>
在该行二分，找到target所在的column <br>
**变形**：找到出现的次数，行、列分别递增 <br>
（非二分法） <br>
从左下角／右上角出发，分别比较target，每次可以排除一行／一列 <br>
遍历matrix，找到count <br>
## 3.Half Half 保留一半
