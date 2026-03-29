<img width="858" height="604" alt="image" src="https://github.com/user-attachments/assets/c2888358-f4a9-4ca6-8dc3-ac48a4c7abbc" />

# 思路如下

· 定义pre[i] 代表 nums[0] - nums[i-1] 的乘积
· 定义suf[i] 代表 num[i+1] - num[n-1] 的乘积

计算num[i] 之外所有的数组的乘积，就等于计算 pre[i]*suf[i]

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] pre = new int[n];
        pre[0] = 1;
        for (int i = 1; i < n; i++) {
            pre[i] = pre[i - 1] * nums[i - 1];
        }

        int[] suf = new int[n];
        suf[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            suf[i] = suf[i + 1] * nums[i + 1];
        }

        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            ans[i] = pre[i] * suf[i];
        }
        return ans;
    }
}
```
