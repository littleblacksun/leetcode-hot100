<img width="685" height="405" alt="image" src="https://github.com/user-attachments/assets/d566724e-269f-4274-b8df-d6ce7da1eb74" />

## 由于本题是连续子数组的问题，所以采用前缀和+哈希表来做

前缀和：用一个数组s来统计nums数组的前缀和

那么要求多少个子序列的和等于k的问题 == 有多少个s[j] - s[i](这个就可以看做子序列的和) == k?的问题

hash表的key是s[j] value为s[j]的前缀和的个数，也就是说：j向右遍历的同时，hash表中保存了s[j]出现过的次数

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        // 1. 构建前缀和数组 s，长度 n+1，s[0]=0
        int[] s = new int[n+1];
        for(int i = 0;i<n;i++){
            s[i+1] = s[i] + nums[i];
        }
        // 2. 创建哈希表：key=前缀和，value=这个和出现的次数
        Map<Integer,Integer> cnt = new HashMap<>(n+1,1);

        int ans = 0;// 答案：统计有多少个子数组
        // 3. 遍历每一个前缀和 sj
        for(int sj:s){
            // 核心：找之前出现过多少次 sj - k → 累加到答案
            ans += cnt.getOrDefault(sj-k,0);
            // 把当前前缀和 sj 放入哈希表（计数+1）
            cnt.merge(sj,1,Integer::sum);

        }
        return ans;
    }
}
```
**以上是两次遍历的写法**

**一次遍历也可以做到**

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>(nums.length + 1, 1); // 预分配空间
        cnt.put(0, 1); // s[0]=0 单独统计
        int s = 0;
        int ans = 0;
        for (int x : nums) {
            s += x;//这里就是nums的前缀和
            ans += cnt.getOrDefault(s - k, 0);//判断符合条件的前缀和出现次数
            cnt.merge(s, 1, Integer::sum); // cnt[s]++
        }
        return ans;
    }
}
```

> 在同一轮循环中，先把 s[i−1] 加入哈希表，再根据 s[i] 更新答案。
> 这样子写无需初始化cnt[0] = 1.

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>(nums.length, 1); // 预分配空间
        int s = 0;
        int ans = 0;
        for (int x : nums) {
            cnt.merge(s, 1, Integer::sum); // cnt[s]++
            s += x;
            ans += cnt.getOrDefault(s - k, 0);
        }
        return ans;
    }
}
```
