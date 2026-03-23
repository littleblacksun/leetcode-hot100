<img width="1231" height="726" alt="image" src="https://github.com/user-attachments/assets/b6f6890a-fbf2-4647-bbc0-631375dc4443" />

这一道题虽然也是双指针，但是略微不同于前面的双指针，这需要求**三数之和**，按照双指针的思想， 可以让其中一个数，进入遍历，另外两个数当做双指针

## 思路

为方便双指针及跳过相同元素，先把 nums 排序。

枚举nums[i],  那么target就是 找到nums[j]+nums[k] == -nums[i]

> 在外层循环中，如果发现 nums[i]=nums[i−1]，那么 nums[i] 与后面两个数组成的和为 0 的三元组，nums[i−1] 也能组成一模一样的三元组，这就重复了，所以遇到 nums[i]=nums[i−1] 的情况，直接 continue。

## 优化
### 优化一：
如果 nums[i] 与后面最小的两个数相加 nums[i]+nums[i+1]+nums[i+2]>0，那么后面不可能存在三数之和等于 0，break 外层循环。

### 优化二：
如果 nums[i] 与后面最大的两个数相加 nums[i]+nums[n−2]+nums[n−1]<0，那么内层循环不可能存在三数之和等于 0，但继续枚举，nums[i] 可以变大，所以后面还有机会找到三数之和等于 0，continue 外层循环。

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for(int i = 0;i < n-2;i++)
        {
            int x = nums[i];
            if(i>0&&x==nums[i-1]){
                continue;
            }
            if(x+nums[i+1]+nums[i+2]>0) break; //当前nums[i]和后面的两个之和都大于0，那么后面三数之和都会大于0，无需再遍历，直接跳出循环
            if(x+nums[n-2]+nums[n-1]<0) continue;

            int j = i+1;
            int k = n-1;//这里j和k是双指针
            while(j<k){
                int s = x + nums[j] + nums[k];
                if (s > 0)
                    k--;//总和大于0，大的减少
                else if (s < 0)
                    j++;//总和大于0，小的增大
                else{
                    //三数之和为0
                    ans.add(List.of(x,nums[j],nums[k]));
                    for (j++; j<k&&nums[j]==nums[j-1];j++);
                    for (k--; k>j&&nums[k]==nums[k+1];k--);//跳过重复数字
                }
            }

        }
        return ans;
    }

}
```
