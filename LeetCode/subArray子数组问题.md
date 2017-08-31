### 技巧：前缀和数组PrefixSum <br>
【Maximunm subArray】  <br>
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
【minimun subArray】<br>

【】
