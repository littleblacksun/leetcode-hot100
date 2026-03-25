<img width="748" height="525" alt="image" src="https://github.com/user-attachments/assets/f108936b-c948-4e04-a704-a4e9efdbe43b" />

此类问题属于是：在一个长数组中找一个连续短数组（或者短数组的总和），这样子的问题还是可以用前缀和的思想来解决

## 核心思路:前缀和+贪心

1、首先计算数组的前缀和（可以看做right指针）

2、用一个变量维护最小的前缀和（可以看做left指针）

3、**当前缀和-最小前缀和**得到最大，就说明left-right中这段范围的子数组的总和达到了最大

<img width="670" height="671" alt="image" src="https://github.com/user-attachments/assets/e19eae48-20a7-4215-a623-852b0283a755" />

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = Integer.MIN_VALUE;
        int minPreSum = 0;
        int preSum = 0;
        for (int x : nums){
            preSum += x;//计算当前前缀和
            ans = Math.max(ans,preSum-minPreSum); //减去前缀和的最小值
            minPreSum = Math.min(minPreSum,preSum);//维护前缀和的最小值
        }
        return ans;
    }
}
```

## 思路二：动态规划

f[i] 表示：以 nums[i] 这个元素结尾的所有连续子数组里，和最大的那一个的和。

1、如果nums就一个元素 那么 f = nums

2、如果nums超过一个元素 那么f[i] = max(f[i-1],0) + num[i] 

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] f = new int[nums.length];
        f[0] = nums[0];
        int ans = f[0];
        for (int i = 1; i < nums.length; i++) {
            f[i] = Math.max(f[i - 1], 0) + nums[i];
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```



