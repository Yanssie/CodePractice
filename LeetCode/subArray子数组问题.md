### 技巧：前缀和数组PrefixSum <br>
- 【Maximunm subArray】  <br>
max = prefixSum(j) - min(prefixSum(i));
```javascript
    public int maxSubArray(int[] nums) {
        // write your code here
        if(nums == null || nums.length == 0){
            return 0;
        }
        int sum = 0, minsum = 0;
        int max = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++) {
            sum += nums[i];
            max = Math.max(max, sum - minsum);
            minsum = Math.min(minsum, sum);
        }
        return max;
    }
```
- 【max two subArrays】<br>
两个不重叠的subArray的和最大，返回最大值 <br>
求出每个分界点的左边max subarray 和右边 maxsubarray，左右相加最大即max <br>
```javascript
    public int maxTwoSubArrays(List<Integer> nums) {
        // write your code here
        int size = nums.size();
        int[] left = new int[size];
        int[] right = new int[size];
        int max = Integer.MIN_VALUE;
        int sum = 0, minsum = 0;
        for(int i = 0; i < size; i++) {
            sum += nums.get(i);
            max = Math.max(max, sum - minsum);
            minsum = Math.min(sum, minsum);
            left[i] = max;
        }
        max = Integer.MIN_VALUE;
        sum = 0;
        minsum = 0;
        for(int i = size - 1; i >= 0; i--) {
            sum += nums.get(i);
            max = Math.max(max, sum - minsum);
            minsum = Math.min(minsum, sum);
            right[i] = max;
        }
        
        max = Integer.MIN_VALUE;
        for(int i = 0; i < size - 1; i++) {
            max = Math.max(max, left[i] + right[i+1]);
        }
        return max;
    }
```
- 【minimun subArray】<br>

- 【】
