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
- 【minimun subArray】<br>
min = prefixSum(j) - max(prefixSum(i));
```javascript
   public int minSubArray(List<Integer> nums) {
        // 这里是MAX_VALUE
        int min = Integer.MAX_VALUE;
        int sum = 0, maxsum = 0;
        int size = nums.size();
        for(int i = 0; i < size; i++) {
            sum += nums.get(i);
            min = Math.min(min, sum - maxsum);
            maxsum = Math.max(sum, maxsum);
        }
        return min;
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
- 【subArray Sum = 0】<br>
sum(i+1 ~ j) = prefixSum[j] - prefixSum[i] = 0; <br>
if prefixSum(j) = prefixSum(i) then  <br>
返回任意满足条件的subarray的起始点和结束点 return [i+1, j] <br>
判断时一定要加  **prefixSum.put(0, -1);**
```javascript
    public ArrayList<Integer> subarraySum(int[] nums) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        if(nums == null || nums.length == 0) {
            return result;
        }
        int l = nums.length;
        int sum = 0;
        HashMap<Integer, Integer> prefixSum = new HashMap<Integer, Integer>();
        // first is prefixsum, second is index
        // 这句话一定要有
        // 放入一个0 key，这样sum = 0的时候会触发else
        prefixSum.put(0, -1);
        for(int i = 0; i < l; i++) {
            sum += nums[i];
            if(!prefixSum.containsKey(sum)) {
                prefixSum.put(sum, i);
            } else{
                int start = prefixSum.get(sum);
                result.add(start + 1);
                result.add(i);
                return result;
            }
        }
        return result;
    }
```
- 【subArray Sum = k】返回满足条件的subarray个数 <br>
prefix(j) - prefix(i) = k <br>
存储prefix(j)及出现的次数到map中，检查map中是否存在prefix(j) - k <br>
如果存在，count += prefix(j)出现的次数 <br>
**将相同的key put到HashMap中，会覆盖** <br>
**map.getOrDefault(key, value)**
```javascript
    public int subarraySum(int[] nums, int k) {
        int count = 0;
        int sum = 0;
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        //一定要把0先put进去
        map.put(0, 1);
        for(int i = 0; i < nums.length; i++) {
            sum += nums[i];
            //如果前面出现sum-k，把sum-k在前面出现过的次数累加
            if(map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            //put相同的key到map中会覆盖
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
```
